---
id: form-submission
title: Form Submission
custom_edit_url: https://github.com/jaredpalmer/formik/edit/master/docs/guides/form-submission.md
---
## 提交阶段

要在Formik中提交表单，您需要以某种方式触发所提供的表单`handleSubmit(e)`要么`submitForm`支柱。当您调用其中任何一种方法时，Formik将执行以下操作*（伪代码）*每一次：

### 预提交

-   触摸所有字段
-   组`isSubmitting`至`true`
-   增量`submitCount`+ 1

### 验证

-   组`isValidating`至`true`
-   运行所有字段级验证，`validate`，和`validationSchema`异步并深度合并结果
-   有什么错误吗？
    -   是：中止提交。组`isValidating`至`false`，设定`errors`，设定`isSubmitting`至`false`
    -   不：设置`isValidating`至`false`，进入“提交”

### 服从

-   继续运行您的提交处理程序（即`onSubmit`要么`handleSubmit`）
-   *你打电话`setSubmitting(false)`*在你的处理程序中完成循环

## 经常问的问题

<details>
<summary>How do I determine if my submission handler is executing?</summary>

如果`isValidating`是`false`和`isSubmitting`是`true`。

</details>

<details>
<summary>Why does Formik touch all fields before submit?</summary>

通常的做法是只在访问过的UI中显示输入错误（a.k.a“触摸”）。在提交表单之前，Formik会触及所有字段，以便现在可以隐藏所有可能已隐藏的错误。

</details>

<details>
<summary>How do I protect against double submits?</summary>

禁用触发提交的任何内容`isSubmitting`是`true`。

</details>

<details>
<summary>How do I know when my form is validating before submit?</summary>

如果`isValidating`是`true`和`isSubmitting`是`true`。

</details>
