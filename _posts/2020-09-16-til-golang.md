---
layout: post
title: "[TIL] Go - Array, slice, map, struct"
date: "2020-09-16"
categories:
  - TIL
excerpt: |
  Array, slice, map, struct
feature_text: |
  ## Array, slice, map, struct
  Basic consepts of Go, Array, slice, map, struct.
feature_image: "https://media.vlpt.us/images/meekukin/post/62dfb35f-b1ae-4c0f-b0c6-0e2cc75c4e2a/Golang.png"
image: "https://media.vlpt.us/images/meekukin/post/62dfb35f-b1ae-4c0f-b0c6-0e2cc75c4e2a/Golang.png"
---

Today I learned about array, slice, map and struct of Go.

### Array
In Go, array can be defined like:

```go
names := [5]string{"a", "b"}
```

Unlike JavaScript's array or Python's list, the lenght of array in Go is fixed.
In case above, I created array with length of 5.
so, case below throws an error:

```go
names[2] = "c"
names[3] = "d"
names[4] = "e"

// below throws an error
names[5] = "f"
```

Array can be traversed by using `for` loop and `range`.

```go
simpleArray := [5]string{"a", "b", "c", "d", "e"}
for index, element := range simpleArray {
  fmt.Println(index, element)
}
```

### Slice
Slices are simmilar to array, yet the length is not fixed; dynamic.

There are briefly two ways to create slices.

First, using `make`.

```go
a := make([]int, 5, 10)
```

Code above generates a slice length of 5 and max-length of 10.

```go
a := []int{1, 2, 30, 100}
```

Code above generates a slice that automatically has length of 4.

The size of a slice is dynamically determined.

by using `append`, new slice with appended data could be easily created.

```go
a := []int{1, 2, 3}
b := append(a, 4)
fmt.Println(b)
// [1 2 3 4]
```

`range` is available just like arrays.

```go
simpleSlice := []string{"a", "b", "c", "d"}
for index, element := range simpleSlice {
  fmt.Println(index, element)
}
```

### Map
Map offers a key / value store. Simmilar to Java's map.
Unlike JavaScript or Python, however, the map in go should only have predetermined type of key / value pairs.

For instance,

```go
stringMap := map[string]string{"name": "Leon", "age": "29"}
```

`stringMap` should only have string key and string value.

```go
simpleMap := map[string]int{"year": 2020, "month": 9, "day": 16}
fmt.Println(simpleMap)
// map[day:16 month:9 year:2020]
```

Maps can be iterated like arrays do:
```go
// Map
simpleMap := map[string]string{"name": "Leon", "age": "29"}
fmt.Println(simpleMap)
for key, value := range simpleMap {
	fmt.Println(key, value)
}
```

### Struct
A Struct can contain multiple types of values.
Simmilar to maps, structs stores key / value pairs of data.

```go
type person struct {
	name          string
	age           int
	favoriteFoods []string
}

favoriteFoods := []string{"Spaghetti", "bulgogi"}
leon := person{name: "Leon", age: 29, favoriteFoods: favoriteFoods}
fmt.Println(leon)
// {Leon 29 [Spaghetti bulgogi]}
```
