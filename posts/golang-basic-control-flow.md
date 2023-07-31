---
title: 'Golang Basic - Control Flow'
date: 2023-05-10 20:24:04
tags: [Golang Basic]
published: true
hideInList: false
feature: 
isTop: false
---
[reference source](https://youtu.be/YS4e4q9oBaU?t=13294)
# Control Flow

## Defer

### Do it until it is time to return.

```java
func main() {
	fmt.Println("start")
	defer fmt.Println("middle")
	fmt.Println("end")
}

start
end
middle
```

### defer is LIFO, just like stack

```java
func main() {
	defer fmt.Println("start")
	defer fmt.Println("middle")
	defer fmt.Println("end")
}

end
middle
start
```

Because defer is not executed until the end of the function, if it is used in the loop, it may cause a large amount of memory to be unable to be released until the end of the function, not the end of the loop.

```java
func main() {
	for i := 0; i < 2; i++ {
		defer fmt.Println("defer", i)
		fmt.Println("normal", i)
	}
	fmt.Println("AA")
}

normal 0
normal 1
AA
defer 1
defer 0
```

If you want to use defer functions in the loop, such as releasing resources after fetch api, you can put them in a new function.

```java
func main() {
	for i := 0; i < 2; i++ {
		test(i)
	}
	fmt.Println("AA")
}
func test(i int) {
	defer fmt.Println("defer", i)
	fmt.Println("normal", i)
}

normal 0
defer 0
normal 1
defer 1
AA
```

### The variables in defer are taken at the moment, not before return

```java
func main() {
		a := 1
		defer fmt.Println(a)
		a = 2
}

1
```

## Panic (exception)
Panic is not necessarily a fatal error, as long as the application knows how to deal with panic, otherwise the program will be stopped.

```java
func main() {
	fmt.Println("start")
	panic("nono")
	fmt.Println("end")
}

start
panic: nono
```

### Interaction between panic and defer

defer will be executed before return, and panic will cause the application to end, so defer will be executed before panic.

```java
func main() {
	fmt.Println("start")
	defer fmt.Println("deffered")
	panic("nono")
	fmt.Println("end")
}

start
deffered
panic: nono
```

## Recover

Before reading recover, you must first know how to write anonymous functions

### callback & anonymous function

func(){...}() is an anonymous function, the front is written as func (like callback), and the back () means to be executed

```java
func main() {
	test(func() { fmt.Println(123) })
}
func test(a func()) {
	a()
}

123
```

### Now use recover() inside the anonymous function

Because defer will be executed before panic, it means that Exception has already occurred. If there is no exception, recover will return nil.

```java
func main() {
	fmt.Println("start")
	defer func() {
		if err := recover(); err != nil {
			fmt.Println("Error:", err)
		}
	}()
	panic("nono")
	fmt.Println("end")
}

start
Error: nono
```

### stack function

As long as the panic has a recover to catch it, the higher-level function will not interrupt the execution due to the panic, but the function of the panic itself will be interrupted.

```java
func main() {
	fmt.Println("start")
	test()
	fmt.Println("end")
}
func test() {
	fmt.Println("start test")
	defer func() {
		if err := recover(); err != nil {
			fmt.Println("Error:", err)
		}
	}()
	panic("nono")
	fmt.Println("end test")
}

start
start test
Error: nono
end
```

### Throw new exception in recover

If the error cannot be handled with recover(), you can use panic() again and throw the exception to the upper function.

```java
func main() {
	fmt.Println("start")
	defer func() {
		if err := recover(); err != nil {
			fmt.Println("Error from test:", err)
		}
	}()
	test()
	fmt.Println("end")
}
func test() {
	fmt.Println("start test")
	defer func() {
		if err := recover(); err != nil {
			fmt.Println("Error:", err)
			panic(err)
		}
	}()
	panic("nono")
	fmt.Println("end test")
}

start
start test
Error: nono
Error from test: nono
```