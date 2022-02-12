(@[TOC])
---

# gotest-template

Auto generate test for golang code. Templates for [gotests](https://github.com/cweill/gotests)

# Usage

## 1. install gotests
```bash
go get -u github.com/cweill/gotests/...
```

## 2. clone this repo

```bash
git clone https://github.com/double12gzh/gotest-template.git
```
## 3. enjoy

let's prepapre a go program

```golang
package examples

import "fmt"

type Student struct {
        Name string
        Age  int
}

type Foo struct {
}

func (f *Foo) Bar(x int) string {
        if x < 0 {
                return "x is a negative integer"
        } else if x > 0 && x < 10 {
                return "x between zero and 10"
        } else if x > 10 && x < 20 {
                return "x between 10 and 20"
        } else if x > 30 && x < 50 {
                return "x between 30 and 50"
        } else {
                return fmt.Sprintf("x not recognized: %d", x)
        }
}
func (f *Foo) bar(x string) Student {
        return Student{Name: "ga", Age: 1}
}

type Manufacture struct {
        Name    string
        Country string
}

type Car struct {
        Kind     string
        Producer Manufacture
}

type Experience struct {
}

func bar(car Car) Experience {

        return Experience{}

}

```
### 3.1 example1
Generate test case with the given test templates `/root/gotest-template/templates`
```bash
 gotests -template_dir /root/gotest-template/templates  -only "^bar$" ./exmaple.go
```

Generated test cases
```golang
package examples

import (
        "testing"

        . "github.com/agiledragon/gomonkey"
        . "github.com/smartystreets/goconvey/convey"
)

func TestFoo_bar(t *testing.T) {
        type args struct {
                x string
        }

        tests := []struct {
                name        string
                f           *Foo
                args        args
                want        Student
                extraCheck  func()
                mockSQLFUNC func()
                patchesFunc func() *Patches
        }{
                // TODO: Add test cases.
        }

        Convey("start", t, func() {
                Convey("case", func() {
                        for _, ttt := range tests {
                                tt := ttt
                                f := func() {

                                        patches := tt.patchesFunc()
                                        defer patches.Reset()

                                        tt.mockSQLFUNC()

                                        f := &Foo{}
                                        got := f.bar(tt.args.x)
                                        So(got, ShouldResemble, tt.want)

                                        tt.extraCheck()
                                }

                                f()

                        }

                })
        })
}

func Test_bar(t *testing.T) {
        type args struct {
                car Car
        }

        tests := []struct {
                name        string
                args        args
                want        Experience
                extraCheck  func()
                mockSQLFUNC func()
                patchesFunc func() *Patches
        }{
                // TODO: Add test cases.
        }

        Convey("start", t, func() {
                Convey("case", func() {
                        for _, ttt := range tests {
                                tt := ttt
                                f := func() {

                                        patches := tt.patchesFunc()
                                        defer patches.Reset()

                                        tt.mockSQLFUNC()

                                        got := bar(tt.args.car)
                                        So(got, ShouldResemble, tt.want)

                                        tt.extraCheck()
                                }

                                f()

                        }

                })
        })
}

```

### 3.2 example2 
Generate test case for method/function (case in-sensitive)

```bash
gotests -template_dir /root/gotests-template/templates -only ^Bar$ examples/e.go
```

Generated test cases
```golang
package examples

import (
        "testing"

        . "github.com/agiledragon/gomonkey"
        . "github.com/smartystreets/goconvey/convey"
)

func TestFoo_Bar(t *testing.T) {
        type args struct {
                x int
        }

        tests := []struct {
                name        string
                f           *Foo
                args        args
                want        string
                extraCheck  func()
                mockSQLFUNC func()
                patchesFunc func() *Patches
        }{
                // TODO: Add test cases.
        }

        Convey("start", t, func() {
                Convey("case", func() {
                        for _, ttt := range tests {
                                tt := ttt
                                f := func() {

                                        patches := tt.patchesFunc()
                                        defer patches.Reset()

                                        tt.mockSQLFUNC()

                                        f := &Foo{}
                                        got := f.Bar(tt.args.x)
                                        So(got, ShouldEqual, tt.want)

                                        tt.extraCheck()
                                }

                                f()

                        }

                })
        })
}

```

### 3.3 example3
Only generate test case for exported method/function

```bash
gotests -template_dir /root/gotests-template/templates -only ^Bar$ -exported examples/e.go
```

### 3.4 example4 

```bash
gotests -template_dir /root/gotests-template/templates -only ^bar$ examples/e.go
```
