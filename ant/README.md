# Command ant

A deployment tools for [TP-Micro](https://github.com/henrylee2cn/tp-micro) micro service framework.

## Feature

- Quickly create a ant project
- Run ant project with hot compilation

## Install

```sh
go install
```

## Generate project

`ant gen` command help:

```
NAME:
   ant gen - Generate an ant project

USAGE:
   ant gen [command options] [arguments...]

OPTIONS:
   --template value, -t value    The template for code generation(relative/absolute)
   --app_path value, -p value  The path(relative/absolute) of the project
```

example: `ant gen -t ./__ant__tpl__.go -p ./myant` or default `ant gen myant`

- template file `__ant__tpl__.go` demo:

```go
// package __ANT__TPL__ is the project template
package __ANT__TPL__

// __API__PULL__ register PULL router:
//  /home
//  /math/divide
type __API__PULL__ interface {
    Home(*struct{}) *HomeReply
    Math
}

// __API__PUSH__ register PUSH router:
//  /stat
type __API__PUSH__ interface {
    Stat(*StatArgs)
}

// Math controller
type Math interface {
    // Divide handler
    Divide(*DivideArgs) *DivideReply
}

// HomeReply home reply
type HomeReply struct {
    Content string // text
}

type (
    // DivideArgs divide api args
    DivideArgs struct {
        // dividend
        A float64
        // divisor
        B float64 `param:"<range: 0.01:100000>"`
    }
    // DivideReply divide api result
    DivideReply struct {
        // quotient
        C float64
    }
)

// StatArgs stat handler args
type StatArgs struct {
    Ts int64 // timestamps
}
```

- The template generated by `ant gen` command.

```
├── README.md
├── main.go
├── api
│   ├── handlers.gen.go
│   ├── handlers.go
│   ├── router.gen.go
│   └── router.go
├── logic
│   └── tmp_code.gen.go
├── rerrs
│   └── rerrs.go
├── sdk
│   ├── rpc.gen.go
│   ├── rpc.gen_test.go
│   ├── rpc.go
│   └── rpc_test.go
└── types
    ├── types.gen.go
    └── types.go
```

Desc:

- add `.gen` suffix to the file name of the automatically generated file
- `tmp_code.gen.go` is temporary code used to ensure successful compilation!<br>When the project is completed, it should be removed!

## Run project

`ant run` command help:

```
NAME:
   ant run - Compile and run gracefully (monitor changes) an any existing go project

USAGE:
   ant run [options] [arguments...]
 or
   ant run [options except -app_path] [arguments...] {app_path}

OPTIONS:
   --watch_exts value, -x value  Specified to increase the listening file suffix (default: ".go", ".ini", ".yaml", ".toml", ".xml")
   --app_path value, -p value    The path(relative/absolute) of the project
```

example: `ant run -x .yaml -p myant` or `ant run`
