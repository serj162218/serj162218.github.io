---
title: 'Golang Basic - Constants'
date: 2023-05-10 20:14:31
tags: [Golang Basic]
published: true
hideInList: false
feature: 
isTop: false
---
[reference source](https://youtu.be/YS4e4q9oBaU?t=5189)
# Constants

## Declaration

```java
func main() {
	const myConst ...
	//Not MYCONST because it will be exported (variable name rules)
}
```

It won't get error if you declare a const variable then not use it.

```java
func main() {
	const a = 5
	const b = 6
	fmt.Printf("%v %T\n", a, a)
}
//no error
```

## Type Constant

const is like var, but const cannot be modified

```java
func main() {
	const myConst int = 42
	fmt.Printf("%v %T\n", myConst, myConst)
	myConst = 66 //cannot assign to myConst (constant 42 of type int)
}
```

If the value of constant is defined at runtime, it cannot be declared

```java
func main() {
	const myConst float64 = math.Sin(1.57) //math.Sin(1.57) (value of type float64) is not constant
	const b int = test() //b() (value of type int) is not constant
	fmt.Printf("%v %T\n", myConst, myConst)
}
func test() int {
	return 30
}
```

Array (collection) is mutable, so it cannot be declared as constant
Constant can be shadowed

```java
const a int16 = 33
const b int16 = 31

func main() {
	const a int8 = 3
	var b int8 = 5
	fmt.Printf("%v %T\n", a, a) //3 int8
	fmt.Printf("%v %T\n", b, b) //5 int8
}
```

There is actually no need to add a type (non-declaration) when const is declared

```java
func main() {
	const a = 3
	fmt.Printf("%v %T\n", a, a) //3 int
}
```

What's interesting about non-declaration is that although different types of integer can't be added together, the compiler will judge by itself and change the type.
This behavior is what var does not have in non-declaration.

```java
func main() {
	const a = 3
	var b int16 = 688
	fmt.Printf("%v %T\n", a+b, a+b) //691 int16
}
```

The compiler will automatically convert type.

```java
func main() {
	const a = 42
	var b int16 = 688
	var c int8 = 55
	fmt.Printf("%v %T\n", a+b, a+b) //730 int16
	fmt.Printf("%v %T\n", a+c, a+c) //97 int8
}
```

counterexample

```java
func main() {
	const a int8 = 3 //Declare the type first.
	var b int16 = 688
	fmt.Printf("%v %T\n", a+b, a+b) //invalid operation: a + b (mismatched types int8 and int16)
}
```

The difference from var is that when using a block, if the next variable has no assigned value, the value assigned above will be used

```java
const (
	a = 5
	b = "hello"
	c
)

func main() {
	fmt.Printf("%v %T\n", c, c) //hello string
}
```

## enumerated constant

iota, which can only be used for constant, is an enumeration constant (counter), starting with 0, and will be +1 every time it is declared in the constant block

Does not overlap between different constant blocks

```java
const a = iota
const (
	b = iota
	c
)
const (
	d = iota
)

func main() {
	fmt.Printf("%v %T\n", a, a) //0 int
	fmt.Printf("%v %T\n", b, b) //0 int
	fmt.Printf("%v %T\n", c, c) //1 int
	fmt.Printf("%v %T\n", d, d) //0 int
}
```

iota can use addition, subtraction, multiplication and division to do offset

```java
const (
	a = iota + 2
	b //iota + 2
	c = iota - 2
	d //iota - 2
)

func main() {
	fmt.Printf("%v\n", a) //2
	fmt.Printf("%v\n", b) //3
	fmt.Printf("%v\n", c) //0
	fmt.Printf("%v\n", d) //1
}
```

## The case of using iota

### Give constants to different states and check them in the program

```java
const (
	_ = iota
	normal
	hard
	veryHard
)

func main() {
	var difficulty int = normal
		if difficulty == normal {
			//do something in normal difficulty
		}
}
```

Beware that iota starts from zero. If the first constant is not declared as _ (directly discarded), it usually declare as error variable.

```java
const (
	err = iota
	normal
	hard
	veryHard
)

func main() {
	var difficulty int
	if difficulty == error {
		fmt.Println("error because difficulty is zero-value")
		//this line will print out
	}
}
```

### bit shifting with iota

```java
const (
	_  = iota
	KB = 1 << (10 * iota)
	MB
	GB
)

func main() {
	fileSize := 4000000000.
	fmt.Printf("%.2fGB", fileSize/GB) //3.73GB
}
```

### Boolean flags for Storing Permissions

```java
const (
	isAdmin = 1 << iota
	isMember
	isVisitor

	canSeeA
	canSeeB
	canSeeC
)

func main() {
	var role byte = isAdmin | canSeeA | canSeeB
	fmt.Printf("%b\n", role) //11001
	if role&isAdmin == isAdmin {
			fmt.Printf("isAdmin") //will print out
	}
}
```