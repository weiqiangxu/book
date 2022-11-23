

[github.com/pkg/errors](https://pkg.go.dev/github.com/pkg/errors)

[pkg.go.dev/errors](https://pkg.go.dev/errors)

### 差异

```
两者不同本质上在 fmt.Printf("%+v" ,err)时候 github.com 的多了堆栈信息
```

### 使用建议

1. 用github.com/pkg/errors的包
2. 如果是给后台程序员看日志的error的话使用 %+v 打印错误信息可以把堆栈信息打印出来
3. WithMessage\WithMessagef\Wrap\Wrapf\Errorf 这几个都是包装error向上传递,并且都会记录堆栈信息,没看出啥区别

### 看看直接 errors.New 和 errors.WithStack(errors.New) 的区别

1. 看看 fmt.Printf("%+v", errors.New("b")) 的输出

```
b
main.b
        /Users/xuweiqiang/Documents/code/yyyy/main.go:20
main.a
        /Users/xuweiqiang/Documents/code/yyyy/main.go:16
main.main
        /Users/xuweiqiang/Documents/code/yyyy/main.go:9
runtime.main
        /usr/local/go/src/runtime/proc.go:250
runtime.goexit
        /usr/local/go/src/runtime/asm_arm64.s:1172
进程 已完成，退出代码为 0
```

2. 看看 fmt.Printf("%+v", errors.WithStack(errors.New("b"))) 
```
b
main.b
        /Users/xuweiqiang/Documents/code/yyyy/main.go:20
main.a
        /Users/xuweiqiang/Documents/code/yyyy/main.go:16
main.main
        /Users/xuweiqiang/Documents/code/yyyy/main.go:9
runtime.main
        /usr/local/go/src/runtime/proc.go:250
runtime.goexit
        /usr/local/go/src/runtime/asm_arm64.s:1172

### 多出来下面的
main.b
        /Users/xuweiqiang/Documents/code/yyyy/main.go:20
main.a
        /Users/xuweiqiang/Documents/code/yyyy/main.go:16
main.main
        /Users/xuweiqiang/Documents/code/yyyy/main.go:9
runtime.main
        /usr/local/go/src/runtime/proc.go:250
runtime.goexit
        /usr/local/go/src/runtime/asm_arm64.s:1172
```

> 总结：没有必要 WithStack 因为已经有足够的堆栈信息了


### 比对 直接return error 和 return errors.Wrap 和 errors.Errorf 和 errors.WithMessage 错误的区别

1. 直接 return error

```
b
main.b
        /Users/xuweiqiang/Documents/code/yyyy/main.go:18
main.a
        /Users/xuweiqiang/Documents/code/yyyy/main.go:14
main.main
        /Users/xuweiqiang/Documents/code/yyyy/main.go:9
runtime.main
        /usr/local/go/src/runtime/proc.go:250
runtime.goexit
        /usr/local/go/src/runtime/asm_arm64.s:1172
进程 已完成，退出代码为 0
```

2. return errors.Wrap(b(), "i am message")  本质上是保留了上一个errors.New 然后再追加一层当前行往上的堆栈信息 （而追加的这一层的 b() 里面哪一行抛出异常的是没有被记录的）

```
b
main.b
        /Users/xuweiqiang/Documents/code/yyyy/main.go:18
main.a
        /Users/xuweiqiang/Documents/code/yyyy/main.go:14
main.main
        /Users/xuweiqiang/Documents/code/yyyy/main.go:9
runtime.main
        /usr/local/go/src/runtime/proc.go:250
runtime.goexit
        /usr/local/go/src/runtime/asm_arm64.s:1172

### wrap之后多出来的一层

i am message
main.a
        /Users/xuweiqiang/Documents/code/yyyy/main.go:14
main.main
        /Users/xuweiqiang/Documents/code/yyyy/main.go:9
runtime.main
        /usr/local/go/src/runtime/proc.go:250
runtime.goexit
        /usr/local/go/src/runtime/asm_arm64.s:1172
进程 已完成，退出代码为 0

```

3. errors.Errorf("i am message %s", b()) 的输出 （b这个func的18行是return error的地方，这个行号没了）

```
i am message b
main.a
        /Users/xuweiqiang/Documents/code/yyyy/main.go:14
main.main
        /Users/xuweiqiang/Documents/code/yyyy/main.go:9
runtime.main
        /usr/local/go/src/runtime/proc.go:250
runtime.goexit
        /usr/local/go/src/runtime/asm_arm64.s:1172
进程 已完成，退出代码为 0
```

> 总结： 直接 errors.Errorf 就会丢失了 b() 那个 func 里面抛出 error 的行号 , errors.Errorf 不要滥用会丢失堆栈

> errors.Wrap 可以使用但是会把堆栈多了很多层，但是可以携带一些个人信息



4. return errors.WithMessage(b(), "i am message") 打印输出

```
b
main.b
        /Users/xuweiqiang/Documents/code/yyyy/main.go:18
main.a
        /Users/xuweiqiang/Documents/code/yyyy/main.go:14
main.main
        /Users/xuweiqiang/Documents/code/yyyy/main.go:9
runtime.main
        /usr/local/go/src/runtime/proc.go:250
runtime.goexit
        /usr/local/go/src/runtime/asm_arm64.s:1172
i am message   ### 这个是和 errors.New 唯一的区别
```

> 总结：wrap会把堆栈搞得很复杂，多了好多，Errorf会丢失上一层的堆栈信息，用这个会更合适

## 总结

### 1. 使用包github.com/errors替换pkg/errors
### 2. 在透传error的时候用errors.WithMessage不要用Wrap也不要用Errorf
### 3. errors.New就有堆栈了不用New出来后WithStack