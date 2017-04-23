defaults
========

[![Build Status](https://travis-ci.org/creasty/defaults.svg?branch=master)](https://travis-ci.org/creasty/defaults)
[![codecov](https://codecov.io/gh/creasty/defaults/branch/master/graph/badge.svg)](https://codecov.io/gh/creasty/defaults)

Initialize members in struct with default values

- Supports almost all kind of types
  - Scalar types
    - `int/8/16/32/64`, `uint/8/16/32/64`, `float32/64`
    - `uintptr`, `time.Duration`
    - `bool`
    - `string`
  - Complex types
    - `map`
    - `slice`
    - `struct`
  - Aliased types
    - e.g., `type Enum string`
  - Pointer types
    - e.g., `*SampleStruct`, `*int`
- Recursively initializes
- Dynamically set default values by [`defaults.Setter`](./setter.go) interface
- Preserves non-initial values from being reset with default values


Usage
-----

```go
type Gender string

type Sample struct {
	Name   string `default:"John Smith"`
	Age    int    `default:"27"`
	Gender Gender `default:"f"` // Supports aliased types

	Slice       []string       `default:"[]"`
	Map         []string       `default:"{}"`
	SliceByJSON []int          `default:"[1, 2, 3]"` // Supports JSON format
	MapByJSON   map[string]int `default:"{\"foo\": 123}"`

	Struct    OtherStruct  `default:"{}"`
	StructPtr *OtherStruct `default:"{\"Foo\": 123}"`
}

type OtherStruct struct {
	Hello  string `default:"world"` // Default tags in nested struct also work
	Random int
}

// SetDefaults implements defaults.Setter interface
func (s *OtherStruct) SetDefaults() {
	s.Random = rand.Int() // You can dynamically set default
}
```

```go
obj := &Sample{}
if err := defaults.Set(obj); err != nil {
	panic(err)
}
```
