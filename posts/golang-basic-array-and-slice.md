---
title: 'Golang Basic - Array & Slice'
date: 2023-05-10 19:52:35
tags: [Golang Basic]
published: true
hideInList: false
feature: 
isTop: false
---
[reference source](https://youtu.be/YS4e4q9oBaU?t=6473)
# Array

## Can only store one type of data

```java
func main() {
	grades := [4]int{95, 88, 37}
	var names [3]string
	names[0] = "John"
	fmt.Printf("%v", grades) //[95 88 37 0]
	fmt.Printf("%v", names) //[John  ]
}
```

## non-declaration can also use like this.

```java
func main() {
	grades := [...]int{97, 85, 93}
	fmt.Printf("%v", grades) //[97 85 93]
}
```

## len(): Length of array (btw, length of string is the same function)

```java
func main() {
	grades := [4]int{97, 85, 93}
	fmt.Printf("%v %v", len(grades)) //4
}
```

## two-dimension array

```java
func main() {
	var names [3]string = [3]string{"John", "Smith", "Jenny"}
	var grades [3][1]int = [3][1]int{[1]int{13}, [1]int{22}, [1]int{67}}
	fmt.Printf("%v %v", names, grades) //[John Smith Jenny] [[13] [22] [67]]
}
```

## Copy array in go is different with other languages. It’s deep copy.

```java
func main() {
	a := [...]int{1, 2, 3}
	b := a
	b[1] = 4
	fmt.Printf("%v %v", a, b) //[1 2 3] [1 4 3]
}
```

## Only if assign by pointer, it’s acting like shallow copy

```java
func main() {
	a := [...]int{1, 2, 3}
	b := &a
	b[0] = 4
	fmt.Printf("%v %v %v", a, b, *b) //[4 2 3] &[4 2 3] [4 2 3]
}
```

# Slice

*arraylist

## It is very similar to Array, but there is no need to preset the length when creating

```java
func main() {
	a := []int{1, 2, 3}
	fmt.Printf("%v", a) //[1 2 3]
}
```

## In most cases, what Array can do, Slice can do the same

```java
func main() {
	a := []int{1, 2, 3}
	fmt.Printf("%v %v", a, len(a)) //[1 2 3] 3
}
```

## Slice is shallow copy, which is different from Array

```java
func main() {
	a := []int{1, 2, 3}
	b := a
	b[1] = 5
	fmt.Printf("%v %v", a, b) //[1 5 3] [1 5 3]
}
```

## cap(): Take the capacity of the slice, which is usually similar to len()

```java
func main() {
	a := []int{1, 2, 3, 4}
	fmt.Printf("%v %v", len(a), cap(a)) //4 4
}
```

## Slice operation, just like slice() in other languages. Array can also use slice operation, but both of them are shallow copy.

```java
func main() {
	a := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
	b := a[:] //all
	c := a[3:] //from 4th
	d := a[:6] //first 6 elements
	e := a[3:6] //from 4th, to 6th
	fmt.Println(a) //[1 2 3 4 5 6 7 8 9 10]
	fmt.Println(b) //[1 2 3 4 5 6 7 8 9 10]
	fmt.Println(c) //[4 5 6 7 8 9 10]
	fmt.Println(d) //[1 2 3 4 5 6]
	fmt.Println(e) //[4 5 6]
	a[5] = 11
	fmt.Println(d) //[1 2 3 4 5 11]
}
```

## make(), param first is the type, second is the len, third is the capacity.

```java
func main() {
	a := make([]int, 3, 100)
	fmt.Println(a) //[0 0 0]
	fmt.Printf("len: %v\n", len(a)) //3
	fmt.Printf("cap: %v\n", cap(a)) //100
}
```

Slice is like an Array with no length limit. The capacity means that when Slice adds elements to exceed this capacity, it will automatically move all the elements to the new memory.

If there are too many elements in the slice, this operation is very expensive, so that when using make() to declare, if you know that the slice has more elements, you will set a higher number for the capacity

```java
func main() {
	a := []int{}
	fmt.Printf("%v\n", len(a)) //0
	fmt.Printf("%v\n", cap(a)) //0
	a = append(a, 1, 2, 3)
	fmt.Printf("%v\n", len(a)) //3
	fmt.Printf("%v\n", cap(a)) //3 (different compile may not be the same)
}
```

```java
func main() {
	a := make([]int, 0, 5)
	fmt.Printf("%v\n", len(a)) //0
	fmt.Printf("%v\n", cap(a)) //5
	a = append(a, 1, 2, 3)
	fmt.Printf("%v\n", len(a)) //3
	fmt.Printf("%v\n", cap(a)) //5
}
```

# Spread Operater

```java
func main() {
	a := make([]int, 0, 5)
	a = append(a, 1)
	a = append(a, []int{2, 3, 4, 5}...) //spead operater, will unpacked the array or slice
	fmt.Println(a)
}
```