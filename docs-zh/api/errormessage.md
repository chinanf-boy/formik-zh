---
id: errormessage
title: <ErrorMessage />
custom_edit_url: https://github.com/jaredpalmer/formik/edit/master/docs/api/errormessage.md
---
`<ErrorMessage />`是一个组件，如果访问了该字段，则呈现给定字段的错误消息（即`touched[name] === true`）（并且有一个`error`留言）。它期望所有错误消息都作为字符串存储在给定字段中。喜欢`<Field />`，`<FastField />`，和`<FieldArray />`，支持类似lodash的点路径和括号语法。

## 例

```diff
import React from 'react';
import { Formik, Form, Field, ErrorMessage } from 'formik';
import * as Yup from "yup";

const SignupSchema = Yup.object().shape({
  name: Yup.string()
    .min(2, 'Too Short!')
    .max(70, 'Too Long!')
    .required('Required'),
  email: Yup.string()
    .email('Invalid email')
    .required('Required'),
});

export const ValidationSchemaExample = () => (
  <div>
    <h1>Signup</h1>
    <Formik
      initialValues={{
        name: '',
        email: '',
      }}
      validationSchema={SignupSchema}
      onSubmit={values => {
        // same shape as initial values
        console.log(values);
      }}
    >
      {({ errors, touched }) => (
        <Form>
          <Field name="name"  />
-           {errors.name && touched.name ? (
-            <div>{errors.name}</div>
-          ) : null}
+         <ErrorMessage name="name" />
          <Field name="email" type="email" />
-           {errors.email && touched.email ? (
-            <div>{errors.email}</div>
-          ) : null}
+         <ErrorMessage name="email" />
          <button type="submit">Submit</button>
        </Form>
      )}
    </Formik>
  </div>
);
```

#### 道具

\<AUTOGENERATED_TABLE_OF_CONTENTS>

* * *

# 参考

## 道具

### `children`

`children?: ((message: string) => React.ReactNode)`

一个返回有效React元素的函数。只有在触摸了字段并且存在错误时才会调用。

```jsx
// the render callback will only be called when the
// field has been touched and an error exists and subsequent updates.
<ErrorMessage name="email">{msg => <div>{msg}</div>}</ErrorMessage>
```

### `component`

`component?: string | React.ComponentType<FieldProps>`

要么是React组件，要么是要呈现的HTML元素的名称。如果没有指定，`<ErrorMessage>`只会返回一个字符串。

```jsx
<ErrorMessage component="div" name="email" />
// --> {touched.email && error.email ? <div>{error.email}</div> : null}

<ErrorMessage component="span" name="email" />
// --> {touched.email && error.email ? <span>{error.email}</span> : null}

<ErrorMessage component={Custom} name="email" />
// --> {touched.email && error.email ? <Custom>{error.email}</Custom> : null}

<ErrorMessage name="email" />
// This will return a string. React 16+.
// --> {touched.email && error.email ? error.email : null}
```

### `name`

`name: string`
**需要**

Formik状态下的字段名称。要访问嵌套对象或数组，name也可以接受像lodash一样的点路径`social.facebook`要么`friends[0].firstName`

### `render`

`render?: (error: string) => React.ReactNode`

一个返回有效React元素的函数。只有在触摸了字段并且存在错误时才会调用。

```jsx
// the render callback will only be called when the
// field has been touched and an error exists and subsequent updates.
<ErrorMessage name="email" render={msg => <div>{msg}</div>} />
```