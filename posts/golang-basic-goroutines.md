---
title: 'Golang Basic - GoRoutines'
date: 2023-05-10 20:42:41
tags: [Golang Basic]
published: true
hideInList: false
feature: 
isTop: false
---
[reference source](https://youtu.be/YS4e4q9oBaU?t=20037)
# GoRoutines
Concurrent and Parallel
## Creating goroutines
A simple example, time.Sleep is needed because main() ends too soon for the goroutines to print

```java
func main() {
	go sayHello()
	time.Sleep(100 * time.Microsecond)
}
func sayHello() {
	fmt.Println("hello")
}

hello
```

Goroutines can also be used in anonymous functions, `msg` will use the nearest variable

```java
func main() {
	var msg = "Hello"
	go func() {
		fmt.Println(msg)
	}()
	time.Sleep(100 * time.Microsecond)
}

Hello
```
### Race condition
The main function is executed too fast and the resource of msg is changed, so that goroutines will also be affected.

```java
func main() {
	var msg = "Hello"
	go func() {
		fmt.Println(msg)
	}()
	msg = "goodnight"
	time.Sleep(100 * time.Microsecond)
}

goodnight
```

## Synchronization

### WaitGroups

Basic usage, `wg.Done()` will automatically minus the counter with one, `wg.Add(1)` will add one to the counter, `wg.Wait()` will pause until the counter is zero

```java
func main() {
	var msg = "Hello"
	wg.Add(1)
	go func() {
		fmt.Println(msg)
		wg.Done()
	}()
	wg.Wait()
	msg = "goodnight"
}
```

From the following example, although WaitGroup is useful, the execution is still a mess. The only improvement point is that the output is ordered.

```java
var wg = sync.WaitGroup{}
var counter = 0

func main() {
	for i := 0; i < 10; i++ {
		wg.Add(2)
		go sayHello()
		go increment()
	}
	wg.Wait()
}

func sayHello() {
	fmt.Println(counter)
	wg.Done()
}

func increment() {
	counter++
	wg.Done()
}

0
2
4
5
6
7
8
9
10
10
```

### Mutexes (Lock)

Although the lock is added, because each thread is still executed, the result will still change.

```java
var wg = sync.WaitGroup{}
var counter = 0
var m = sync.RWMutex{}

func main() {
	for i := 0; i < 10; i++ {
		wg.Add(2)
		go sayHello()
		go increment()
	}
	wg.Wait()
}

func sayHello() {
	m.RLock()
	fmt.Println(counter)
	m.RUnlock()
	wg.Done()
}

func increment() {
	m.Lock()
	counter++
	m.Unlock()
	wg.Done()
}

0
1
1
1
1
1
1
1
1
1
```

After improvement, the results are finally in order.

```java
var wg = sync.WaitGroup{}
var counter = 0
var m = sync.RWMutex{}

func main() {
	for i := 0; i < 10; i++ {
		wg.Add(2)
		m.RLock()
		go sayHello()
		m.Lock()
		go increment()
	}
	wg.Wait()
}

func sayHello() {
	fmt.Println(counter)
	m.RUnlock()
	wg.Done()
}

func increment() {
	counter++
	m.Unlock()
	wg.Done()
}

0
1
2
3
4
5
6
7
8
9
```

This is an exercise purely as an example, not a good exercise, because the advantages of parallelism and concurrency are gone, and the execution speed and resources are even worse.

### `runtime.GOMAXPROCS()`

```java
func main() {
	fmt.Println(runtime.GOMAXPROCS(-1))
}

20
```

`runtime.GOMAXPROCS(n)` is adjusted to use n threads, -1 means do not adjust and return the currently set thread nums

```java
func main() {
	runtime.GOMAXPROCS(30)
	fmt.Println(runtime.GOMAXPROCS(-1))
}

30
```

If n = 1, it is a single thread

```java
func main() {
	runtime.GOMAXPROCS(1)
	fmt.Println(runtime.GOMAXPROCS(-1))
}
```

Because it is a single thread, so everything is in order, after the above example is added, it will follow the order obediently. This purpose is to ensure the order is correct to avoid any race condition.

```java
func main() {
	runtime.GOMAXPROCS(1)
	for i := 0; i < 10; i++ {
		wg.Add(2)
		go sayHello()
		go increment()
	}
	wg.Wait()
}

func sayHello() {
	fmt.Println(counter)
	wg.Done()
}

func increment() {
	counter++
	wg.Done()
}

1
2
3
4
5
6
7
8
9
10
```

## Best Practices

- Don’t create goroutines in libraries
    - Let consumer control concurrency
- When creating a goroutine, know how it will end
    - Avoids subtle memory leaks
- Check for race conditions at compile time (need to use cgo, I did not open that.)