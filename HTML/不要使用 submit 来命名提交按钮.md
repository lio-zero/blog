# 不要使用 submit 来命名提交按钮

给定表单元素，我们通常在验证表单字段后调用 `submit()` 方法来提交表单。

如果表单的提交按钮具有 `name="submit"` 或 `id="submit"` 属性，则 `formEle.submit` 将返回提交按钮实例。因此，`formEle.submit()` 抛出异常，因为它不再是实际函数。

在使用特殊的表单属性（如 `reset`、`length`、`method`）时，我们可能会遇到类似的问题。

```html
<!-- 不要这样做 -->
<button type="submit" name="submit">Submit</button>
<button type="submit" id="submit">Submit</button>

<!-- 这样做 -->
<button type="submit" name="submitButton">Submit</button>
```
