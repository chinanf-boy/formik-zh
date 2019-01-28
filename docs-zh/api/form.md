---
id: form
title: <Form />
custom_edit_url: https://github.com/jaredpalmer/formik/edit/master/docs/api/form.md
---

Form 是一个围绕 HTML `<form>`元素的小包装器，自动挂钩到 Formik 的`handleSubmit`和`handleReset`。所有其他 props 直接传递到 DOM 节点。

```jsx
// so...
<Form />

// 与此，相同...
<form onReset={formikProps.handleReset} onSubmit={formikProps.handleSubmit} {...props} />
```
