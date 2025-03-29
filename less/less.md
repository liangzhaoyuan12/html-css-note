如果要使用除法，要用小括号包起来

```less
@base: 5cm + (10cm / 2);
```

在默认情况下，会导出为当前目录下相同名字的css文件

如果要指定生成的css的目录和文件名

在第一行输入

```less
// out: ./css/styles.css
```

如果要求不会输出为css，例如我们要清除默认样式的less，不需要这些文件的css，只需要less就可以了

那么就在第一行输入

```less
// out: false
```

# Less 使用文档

Less 是一种 CSS 预处理器，它扩展了 CSS 语言，增加了变量、混合（mixins）、函数等功能，使 CSS 更易于维护和扩展。

## 1. 变量

Less 允许使用变量来存储值，然后在样式表中重复使用。

```less
// 定义变量
@primary-color: #428bca;
@secondary-color: #5cb85c;
@font-size-base: 14px;

// 使用变量
.header {
  color: @primary-color;
  font-size: @font-size-base;
}

.button {
  background-color: @secondary-color;
  color: white;
}
```

## 2. 嵌套

Less 允许嵌套规则，使代码结构更清晰。

```less
.nav {
  background-color: #333;
  
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }
  
  li {
    display: inline-block;
    
    a {
      color: white;
      text-decoration: none;
      padding: 10px 15px;
      
      &:hover {
        background-color: #111;
      }
    }
  }
}
```

### & 符号

`&` 符号表示当前选择器的父选择器，常用于伪类和伪元素。

```less
.button {
  color: blue;
  
  &-large {
    font-size: 20px;
  }
  
  &:hover {
    color: darkblue;
  }
  
  &::after {
    content: "→";
  }
}
```

## 3. 运算

Less 支持算术运算（+、-、*、/），可以对数字、颜色或变量进行运算。

```less
@base-padding: 10px;
@base-margin: @base-padding * 2;
@base-color: #888;

.box {
  padding: @base-padding;
  margin: @base-margin;
  background-color: @base-color + #111; // 颜色变亮
  width: 100% / 3;
}
```

## 4. 导入（Import）

Less 支持使用 `@import` 导入其他 Less 文件。

```less
// 导入 variables.less 文件
@import "variables";

// 导入 mixins.less 文件
@import "mixins";

// 导入 CSS 文件（会原样输出）
@import (inline) "reset.css";
```

导入选项：
• `reference`: 只导入但不输出
• `inline`: 原样输出，不处理
• `less`: 作为 Less 文件处理（默认）
• `css`: 作为 CSS 文件处理
• `once`: 只导入一次（默认）
• `multiple`: 可以多次导入

## 5. 导出（编译为 CSS）

Less 文件需要编译为 CSS 才能在浏览器中使用。编译方式有多种：

### 命令行编译

```bash
lessc styles.less styles.css
```

### Node.js API

```javascript
const less = require('less');

less.render('@color: red; body { color: @color }', (e, output) => {
  console.log(output.css);
});
```

### 浏览器端编译（不推荐生产环境使用）

```html
<link rel="stylesheet/less" type="text/css" href="styles.less" />
<script src="//cdnjs.cloudflare.com/ajax/libs/less.js/3.11.1/less.min.js"></script>
```

## 6. 其他特性

### 混合（Mixins）

```less
.border-radius(@radius: 5px) {
  -webkit-border-radius: @radius;
     -moz-border-radius: @radius;
          border-radius: @radius;
}

.button {
  .border-radius();
}

.box {
  .border-radius(10px);
}
```

### 函数

Less 提供了许多内置函数用于颜色操作、数学运算等。

```less
@color: #f36;

.header {
  background-color: lighten(@color, 20%);
  color: darken(@color, 10%);
}
```

### 命名空间

```less
#bundle() {
  .button {
    display: block;
    border: 1px solid black;
    background-color: grey;
  }
}

#header a {
  color: orange;
  #bundle.button();  // 混合 #bundle 中的 .button
}
```

## 7. 最佳实践

1. 将变量和混合定义在单独的文件中（如 `variables.less` 和 `mixins.less`）
2. 使用有意义的变量名
3. 合理组织文件结构，使用 `@import` 管理依赖
4. 避免过度嵌套（一般不超过 3 层）
5. 在开发环境中使用 source map 方便调试

通过使用 Less，你可以编写更简洁、更易维护的样式代码，并利用其强大的特性提高开发效率。