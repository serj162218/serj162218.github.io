---
title: 'Golang Basic - For loop'
date: 2023-05-10 20:30:20
tags: [Golang Basic]
published: true
hideInList: false
feature: 
isTop: false
---
[reference source](https://youtu.be/YS4e4q9oBaU?t=12077)
# For loop
## Simple loop
### Only non-declaration in for loop initializer

```java
func main() {
	for i := 0; i < 5; i++ {
		fmt.Println(i)
	}
}
```

### Multiple variable example. Note that you cannot use i++, j++.

```java
func main() {
	for i, j := 0, 0; i < 5 && j < 4; i, j = i+1, j+2 {
		fmt.Println(i, j)
	}
}
```

### Scope

```java
func main() {
	// i scope only for loop
	for i := 0; i < 5; i++ {
		fmt.Println(i)
	}
}
```

```java
func main() {
	i := 0 //scope in main func
	for ; i < 5; i++ {
		fmt.Println(i)
	}
}
```

### use it like while loop

```java
func main() {
	i := 0
	for i < 5 {
			fmt.Println(i)
			i++
	}
}
```

### break

```java
func main() {
		i := 0
		for {
				fmt.Println(i)
				if i == 5 {
						break
				}
				i++
		}
}
```

### continue

```java
func main() {
		i := 0
		for i < 10 {
				i++
				if i%2 == 0 {
						fmt.Println(i) //2 4 6 8 10
						continue
				}
		}
}
```

### Label

Because break will only stop the nearest loop, you can use Label when encountering a nested loop

Label does not have to be called "Loop"

```java
func main() {
Loop:
	for i := 1; i <= 3; i++ {
		for j := 1; j <= 3; j++ {
			fmt.Println(i * j)
			if i*j >= 6 {
				break Loop
			}
		}
	}
}
```

## Loop through collection

### Collection can be slice, array, map, string

```java
func main() {
	s := []int{3, 6, 9}
	for k, v := range s {
		fmt.Println(k, v)
	}
}

0 3
1 6
2 9
```

### map

```java
func main() {
	classPopulations := map[string]int{
		"A": 33,
		"B": 41,
		"C": 37,
	}
	//It can be noticed that the loop of the map will not be in order
	for k, v := range classPopulations {
		fmt.Println(k, v)
	}
}

B 41
C 37
A 33
```

### string

Because String actually stores bytes, so pay attention to converting to string when using.

```java
func main() {
	s := "hello world"
	for k, v := range s {
		fmt.Println(k, string(v))
	}
}

0 h
1 e
2 l
3 l
4 o
5  
6 w
7 o
8 r
9 l
10 d
```

### _ (underscore)

Because Go insists that every variable must be used, underscore can be used if you don't want to use this variable.

```java
func main() {
		s := "hello world"
		for _, v := range s {
				fmt.Println(string(v))
		}
}
```