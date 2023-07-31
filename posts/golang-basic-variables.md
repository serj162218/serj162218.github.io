---
title: 'Golang Basic - Variables'
date: 2023-05-10 21:25:31
tags: [Golang Basic]
published: true
hideInList: false
feature: 
isTop: false
---
[reference source](https://youtu.be/YS4e4q9oBaU?t=2148)
# Variables
## Declare
```java
func main(){
	//you can use this three method above in the function.
	var i int = 5
	var j int
	j = 8
	k := 10 //non-declaration
	var (
		m int
		n int = 22
	)
	m = 8
	fmt.Printf("%v %v %v %v %v", i, j, k, m, n)
}

//You cannot use non-declartion outside the function.
//x := 10

//You cannot split var j int = 5 into two lines.
//var j int
//j = 5
```

## _ (underscore)

When you encounter some function that returns a value, or other situations, you can use underscore when you don't want to use this variable.


```java
func main() {
	var _ int = 5
}
//no error
```

## shadowing

Variable declaration priority will take the closest scope

```java
func main() {
	var i int = 15
	fmt.Printf("%v", i) //15
	otherFunc()         //10

	func() {
		fmt.Printf("%v", i) //15
		var i int = 30
		fmt.Printf("%v", i) //30
	}()
}
func otherFunc() {
	fmt.Printf("%v", i)
}
```

## Always have to be used

```java
func main() {
	var i int = 42
	//i declared and not used
}
```

## Name Rules

First Letter of variable is Uppercase means export it to other packages.

```java
var (
	accountName int //global only for this package
	SeasonNum int //can be used outside this package
)

//global only for this package
func privateFunc() {

}

//can be used outside this package
func PublicFunc() {

}
```

## Type conversion

```java
func main() {
	i := 88
	fmt.Printf("%v", string(i)) //X, integer to ascii
	j := 55.22
	fmt.Printf("%v", int(j)) //55
	//Bewares float cannot be changed to string
	//fmt.Printf("%v", string(j))
}
```

### String to Integer directly

```java
import (
	"strconv"
)

func main() {
	i := 85
	fmt.Printf("%v %T", string(i)) //U string
	fmt.Printf("%v %T", strconv.Itoa(i)) //85 string

	j := "66"
	k, err := strconv.ParseInt(j, 10, 32)
	if err != nil {
		panic(err)
	}
	fmt.Printf("%v %T", k, k)
}
```

### Zero-Value

In Go, every time initialize a value, it has a zero value.

```java
func main() {
	var i int
	var n bool
	var k string
	p := k == ""
	fmt.Printf("%v %T , %v %T , %v %T , %v", i, i, n, n, k, k, p)
}

0 int, false bool,  string, true
```