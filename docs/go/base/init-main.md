## init 函数

> 1. init函数是用于程序执行前做包的初始化的函数，比如初始化包里的变量等
> 2. 每个包可以拥有多个init函数
> 3. 包的每个源文件也可以拥有多个init函数
> 4. 同一个包中多个init函数的执行顺序go语言没有明确的定义(说明)
> 5. 不同包的init函数按照包导入的依赖关系决定该初始化函数的执行顺序
> 6. init函数不能被其他函数调用，而是在main函数执行之前，自动被调用


## init main 函数的异同

    相同点：
        两个函数在定义时不能有任何的参数和返回值，且Go程序自动调用
    不同点：
        init可以应用于任意包中，且可以重复定义多个
        main函数只能用于main包中，且只能定义一个

两个函数的执行顺序
> 1. 对同一个 go 文件的 `init()` 调用顺序是从上到下的
> 2. 对同一个 package 中不同文件是按文件名字符串比较“从小到大”顺序调用各文件中的 `init()` 函数
> 3. 对于不同的 package ，如果不相互依赖的话，按照 main 包中”先 import 的后调用”的顺序调用其包中的 `init()` ，如果 package 存在依赖，则先调用最早被依赖的 package 中的 `init()` ，最后调用 `main` 函数
> 4. 如果 `init` 函数中使用了 `println()` 或者 `print()` 你会发现在执行过程中这两个不会按照你想象中的顺序执行。这两个函数官方只推荐在测试环境中使用，对于正式环境不要使用