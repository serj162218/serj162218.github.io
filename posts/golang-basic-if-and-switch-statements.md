---
title: 'Golang Basic - If and Switch Statements'
date: 2023-05-10 20:49:45
tags: [Golang Basic]
published: true
hideInList: false
feature: 
isTop: false
---
[reference source](https://youtu.be/YS4e4q9oBaU?t=10080)
# If

```java
func main() {
	if true {
		fmt.Println("it will print out")
	}
}
```
Curly brackets cannot be removed, even if there is only one line to execute.
.
### initializer

If the variables is generated in this way, the scope will only be in the conditional expression.

```java
func main() {
	classPop := map[string]int{
		"A": 55,
		"B": 37,
	}
	if pop, ok := classPop["A"]; ok {
		fmt.Println(pop)//55
	}
	fmt.Println(pop) //undefined: pop
}
```

### Conditional

```java
func main() {
	number := 50
	goal := 70
	if number < 0 || number > 100 {
		fmt.Println("number is not between 0 and 100")
	}
	fmt.Println(number > goal, number <= goal, number != goal, number == goal)
}
```

### if else if else

```java
func main() {
	if ... {
	
	} else if ... {
	
	} else{
	
	}
}
```

## Switch

### No need to use `break`

```java
func main() {
	num := 1
	switch num {
	case 1:
		fmt.Println(1)
	case 2:
		fmt.Println(2)
	case 10:
		fmt.Println(10)
	default:
		fmt.Println("??")
	}
}
```

### Comparing multiple values in one case

Other languages, like PHP.

```php
switch ($a){
		case 1:
		case 2:
		case 3:
				print("...");
				break;
		...
}
```

Go

```java
func main() {
	num := 3
	switch num {
	case 1, 2, 3, 4:
		fmt.Println(1, 2, 3, 4)
	case 10:
		fmt.Println(10)
	default:
		fmt.Println("??")
	}
}
```

### If repeated conditions in the statements, an error will be reported.

```java
func main() {
	num := 3
	switch num {
	case 1, 2, 3, 4:
		fmt.Println(1, 2, 3, 4) //duplicate case 4 (constant of type int) in expression switch
	case 10, 4:
		fmt.Println(10)
	default:
		fmt.Println("??")
	}
}
```

### Condition

If there are duplicates in the range, compare from top to bottom, and the first statement with true will be executed.

```java
func main() {
	num := 5
	switch {
	case num < 10:
		fmt.Println("less than 10")
	case num < 20:
		fmt.Println("less than 20")
	default:
		fmt.Println("??")
	}
}
```

### initializer

```java
func main() {
	switch num := 2 + 1; num {
	case 1, 2, 3, 4:
		fmt.Println(1, 2, 3, 4)
	case 10:
		fmt.Println(10)
	default:
		fmt.Println("??")
	}
}
```

Can call other functions in switch

```java
func main() {
	switch num, _ := test(); num {
	case 1, 2, 3, 4:
		fmt.Println(1, 2, 3, 4)
	case 10:
		fmt.Println(10)
	default:
		fmt.Println("??")
	}
}
func test() (int, bool) {
	return 3, true
}
```

### fallthrough

Even if the next layer of case does not meet the conditions, it will be executed, and there are not many opportunities to use fallthrough.

```java
func main() {
	i := 10
	switch {
	case i == 10:
		fmt.Println(10)
		fallthrough
	case i == 20:
		fmt.Println(20)
	}
}
//10 20
```

### Switch with type

In the example, interface{} is used because interface{} can plug any type, and types can be compared in this way
If you use i := 1 or var i int = 1, it won't work, because i.(type) is an interface.

```java
func main() {
	var i interface{} = 1
	switch i.(type) {
	case int:
		fmt.Println("it is int") //print this line
	case string:
		fmt.Println("it is string")
	}
}
```

```java
func main() {
	var i interface{} = [3]int{}
	switch i.(type) {
	case [2]int:
		fmt.Println(2)
	case [3]int:
		fmt.Println(3) //this line will print out
	}
}
```

### break

```java
func main() {
	i := 3
	switch i {
	case 1, 2, 3, 4:
		fmt.Println(1)
		fmt.Println(2)
		fmt.Println(3)
		break
		fmt.Println(4) //won't execute but it get error with unreachable code
									 //might be different with other compiler
	case 5:
		fmt.Println(5)
	}
}
```