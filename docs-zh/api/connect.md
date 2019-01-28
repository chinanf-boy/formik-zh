---
id: connect
title: connect()
custom_edit_url: https://github.com/jaredpalmer/formik/edit/master/docs/api/connect.md
---

`connect()`是一个高阶组件（HoC），允许您将任何内容挂钩到 Formik 的上下文中。它在内部用于构建`<Field>`和`<Form>`，但您可以根据需求的变化使用它来构建新的组件。

## 类型签名

```tsx
connect<OuterProps, Values = any>(Comp: React.ComponentType<OuterProps & FormikProps<Values>>) => React.ComponentType<OuterProps>
```

## 例

```jsx
import React from 'react';
import {connect, getIn} from 'formik';

// 如果字段有一个错误，
// 且它已经被触动了。该组件渲染一个错误信息
const ErrorMessage = props => {
  // props.formik上提供所有 FormikProps！
  const error = getIn(props.formik.errors, props.name);
  const touch = getIn(props.formik.touched, props.name);
  return touch && error ? error : null;
};

export default connect(ErrorMessage);
```
