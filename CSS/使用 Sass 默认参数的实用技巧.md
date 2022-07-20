# 使用 Sass 默认参数的实用技巧

Sass 提供[接受参数](https://sass-lang.com/documentation/at-rules/mixin#arguments)的函数和 `mixin`。您可以使用 Sass 默认参数，即具有值的参数，即使在调用函数或 `mixin` 时未提供这些参数。

本文主要关注 `mixin`。以下是 `mixin` 的语法：

```scss
@mixin foo($a, $b, $c) {
  // 我可以在这里使用 $a、$b 和 $c，但它们可能为空
}

.el {
  @include foo(1, 2, 3);

  // 如果我们试着使用 `@include foo`，不传任何参数，这是有效的语法
  // 但会从 Sass 那里得到 Error: Missing argument $a
}
```

在此 Sass `mixin` 中设置默认参数更安全、更有用：

```scss
@mixin foo($a: 1, $b: 2, $c: 3) {
}

.el {
  @include foo;

  @include foo('three', 'little', 'pigs');
}
```

但是如果我想传递 `$b` 和 `$c`，但将 `$a` 保留为 Sass 默认参数，该怎么办？诀窍是您需要指定 `named` 参数：

```scss
@mixin foo($a: 1, $b: 2, $c: 3) {
}

.el {
  @include foo($b: 2, $c: 3);
}
```

## 使用 Sass 默认参数的示例

这是一个快速的 `mixin`，它输出你需要的非常基本的样式滚动条（[Kitty 也有一个](https://css-tricks.com/snippets/sass/custom-scrollbars-mixin/)）：

```scss
@mixin scrollbar(
  $size: 10px,
  $foreground-color: #eee,
  $background-color: #333
) {
  // 仅支持 WebKit 内核的浏览器
  &::-webkit-scrollbar {
    width: $size;
    height: $size;
  }
  &::-webkit-scrollbar-thumb {
    background: $foreground-color;
  }
  &::-webkit-scrollbar-track {
    background: $background-color;
  }

  // 标准版本（目前仅限 Firefox）
  scrollbar-color: $foreground-color $background-color;
}
```

现在我们可以以多种形式调用它：

```scss
.scrollable {
  @include scrollbar;
}

.thick-but-otherwise-default-scrollable {
  // 我可以跳过 $b 和 $c，因为它们是第二和第三
  @include scrollbar(30px);
}

.custom-colors-scrollable {
  // 如果所有其他参数都已命名，我可以跳过第一个参数。
  @include scrollbar($foreground-color: orange, $background-color: black);
}

.totally-custom-scrollable {
  @include scrollbar(20px, red, black);
}
```

> **注意**：指定空字符串或 `null` 无法将不起作用。必须使用命名参数方法。
