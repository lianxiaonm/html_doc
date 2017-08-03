## JavaScript
*
*
* 遍历自己的属性，不包含原型和不可枚举属性
    ``` JavaScript
    let object = {...}                // 一个复杂的对象
    let keys = Object.keys(object);  //返回对象自身的属性，不包括原型和不可枚举的属性
    for(let key in object){
        if(object.hasOwnProperty(key))  //判断key是否为自身属性
            continue;
    }
    for(let val of objecy){             //ES6的语法糖
    }
    ```
* cookie,sessionStorage和localStorage的区别
*

## webpack


## framework（vue angular react...）



