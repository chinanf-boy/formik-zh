---
id: form-submission
title: Form 提交
custom_edit_url: https://github.com/jaredpalmer/formik/edit/master/docs/guides/form-submission.md
---

## 提交阶段

要在 Formik 中提交表单，您需要以某种方式触发所提供的`handleSubmit(e)`或`submitForm`prop。当您调用其中任何一种方法时，Formik 每次都执行以下操作 _(伪代码)_ ：

### 预提交

- 触摸所有字段
- 设`isSubmitting`为`true`
- 增`submitCount`+ 1

### 验证

- 设`isValidating`为`true`
- 运行所有字段级验证,`validate`，和`validationSchema`异步并深度合并结果
- 有什么错误吗？
  - 有：中止提交。设`isValidating`为`false`，设定`errors`，设定`isSubmitting`为`false`
  - 没有：设置`isValidating`为`false`，进入“提交”

### 提交

- 继续运行您的提交处理程序（即`onSubmit`要么`handleSubmit`）
- *你调用`setSubmitting(false)`*在你的处理程序中完成循环

## 经常问的问题

<details>
<summary>如何确定提交处理程序是否正在执行？</summary>

如果`isValidating`是`false`，和`isSubmitting`是`true`。

</details>

<details>
<summary>为什么提交前,Formik 会触摸所有字段？?</summary>

通常的做法是只在访问过（a.k.a“触摸”）的 UI 中显示输入错误。在提交表单之前，Formik 会触及所有字段，以便所有可能已隐藏的错误现在看得见。

</details>

<details>
<summary>如何防止重复提交?</summary>

如果`isSubmitting`是`true`，禁用触发提交的任何内容。

</details>

<details>
<summary>我如何知道我的表单在提交前是何时验证的？?</summary>

如果`isValidating`是`true`，和`isSubmitting`是`true`。

</details>
