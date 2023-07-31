---
title: 'Golang Basic - Channel'
date: 2023-05-10 19:57:52
tags: [Golang Basic]
published: true
hideInList: false
feature: 
isTop: false
---
[reference source](https://youtu.be/YS4e4q9oBaU?t=21910)
# Channel

For communicating data between goroutines

## Example

`i := ← ch`Get the first element in the channel

`ch ← 5`Push elements into the channel

`ch := make(chan int)`Create an integer channel with a space of 1

Channel can be specified as any type, even struct, but after specifying, the channel can only hold this type, which is like an array
A space of 1 means that no new elements can be placed in the channel before the elements in the channel have been discharged or you will get an out of index error.

```java
var wg = sync.WaitGroup{}

func main() {
	ch := make(chan int)
	wg.Add(2)
	go func() {
		i := <-ch
		fmt.Println(i)
		wg.Done()
	}()
	go func() {
		ch <- 5
		wg.Done()
	}()
	wg.Wait()
}
```

If you want to put multiple elements in, you can directly specify the size of the space when making

```java
var wg = sync.WaitGroup{}

func main() {
	ch := make(chan int, 10)
	wg.Add(2)
	go func() {
		i := <-ch
		fmt.Println(i)
		wg.Done()
	}()
	go func() {
		ch <- 5
		ch <- 3 //no error
		wg.Done()
	}()
	wg.Wait()
}
```

## Receive / Send

In goroutines, channels can be received and sent, but there will be order problems
line 7, i is waiting for elements in the channel. After line 13 put a element in, line 8 will be executed.
line 14 is also waiting for elements in the channel. After line 9 put a element in, line 14 will be executed.

```java
var wg = sync.WaitGroup{}

func main() {
	ch := make(chan int)
	wg.Add(2)
	go func() {
		i := <-ch
		fmt.Println("from 15", i)
		ch <- 2
		wg.Done()
	}()
	go func() {
		ch <- 1
		fmt.Println("from 21", <-ch)
		wg.Done()
	}()
	wg.Wait()
}

from 15 1
from 21 2
```

You can specify whether the goroutines are send on channel or receive only channel to avoid the above problems.

```java
var wg = sync.WaitGroup{}

func main() {
	ch := make(chan int)
	wg.Add(2)
	//receive only channel
	go func(ch <-chan int) {
		i := <-ch
		fmt.Println(i)
		wg.Done()
	}(ch)
	//send only channel
	go func(ch chan<- int) {
		ch <- 1
		wg.Done()
	}(ch)
	wg.Wait()
}
```

## for loop with channel

Use `for := range ch` to loop the channel, when there is a new element in the channel, it will be executed immediately, and it will end when the channel is closed.
`close(ch)` means to close the channel. After closing the channel, there is no way to reopen it, and no more elements can be placed.

```java
var wg = sync.WaitGroup{}

func main() {
	ch := make(chan int)
	wg.Add(2)
	go func(ch <-chan int) {
		for i := range ch {
			fmt.Println(i)
		}
		wg.Done()
	}(ch)
	go func(ch chan<- int) {
		for i := 0; i < 10; i++ {
			ch <- i
		}
		close(ch)
		wg.Done()
	}(ch)
	wg.Wait()
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

If you can't loop channel directly, you can rewrite it as follows

```java
var wg = sync.WaitGroup{}

func main() {
	ch := make(chan int)
	wg.Add(2)
	go func(ch <-chan int) {
		for {
			//do something else
			if i, ok := <-ch; ok {
				fmt.Println(i)
			} else {
				break
			}
		}
		wg.Done()
	}(ch)
	go func(ch chan<- int) {
		for i := 0; i < 3; i++ {
			ch <- i
		}
		close(ch)
		wg.Done()
	}(ch)
	wg.Wait()
}

0
1
2
```

## Select and struct{}

The above methods must close the channel to end the loop.
If you don't want to end the loop by closing the channel, you can use the second method instead
`select` means that if one of ch and doneCh has an element, it will be executed first. Otherwise, if there is none, it will block.
Use the flag method to break the outer loop

`doneCh ← struct{}{}`struct{} is the type, and the following {} is the way of creation. Just imagine it as an common obj{}

The advantage of `struct{}` is that it does not occupy any memory, it is better than boolean, and it is specially used to detect whether to send a message

```java
var wg = sync.WaitGroup{}

func main() {
	ch := make(chan int)
	doneCh := make(chan struct{})
	wg.Add(2)
	go func(ch <-chan int) {
	LOOP:
		for {
			select {
			case entry := <-ch:
				fmt.Println(entry)
			case <-doneCh:
				break LOOP
			}
		}
		wg.Done()
	}(ch)
	go func(ch chan<- int) {
		for i := 0; i < 3; i++ {
			ch <- i
		}
		doneCh <- struct{}{}
		wg.Done()
	}(ch)
	wg.Wait()
}

0
1
2
```

## Example 2 with closing channel

Goroutines will be forced to close after the execution of main function. For a safe and smooth shutdown, the following method can be used.

```java
type logEntry struct {
	time     time.Time
	severity string
	message  string
}

var logCh = make(chan logEntry, 50)

func main() {
	go logger()

	logCh <- logEntry{time.Now(), "NOTICE", "App starting"}
	logCh <- logEntry{time.Now(), "WARNING", "App finishing"}

	time.Sleep(100 * time.Millisecond)
}

func logger() {
	for entry := range logCh {
		fmt.Printf("%v - [%v]%v\n", entry.time.Format("2006-01-02T15:04P:05"), entry.severity, entry.message)
	}
}

2023-04-14T14:37P:26 - [NOTICE]App starting
2023-04-14T14:37P:26 - [WARNING]App finishing
```

### 1. defer

```java
func main() {
	go logger()
	defer func() {
		close(logCh)
	}()
	logCh <- logEntry{time.Now(), "NOTICE", "App starting"}
	logCh <- logEntry{time.Now(), "WARNING", "App finishing"}

	time.Sleep(100 * time.Millisecond)
}

func logger() {
	for entry := range logCh {
		fmt.Printf("%v - [%v]%v\n", entry.time.Format("2006-01-02T15:04P:05"), entry.severity, entry.message)
	}
}
```

### 2. Select and struct{}

```java
var logCh = make(chan logEntry, 50)
var doneCh = make(chan struct{})

func main() {
	go logger()
	logCh <- logEntry{time.Now(), "NOTICE", "App starting"}
	logCh <- logEntry{time.Now(), "WARNING", "App finishing"}
	time.Sleep(100 * time.Millisecond)
	doneCh <- struct{}{}
}

func logger() {
Loop:
	for {
		select {
		case entry := <-logCh:
			fmt.Printf("%v - [%v]%v\n", entry.time.Format("2006-01-02T15:04P:05"), entry.severity, entry.message)
		case <-doneCh:
			break Loop
		}
	}
}
```