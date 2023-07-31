---
title: 'Golang Basic - Primitives'
date: 2023-05-10 21:14:01
tags: [Golang Basic]
published: true
hideInList: false
feature: 
isTop: false
---
[reference source](https://youtu.be/YS4e4q9oBaU?t=3425)
# Primitives
## Bool
```java
func main() {
	var a bool = true
	b := 1 == 1 
	var c bool = 2 == 1
	fmt.Printf("%v %v %v\n", a, b, c) //true true false
}
```

## Integer

### signed integer

| int8 | -128 ~ 127 |
| --- | --- |
| int16 | -32 768 ~ 32 767 |
| int32 | -2 147 483 648 ~ 2 147 483 647 |
| int64 | -9 223 372 036 854 775 808 ~ 9 223 372 036 854 775 807 |

### unsigned integer

| uint8 | 0 ~ 255 |
| --- | --- |
| uint16 | 0 ~ 65 535 |
| uint32 | 0 ~ 4 294 967 295 |

### with operator

```java
func main() {
	a := 10
	b := 4
	fmt.Println(a + b) //14
	fmt.Println(a - b) //6
	fmt.Println(a / b) //2
	fmt.Println(a * b) //40
	fmt.Println(a % b) //2
}
```

### Integer’s type can’t be different when operation

```java
func main() {
	var a int = 10
	var b int8 = 12
	fmt.Println(a + b)
	//invalid operation: a + b (mismatched types int and int8)
}
```

### Bit operators

```java
func main() {
	a := 10             //1010
	b := 3              //0011
	fmt.Println(a & b)  //AND 0010
	fmt.Println(a | b)  //OR 1011
	fmt.Println(a ^ b)  //Exclusive OR 1001
	fmt.Println(a &^ b) //NAND 1000, a(1010) & ^b(0011->1100), so it becomes 1010 & 1100
}
```

### Bit shifting

```java
func main() {
	a := 4              //0100 2^2
	fmt.Println(a >> 1) //0010 2^2 / 2^1
	fmt.Println(a << 2) //1000 2^2 * 2^2
}
```

## Float

| float32 | +-1.18E-38 ~ +-3.4E38 |
| --- | --- |
| float64 | +-2.23E-308 ~ +-1.8E308 |

```java
func main() {
	n := 3.14
	fmt.Printf("%v %T\n", n, n) //3.14 float64
	n = 13.7e72
	fmt.Printf("%v %T\n", n, n) //1.37e+73 float64
	n = 2.1e14
	fmt.Printf("%v %T\n", n, n) //2.1e+14 float64
}
```

## Complex

complex64, complex32 are numbers larger than float, used for data analysis.

## String
You can treat string as an array, but when you take string\[n], it will become byte, because string is actually another name for bytes in Go

```java
func main() {
	s := "hello world"
	fmt.Printf("%v, %T\n", s[2], s[2]) //108, uint8
	fmt.Printf("%v, %T\n", string(s[2]), s[2]) //l, uint8
}
```

string is immutable, and each element is byte, so it cannot be modified

```java
func main() {
	s := "hello world"
	s[2] = "u" //cannot assign to s[2] (value of type byte)
	fmt.Printf("%v, %T\n", s[2], s[2])
}
```

But strings can be added, after all arrays can also be added

```java
func main() {
	a := "hello "
	a = a + "world"
	fmt.Printf("%v, %T\n", a, a)
}
```
string can be converted into byte, it is often used between functions or when communicating with other applications.

```java
func main() {
	a := "hello world"
	b := []byte(a)
	fmt.Printf("%v, %T\n", b, b)
	//[104 101 108 108 111 32 119 111 114 108 100], []uint8
}
```

## Rune
string is utf8, Rune is utf32, in utf32 it can be up to 32bits, but not necessarily 32bits.

Rune is another name for int32, declared in single quotes.

The difference with string is that even if rune is used as the type, it is still int32.
```java
func main() {
	var a rune = 'a'
	b := 'b'
	fmt.Printf("%v %T\n", a, a) //97 int32
	fmt.Printf("%v %T\n", b, b) //98 int32
}
```

> To process the data stream which is encoded into utf32, you can use a api called `ReadRune()`