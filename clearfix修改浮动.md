```css
.cf:before,
.cf:after {
    content: " "; /* 1 */
    display: table; /* 2 */
}

.cf:after {
    clear: both;
}

/**
 * For IE 6/7 only
 * Include this rule to trigger hasLayout and contain floats.
 */
.cf {
    *zoom: 1;
}
```

上述代码中的`.cd:before`是为了在为该类型前面添加一个内容为" "（这个空格可以神奇的修复一个opera的bug）的伪元素，并将其转换为`display:table`。用于防止上部出现**边距重叠**的发生。
下部分`.cf:after`则不同，它通过`clear:both`去除两侧的浮动。
至于什么鬼的`*zoom:1`是用来修复IE6/7的问题的。已经不需要讨论了。