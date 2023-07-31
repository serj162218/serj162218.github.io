---
title: 'Golang Basic - Struct'
date: 2023-05-10 21:18:43
tags: [Golang Basic]
published: true
hideInList: false
feature: 
isTop: false
---
[reference source](https://youtu.be/YS4e4q9oBaU?t=8885)
# Struct
*struct is like a class, new as a object*

## Example

```java
type Doctor struct {
	number     int
	actorName  string
	companions []string
}

func main() {
	aDoc := Doctor{
		number:    3,
		actorName: "JOSH",
		companions: []string{
			"Liz",
			"Aosh",
		},
	}
	fmt.Println(aDoc) //{3 JOSH [Liz Aosh]}
	fmt.Println(aDoc.number) //3
}
```

You can also code it like this, but it is not recommended because it is inconvenient to maintain.

```java
func main() {
	aDoc := Doctor{
		3,
		"JOSH",
		[]string{
			"Liz",
			"Aosh",
		},
	}
	fmt.Println(aDoc.companions)
}
```

The naming rules of struct are the same as variables, if the first letter of the key is uppercase, it will be exported, and if the first letter of struct is uppercase, it will also be exported.
If the struct is exported, but the key does not, the key cannot be used in other packages.

## anonymous struct

```java
func main() {
	aDoc := struct{ name string }{name: "John"}
	fmt.Println(aDoc)
}
```

Struct is a deep copy, and like Array, it can use indicators to do similar shallow copy

```java
func main() {
	aDoc := struct{ name string }{name: "John"}
	bDoc := aDoc
	bDoc.name = "Bee"
	fmt.Println(aDoc) //{John}
	fmt.Println(bDoc) //{Bee}
}
```

## Embedding

Go does not have inheritance but it have embeddon. Although there has curly brackets when printing out, it does not affect anything.

go沒有inheritance，反之在go是使用embedding，雖然embedding後print out時多了個curly brackets，但在使用上不影響。

```java
type Animal struct {
	Name   string
	Origin string
}

type Bird struct {
	Animal   //embedding
	CanFly   bool
	SpeedKPH float32
}

func main() {
	b := Bird{}
	b.Name = "Blue bird"
	b.Origin = "Australia"
	b.CanFly = true
	b.SpeedKPH = 32.

	fmt.Println(b) //{{Blue bird Australia} true 32}
	fmt.Println(b.Name) //Blue bird
}
```

Another example

```java
func main() {
	b := Bird{
		Animal:   Animal{Name: "BBird", Origin: "Taiwan"},
		CanFly:   true,
		SpeedKPH: 32.,
	}

	fmt.Println(b) //{{BBird Taiwan} true 32}
	fmt.Println(b.Name) //BBird
}
```

## Tag

It has no meaning itself, but for using in some frameworks or validation.

```java
type Animal struct {
	Name   string `required:"true" max:"100"`
	Origin string
}

func main() {
	t := reflect.TypeOf(Animal{})
	field, _ := t.FieldByName("Name")
	fmt.Println(field.Tag) //required:"true" max:"100"
}
```