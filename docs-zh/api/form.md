---
id: form
title: <Form />
custom_edit_url: https://github.com/jaredpalmer/formik/edit/master/docs/api/form.md
---
Form是一个围绕HTML的小包装器`<form>`自动挂钩到Formik的元素`handleSubmit`和`handleReset`。所有其他道具直接传递到DOM节点。

```jsx
// so...
<Form />

// is identical to this...
<form onReset={formikProps.handleReset} onSubmit={formikProps.handleSubmit} {...props} />
```
