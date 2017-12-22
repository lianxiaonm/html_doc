> 这是css知识点的一个集合。

## LESS
* extend和mixins的区别 --> [css尺寸归并](http://www.lesscss.net/features/#extend-feature-css-)
    ``` less
    .a { color:red;
        .b{ color:blue }
    }
    //mixins -- 编译生成两份独立的css .a .b{} .d .b{}
    .c{ .b }
    //extend -- 编译生成一份独立的css 通过,分隔 。。 如 .a .b,.d .b{}
    .d:extend(.b all){ }
    ```

## Stylus

## SASS

