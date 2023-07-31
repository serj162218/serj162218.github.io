---
title: 'Golang Basic - Function'
date: 2023-05-10 20:36:03
tags: [Golang Basic]
published: true
hideInList: false
feature: 
isTop: false
---
[reference source](https://youtu.be/YS4e4q9oBaU?t=15690)
# Function
## example
```java
func test(a int, b *int) (int, bool, string) {
	return 5, true, "hello"
}
```

### params are same type

```java
func test(a, b int, c string) {
	
}
```

## Passing a pointer

Parameter can be a pointer. When a large object is passed in, the original operation will be to copy a new object, but if it is a pointer, it will save memory and speed. Be careful because it is passed in pointer, the original object will also be changed.

## variadic paramter

Turn the parameters passed in later into slices.

```java
func main() {
	sum("sum", 1, 2, 3, 4, 5)
}
func sum(a string, values ...int) {
	fmt.Println(a, values)
}

sum [1 2 3 4 5]
```

## 回傳pointer

When Go finds that the returned value is about to be released, Go will put this value into shared memory, so this value will not be released, and it may not have the same behavior in other languages.

```java
func main() {
	fmt.Println(*test())
}
func test() *int {
	result := 5
	return &result
}

5
```

## Name return value

Create a variable in the returned parameter, and then automatically specify this variable when returning, which is a return sugar.

```java
func main() {
	fmt.Println(test(1, 2, 3))
}
func test(values ...int) (result int) {
	for _, v := range values {
		result += v
	}
	return
}

6
```

## Return multiple values

It's very common in Go to return value and error

```java
func main() {
	d, err := divide(5.0, 0.0)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(d)
}

func divide(a, b float64) (float64, error) {
	if b == 0.0 {
		return 0.0, fmt.Errorf("cannot divide by zero")
	}
	return a / b, nil
}
```

## Passing through anonymous function

The change of the value inside will not affect the value outside, just like passing value to other function.

```java
func main() {
	for i := 0; i < 5; i++ {
		func(i int) {
			i++
			fmt.Println(i)
		}(i)
	}
}

1
2
3
4
5
```

If it is written in the following way, the function does not pass any parameters, but directly takes the nearest scope variable, so the changes made inside will change the outside value.

```java
func main() {
	for i := 0; i < 5; i++ {
		func() {
			i++
			fmt.Println(i)
		}()
	}
}

1
3
5
```

### Create anonymous function as variable

Put the anonymous function into the variable, and it can be passed to other functions

```java
func main() {
	f := func(i int) {
		i++
		fmt.Println(i)
	}
	for i := 0; i < 5; i++ {
		f(i)
	}
}
```

Another way

```java
func main() {
	var f func(int) = func(i int) {
		i++
		fmt.Println(i)
	}
	for i := 0; i < 5; i++ {
		f(i)
	}
}
```

With return

```java
func main() {
	var f func(int) int = func(i int) int {
		i++
		fmt.Println(i)
		return i
	}
	for i := 0; i < 5; i++ {
		f(i)
	}
}
```

### Pass in function, execute, return

Function with no return

```java
func main() {
	execFunc("sum", func(str string, value []int) {
		result := 0
		for _, i := range value {
			result += i
		}
		fmt.Println(result)
	}, 1, 2, 3, 4, 5)

}
func execFunc(str string, f func(str string, value []int), value ...int) {
	f(str, value)
}
```

Function with return parameters.

```java
func main() {
	value, err := execFunc("sum", func(str string, value []int) (int, error) {
		if len(value) == 0 {
			return 0, fmt.Errorf("length of values must more than 0")
		}
		result := 0
		for _, i := range value {
			result += i
		}
		return result, nil
	}, 1, 2, 3, 4, 5)
	if err != nil {
		fmt.Println(err)
		return
	}
	fmt.Println(value)

}
func execFunc(str string, f func(str string, value []int) (int, error), value ...int) (int, error) {
	return f(str, value)
}

15
```

## Method

```java
func main() {
	g := greeter{
		greeting: "Hello,",
		name:     "John",
	}
	g.greet()
}

type greeter struct {
	greeting string
	name     string
}

func (g greeter) greet() {
	fmt.Println(g.greeting, g.name)
}

Hello, John
```

At line 14, the operation is the copy of the object, not the object itself, so even if it is modified, it will not affect the original value.

```java
func (g greeter) greet() {
	fmt.Println(g.greeting, g.name)
	g.name = "Amy" //ineffective assignment to field greeter.name
}
```

Of course, the parameter passed in line 14 can also be a pointer, which will affect the original value.

```java
func main() {
	g := greeter{
		greeting: "Hello,",
		name:     "John",
	}
	g.greet()
	g.greet()
}

type greeter struct {
	greeting string
	name     string
}

func (g *greeter) greet() {
	fmt.Println(g.greeting, g.name)
	g.name = ""
}

Hello, John
Hello,
```