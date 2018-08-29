将伪数组转化为数组

    function foo() {
        var arr = Array.prototype.slice.call(arguments)
        arr.push('bam')
        console.log(arr)
    }
    foo('bar', 'baz')

    ES6中可以使用

        var arr = Array.from(arguments)