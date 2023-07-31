---
title: 'Golang Basic - Pointers'
date: 2023-05-10 21:09:47
tags: [Golang Basic]
published: true
hideInList: false
feature: 
isTop: false
---
[reference source](https://youtu.be/YS4e4q9oBaU?t=14637)
# Pointers
## example
```java
func main() {
	var a int = 45
	var b *int = &a
	fmt.Println(a, *b)
}

45 45
```

## Use pointers to manipulate elements in array

```java
func main() {
	a := [3]int{1, 2, 3}
	b := &a[0]
	c := &a[1]
	fmt.Printf("%v %p %p", a, b, c)
}

[1 2 3] 0xc000018180 0xc000018188
```

### In go, you cannot access the memory by mathematical operations.

```java
func main() {
	a := [3]int{1, 2, 3}
	b := &a[0]
	c := &a[1] - 8
	fmt.Printf("%v %p %p", a, b, c)
}

invalid operation: &a[1] - 8 (mismatched types *int and untyped int)
```

### It is also possible to create pointers to objects

```java
func main() {
	var ms *myStruct = &myStruct{foo: 43}
	fmt.Println(ms)
}

type myStruct struct {
	foo int
}

&{43}
```

`new()` cannot specify a given value at creation time.

```java
func main() {
	var ms *myStruct = new(myStruct)
	fmt.Println(ms)
}

type myStruct struct {
	foo int
}

&{0}
```

## The pointer's  initial value is nil, so remember to check first avoiding runtime error.

```java
func main() {
	var ms *int
	fmt.Println(ms) 
}

<nil>
```

### The method to get the value from the pointer object, line 4 is a syntax sugar.

```java
func main() {
	var ms *myStruct = &myStruct{foo: 32}
	fmt.Println((*ms).foo)
	fmt.Println(ms.foo)
}

32
32
```

## array vs slice

Because array is a fixed length, it stores data directly.
But the slice is the underlying projection of the array, which stores a pointer to the first element of the underlying array
So when copying, array is deep copy, but slice is shallow copy

## Map

Like Slice, a map stores a pointer to the first element of the underlying array.