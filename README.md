# gotest-template

templates for [gotests](https://github.com/cweill/gotests)

# Usage

## 1. clone this repo

```bash
git clone https://github.com/double12gzh/gotest-template.git
```
## 2. enjoy

let's prepapre a go program

```golang
package examples

import (
        "testing"

        "gopkg.in/go-playground/assert.v1"
)

func TestFoo_Bar(t *testing.T) {
        type args struct {
                x int
        }
        tests := []struct {
                name  string
                f     *Foo
                args  args
                want  string
                setup func(args args, want string)
        }{
                // TODO: Add test cases.
        }
        for _, tt := range tests {
                t.Run(tt.name, func(t *testing.T) {
                        f := &Foo{}
                        if tt.setup != nil {
                                tt.setup(tt.args, tt.want)
                        }
                        assert.Equal(t, tt.want, f.Bar(tt.args.x))

                })
        }

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
### example1
```bash
 gotests -template_dir /root/gotest-template/templates  -only "^bar$" ./exmaple.go
```

```golang
package examples

import (
        "testing"

        "gopkg.in/go-playground/assert.v1"
)


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

## example2 

```golang
➜  abb gotests -template_dir /root/gotests-template/templates  -only ^Bar$ examples/e.go
Generated TestFoo_Bar
package examples

import (
        "testing"

        "gopkg.in/go-playground/assert.v1"
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
