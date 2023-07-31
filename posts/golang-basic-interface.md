---
title: 'Golang Basic - Interface'
date: 2023-05-10 20:57:26
tags: [Golang Basic]
published: true
hideInList: false
feature: 
isTop: false
---
[reference source](https://youtu.be/YS4e4q9oBaU?t=17879)
# Interface
As same as other languages, the difference between interface and struct is that interface is a declaration behavior, not a data structure.
## Example
The ConsoleWriter struct implements the interface on the second line

```java
func main() {
	var w Writer = ConsoleWriter{}
	w.Write([]byte("hello Go!"))
}

type Writer interface {
	Write([]byte) (int, error)
}

type ConsoleWriter struct {
}

//method
func (cw ConsoleWriter) Write(data []byte) (int, error) {
	n, err := fmt.Println(string(data))
	return n, err
}
//hello Go!
```

## Name Rules

The naming should be related to the behavior. If there is only one behavior in the interface, name it as `(behavior)er`

```java
type Writer interface {
	Write([]byte) (int, error)
}
```

## If there is a method, it can be implemented as an interface

```java
func main() {
	myInt := IntCounter(0)
	var inc Incrementer = &myInt
	for i := 0; i < 3; i++ {
		fmt.Println(inc.Increment())
	}
}

type Incrementer interface {
	Increment() int
}

type IntCounter int

func (ic *IntCounter) Increment() int {
	*ic++
	return int(*ic)
}

1
2
3
```

## Classic Example

After merging the two interfaces, use struct to implement

```java
func main() {
	var wc WriterCloser = NewBufferedWriterCloser()
	wc.Write([]byte("Hello World, this is a test"))
	wc.Close()
}

type Writer interface {
	Write([]byte) (int, error)
}
type Closer interface {
	Close() error
}

type WriterCloser interface {
	Writer
	Closer
}

type BufferedWriterCloser struct {
	buffer *bytes.Buffer
}

func (bwc *BufferedWriterCloser) Write(data []byte) (int, error) {
	n, err := bwc.buffer.Write(data)
	if err != nil {
		return 0, err
	}
	//Read 8 bytes at a time until the length is less than 8
	v := make([]byte, 8)
	for bwc.buffer.Len() > 8 {
		_, err := bwc.buffer.Read(v)
		if err != nil {
			return 0, err
		}
		_, err = fmt.Println(string(v))
		if err != nil {
			return 0, err
		}
	}
	return n, nil
}

//Read the remaining byte
func (bwc *BufferedWriterCloser) Close() error {
	for bwc.buffer.Len() > 0 {
		data := bwc.buffer.Next(8)
		_, err := fmt.Println(string(data))
		if err != nil {
			return err
		}
	}
	return nil
}

//Return the pointer
func NewBufferedWriterCloser() *BufferedWriterCloser {
	return &BufferedWriterCloser{
		buffer: bytes.NewBuffer([]byte{}),
	}
}

Hello Wo
rld, thi
s is a t
est
```

## Type Conversion

```java
func main() {
	var wc WriterCloser = NewBufferedWriterCloser()
	wc.Write([]byte("Hi"))
	wc.Close()
	
	//bwc is no longer WriterCloser but BufferedWriterCloser
	bwc := wc.(*BufferedWriterCloser)
	fmt.Println(bwc)
}

Hi
&{0xc0000241b0}
```

### To avoid type conversion error

instead of doing this then use recover

```java
func main() {
	var wc WriterCloser = NewBufferedWriterCloser()
	wc.Write([]byte("Hi"))
	wc.Close()

	bwc := wc.(io.Reader)
	fmt.Println(bwc)
}

Hi
panic: interface conversion: *main.BufferedWriterCloser is not io.Reader: missing method Read
```

we can doing this

```java
func main() {
	var wc WriterCloser = NewBufferedWriterCloser()
	wc.Write([]byte("Hi"))
	wc.Close()

	r, ok := wc.(io.Reader)
	if ok {
		fmt.Println(r)
	} else {
		fmt.Println("Conversion failed")
	}
}

Hi
Conversion failed
```

### Change the above example to return value.

```java
func main() {
	var wc WriterCloser = NewBufferedWriterCloser()
	wc.Write([]byte("Hillo owworowro"))
	wc.Close()

	r, ok := wc.(BufferedWriterCloser)
	if ok {
		fmt.Println(r)
	} else {
		fmt.Println("Conversion failed")
	}
}

type Writer interface {
	Write([]byte) (int, error)
}
type Closer interface {
	Close() error
}

type WriterCloser interface {
	Writer
	Closer
}

type BufferedWriterCloser struct {
	buffer *bytes.Buffer
}

func (bwc BufferedWriterCloser) Write(data []byte) (int, error) {
	n, err := bwc.buffer.Write(data)
	if err != nil {
		return 0, err
	}
	v := make([]byte, 8)
	for bwc.buffer.Len() > 8 {
		_, err := bwc.buffer.Read(v)
		if err != nil {
			return 0, err
		}
		_, err = fmt.Println(string(v))
		if err != nil {
			return 0, err
		}
	}
	return n, nil
}

func (bwc BufferedWriterCloser) Close() error {
	for bwc.buffer.Len() > 0 {
		data := bwc.buffer.Next(8)
		_, err := fmt.Println(string(data))
		if err != nil {
			return err
		}
	}
	return nil
}

func NewBufferedWriterCloser() BufferedWriterCloser {
	return BufferedWriterCloser{
		buffer: bytes.NewBuffer([]byte{}),
	}
}
```

## Interface with no method

It is very useful when testing the type of a variable. You can only declare an empty interface and you can always convert the type.
Because it is an empty interface, it cannot perform any behavior.

### example 1

```java
func main(){
	var wc interface{} = NewBufferedWriterCloser()
}
type Writer interface {}
```

### example 2

An empty interface may hold values of any type. (Every type implements at least zero methods.)

Empty interfaces are used by code that handles values of unknown type. For example, `fmt.Print` takes any number of arguments of type `interface{}`.

```java
func main() {
	var i interface{}
	describe(i)

	i = 42
	describe(i)

	i = "hello"
	describe(i)
}

func describe(i interface{}) {
	fmt.Printf("(%v, %T)\n", i, i)
}

(<nil>, <nil>)
(42, int)
(hello, string)
```

### example 3

Anything can be put into an empty interface, even primitive types

```java
func main() {
	var myObj interface{} = NewBufferedWriterCloser()
	if wc, ok := myObj.(WriterCloser); ok {
		wc.Write([]byte("Hillo owworowro"))
		wc.Close()
	}
	r, ok := myObj.(BufferedWriterCloser)
	if ok {
		fmt.Println(r)
	} else {
		fmt.Println("Conversion failed")
	}
	myObj = 5
	fmt.Printf("%T %v", myObj, myObj)
}

Hillo ow
worowro
{0xc0000241b0}
int 5
```

## Pointer receiver / Value receiver

### example 1

Line 2 is to pass the value of myWriterReader, so lines 20 and 21 must be value type

```java
func main() {
	var m WriterReader = myWriterReader{}
	fmt.Println(m)
}

type Writer interface {
	Write()
}
type Reader interface {
	Read()
}

type WriterReader interface {
	Writer
	Reader
}

type myWriterReader struct{}

func (m myWriterReader) Write() {}
func (m myWriterReader) Read()  {}
```

### example 2

Line 2 pass a pointer type, then 20 21 can be either value or pointer.

```java
func main() {
	var m WriterReader = &myWriterReader{}
	fmt.Println(m)
}

type Writer interface {
	Write()
}
type Reader interface {
	Read()
}

type WriterReader interface {
	Writer
	Reader
}

type myWriterReader struct{}

func (m *myWriterReader) Write() {}
func (m myWriterReader) Read() {}
```

## Best Practice

- Use many, small interface with one performance
    - Single method interfaces are some of the most powerful and flexible
        - io.Writer, io.Reader, interface{}
- Don’t export interfaces for types that will be consumed
    - Instead of export interface, it is better to export concrete type, so that you can define the interface you want for testing.
    - If a package exports the behavior of A B C with interface, it needs to implement A B C, but C behavior may not be used, so it will look better to export struct at this time, and then write an interface with A B to receive it.
- Do export interfaces for types that will be used by package
    - If you’re creating a library that other people are going to consume, you can define the interface that you accept. And then they can provide whatever implementations that  they want.
- Design functions and methods to receive interfaces whenever possible