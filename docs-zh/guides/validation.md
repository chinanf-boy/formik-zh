---
id: validation
title: 验证
custom_edit_url: https://github.com/jaredpalmer/formik/edit/master/docs/guides/validation.md
---

Formik 旨在轻松管理具有复杂验证的表单。Formik 支持同步和异步，表单级和字段级验证。此外，它还通过 Yup ，提供基于模式的表单级验证支持。本指南将描述上述所有内容的来龙去脉。

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [验证的味道](#%E9%AA%8C%E8%AF%81%E7%9A%84%E5%91%B3%E9%81%93)
  - [表单级验证](#%E8%A1%A8%E5%8D%95%E7%BA%A7%E9%AA%8C%E8%AF%81)
    - [`validate`](#validate)
    - [`validationSchema`](#validationschema)
  - [字段级验证](#%E7%8E%B0%E5%9C%BA%E7%BA%A7%E9%AA%8C%E8%AF%81)
    - [`validate`](#validate-1)
  - [手动触发验证](#%E6%89%8B%E5%8A%A8%E8%A7%A6%E5%8F%91%E9%AA%8C%E8%AF%81)
- [验证何时运行？](#%E9%AA%8C%E8%AF%81%E4%BD%95%E6%97%B6%E8%BF%90%E8%A1%8C)
- [显示错误消息](#%E6%98%BE%E7%A4%BA%E9%94%99%E8%AF%AF%E6%B6%88%E6%81%AF)
- [常见问题](#%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## 验证的味道

### 表单级验证

表单级验证非常有用，因为您通过`values`和 props，可以完全访问所有表单，并且只要函数运行，这样您就可以同时验证依赖字段。

有两种方法可以使用 Formik 进行表单级验证：

- `<Formik validate>`和`withFormik({ validate: ... })`
- `<Formik validationSchema>`和`withFormik({ validationSchema: ... })`

#### `validate`

`<Formik>`和`withFormik()`采取 prop/option 调用`validate`，接受同步或异步函数。

```js
// Synchronous validation
const validate = (values, props /* only available when using withFormik */) => {
  let errors = {};

  if (!values.email) {
    errors.email = 'Required';
  } else if (!/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i.test(values.email)) {
    errors.email = 'Invalid email address';
  }

  //...

  return errors;
};

// Async Validation
const sleep = ms => new Promise(resolve => setTimeout(resolve, ms));

const validate = (values, props /* only available when using withFormik */) => {
  return sleep(2000).then(() => {
    let errors = {};
    if (['admin', 'null', 'god'].includes(values.username)) {
      errors.username = 'Nice try';
    }
    // ...
    if (Object.keys(errors).length) {
      throw errors;
    }
  });
};
```

有关`<Formik validate>`的更多信息，请参阅 API 参考。

#### `validationSchema`

如您所见，验证交由你决定。您可以随意编写自己的验证器或使用第三方库。在 The Palmer 集团，我们使用[Yup](https://github.com/jquense/yup)，来完成对象模式验证。它与[穰](https://github.com/hapijs/joi)和[反应 PropTypes](https://github.com/facebook/prop-types)有非常类似的 API，但是对浏览器而言足够小，并且对于运行引擎的使用来说足够快。因为我们:heart: Yup sooo much，Formik 有一个特殊的配置 option/ prop 用于 Yup 对象模式，叫作`validationSchema`，它自动将 Yup 的验证错误转换为 key 匹配`values`和`touched`的漂亮对象。这种对称性使得可以轻松地围绕错误消息，管理业务逻辑。

要将 Yup 添加到项目中，请从 NPM 安装它。

```sh
npm install yup --save
# typescript 用户应添加 @types/yup
```

```jsx
import React from 'react';
import {Formik, Form, Field} from 'formik';
import * as Yup from 'yup';

const SignupSchema = Yup.object().shape({
  firstName: Yup.string()
    .min(2, 'Too Short!')
    .max(50, 'Too Long!')
    .required('Required'),
  lastName: Yup.string()
    .min(2, 'Too Short!')
    .max(50, 'Too Long!')
    .required('Required'),
  email: Yup.string()
    .email('Invalid email')
    .required('Required')
});

export const ValidationSchemaExample = () => (
  <div>
    <h1>Signup</h1>
    <Formik
      initialValues={{
        firstName: '',
        lastName: '',
        email: ''
      }}
      validationSchema={SignupSchema}
      onSubmit={values => {
        // same shape as initial values
        console.log(values);
      }}
    >
      {({errors, touched}) => (
        <Form>
          <Field name="firstName" />
          {errors.firstName && touched.firstName ? (
            <div>{errors.firstName}</div>
          ) : null}
          <Field name="lastName" />
          {errors.lastName && touched.lastName ? (
            <div>{errors.lastName}</div>
          ) : null}
          <Field name="email" type="email" />
          {errors.email && touched.email ? <div>{errors.email}</div> : null}
          <button type="submit">Submit</button>
        </Form>
      )}
    </Formik>
  </div>
);
```

有关`<Formik validationSchema>`的更多信息，请参阅 API 参考。

### 字段级验证

#### `validate`

Formik 通过`<Field>`/`<FastField>`组件的`validate`支持字段级验证。此函数可以是同步的或异步的（返回 Promise）。默认情况下它将在任一`onChange`和`onBlur`之后运行。此行为可让分别使用`validateOnChange`和`validateOnBlur`props 的顶层`<Formik/>`组件改变。除了 change/blur 之外，所有字段级验证都在尝试提交开始时运行，然后结果与任何顶级验证结果深度合并。

> 注意：`<Field>/<FastField>`组件'`validate`函数只能在已装载的字段上执行。也就是说，如果您的任何字段在表单流程中卸载（例如 Material-UI 的`<Tabs>`卸下前一个您的用户开启的`<Tab>`），在表单验证/提交期间不会验证这些字段。

```jsx
import React from 'react';
import {Formik, Form, Field} from 'formik';
import * as Yup from 'yup';

function validateEmail(value) {
  let error;
  if (!value) {
    error = 'Required';
  } else if (!/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i.test(value)) {
    error = 'Invalid email address';
  }
  return error;
}

function validateUsername(value) {
  let error;
  if (value === 'admin') {
    error = 'Nice try!';
  }
  return error;
}

export const FieldLevelValidationExample = () => (
  <div>
    <h1>Signup</h1>
    <Formik
      initialValues={{
        username: '',
        email: ''
      }}
      onSubmit={values => {
        // same shape as initial values
        console.log(values);
      }}
    >
      {({errors, touched, isValidating}) => (
        <Form>
          <Field name="email" validate={validateEmail} />
          {errors.email && touched.email && <div>{errors.email}</div>}

          <Field name="username" validate={validateUsername} />
          {errors.username && touched.username && <div>{errors.username}</div>}

          <button type="submit">Submit</button>
        </Form>
      )}
    </Formik>
  </div>
);
```

### 手动触发验证

您可以使用 Formik 手动触发，表单级和字段级验证，分别为`validateForm`和`validateField`方法。

```jsx
import React from 'react';
import { Formik, Form, Field } from 'formik';

function validateEmail(value) {
  let error;
  if (!value) {
    error = 'Required';
  } else if (!/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i.test(value)) {
    error = 'Invalid email address';
  }
  return error;
}

function validateUsername(value) {
  let error;
  if (value === 'admin') {
    error = 'Nice try!';
  }
  return error;
}

export const FieldLevelValidationExample = () => (
  <div>
    <h1>Signup</h1>
    <Formik
      initialValues={{
        username: '',
        email: '',
      }}
      onSubmit={values => {
        // same shape as initial values
        console.log(values);
      }}
    >
      {({ errors, touched, validateField, validateForm }) => (
        <Form>
          <Field name="email" validate={validateEmail} />
          {errors.email && touched.email && <div>{errors.email}</div>}

          <Field name="username" validate={validateUsername} />
          {errors.username && touched.username && <div>{errors.username}</div>}
          {/** Trigger field-level validation
           imperatively */}
          <button type="button" onClick={() => validateField('username')}>
            Check Username
          </button>
          {/** Trigger form-level validation
           imperatively */}
          <button type="button" onClick={() => validateForm().then(() => console.log('blah')))}>
            Validate All
          </button>
          <button type="submit">Submit</button>
        </Form>
      )}
    </Formik>
  </div>
);
```

## 验证何时运行？

您可以通过更改`<Formik validateOnChange>`和/或`<Formik validateOnBlur>`props 的值，来控制 Formik 何时运行验证，这都取决于你的需要。默认情况下，Formik 将运行以下验证方法：

**“change”事件/方法之后**（更新`values`）

- `handleChange`
- `setFieldValue`
- `setValues`

**在“blur”事件/方法之后**（更新`touched`）

- `handleBlur`
- `setTouched`
- `setFieldTouched`

**每当试图提交时**

- `handleSubmit`
- `submitForm`

还通过 Formik 的呈现/注入 props 向您提供了必要的助手方法，您可以使用这些方法强制调用验证。

- `validateForm`
- `validateField`

## 显示错误消息

@todo

## 常见问题

<details>
<summary>如何确定表单是否正在验证？</summary>

如果`isValidating`prop 是`true`

</details>

<details>
<summary>我可以返回“null”作为错误消息吗？</summary>

不，使用`undefined`。Formik 使用`undefined`表示空状态。如果你使用`null`，Formik 计算出的几个部分（例如`isValid`），将无法按预期工作。

</details>

<details>
<summary>如何测试验证？</summary>

formik 有大量的单元测试用于 yup 验证，因此您不需要测试它。但是，如果您正在滚动自己的验证函数，那么您应该简单地对它们进行单元测试。如果您确实需要测试 formik 的执行情况，您应该分别使用指令式`validateForm`和`validateField`方法。

</details>
