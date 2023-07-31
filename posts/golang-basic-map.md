---
title: 'Golang Basic - Map'
date: 2023-05-10 21:05:40
tags: [Golang Basic]
published: true
hideInList: false
feature: 
isTop: false
---
[reference source](https://youtu.be/YS4e4q9oBaU?t=8240)
# Map

*hashset*

```java
func main() {
	classPopulations := map[string]int{
		"A": 33,
		"B": 41,
		"C": 37,
	}
	fmt.Println(classPopulations) //map[A:33 B:41 C:37]
}
```

Most types (such as array) can be used as keys, but a few types (such as slices) cannot.

```java
func main() {
	m := map[[3]int]string{}
	fmt.Println(m)//map[]
}
```

You can also create a map with make()

```java
func main() {
	classPopulations := make(map[string]int)
	classPopulations = map[string]int{
		"A": 33,
		"B": 41, 
		"C": 37,
	}
	fmt.Println(classPopulations)
}
```

You can directly add key-value into it

```java
func main() {
	classPopulations := make(map[string]int)
	classPopulations = map[string]int{
		"A": 33,
		"B": 41,
		"C": 37,
	}
	classPopulations["D"] = 55
	fmt.Println(classPopulations["D"]) //55
}
```

The order of the map is not guaranteed unlike array or slice.

`delete()` delete the key-value according to the key

```java
func main() {
	classPopulations := make(map[string]int)
	classPopulations = map[string]int{
		"A": 33,
		"B": 41,
		"C": 37,
	}
	delete(classPopulations, "A")
	fmt.Println(classPopulations)
}
```

If the key does not exist in the map, it will return (zero-value false).

```java
func main() {
	classPopulations := make(map[string]int)
	classPopulations = map[string]int{
		"A": 33,
		"B": 41,
		"C": 37,
	}
	pop, ok := classPopulations["D"]
	fmt.Println(pop, ok) //0 false
}
```

`len()` can also take the number of key-values owned by the map

map is pass by reference

```java
func main() {
	classPopulations := make(map[string]int)
	classPopulations = map[string]int{
		"A": 33,
		"B": 41,
		"C": 37,
	}
	sc := classPopulations
	delete(sc, "C")
	fmt.Println(sc) //map[A:33 B:41]
	fmt.Println(classPopulations) //map[A:33 B:41]
}
```