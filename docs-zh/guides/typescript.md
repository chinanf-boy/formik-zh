---
id: typescript
title: TypeScript
custom_edit_url: https://github.com/jaredpalmer/formik/edit/master/docs/guides/typescript.md
---

[![TypeScript Types](https://img.shields.io/npm/types/formik.svg)](https://npm.im/formik)

Formik 源代码是用 TypeScript 编写的，因此您放心， Formik 的类型始终是最新的。作为思维模型，Formik 的类型签名与 React Router 4 的`<Route>`非常相似。

#### 渲染 props（`<Formik />`和`<Field />`）

```typescript
import * as React from 'react';
import {Formik, FormikProps, Form, Field, FieldProps} from 'formik';

interface MyFormValues {
  firstName: string;
}

export const MyApp: React.SFC<{}> = () => {
  return (
    <div>
      <h1>My Example</h1>
      <Formik
        initialValues={{firstName: ''}}
        onSubmit={(values: MyFormValues) => alert(JSON.stringify(values))}
        render={(formikBag: FormikProps<MyFormValues>) => (
          <Form>
            <Field
              name="firstName"
              render={({field, form}: FieldProps<MyFormValues>) => (
                <div>
                  <input type="text" {...field} placeholder="First Name" />
                  {form.touched.firstName &&
                    form.errors.firstName &&
                    form.errors.firstName}
                </div>
              )}
            />
          </Form>
        )}
      />
    </div>
  );
};
```

#### `withFormik()`

```typescript
import React from 'react';
import * as Yup from 'yup';
import {withFormik, FormikProps, FormikErrors, Form, Field} from 'formik';

// form 值的样子
interface FormValues {
  email: string;
  password: string;
}

interface OtherProps {
  message: string;
}

// 旁白: 你可以看到 InjectedFormikProps<OtherProps, FormValues> 替代了 旧代码的下面.. 当 Formik 仅导出一个 HoC，InjectedFormikProps 是工件， 这也少了些灵活性 因它 必须包裹所有 props (传递所有).
const InnerForm = (props: OtherProps & FormikProps<FormValues>) => {
  const {touched, errors, isSubmitting, message} = props;
  return (
    <Form>
      <h1>{message}</h1>
      <Field type="email" name="email" />
      {touched.email && errors.email && <div>{errors.email}</div>}

      <Field type="password" name="password" />
      {touched.password && errors.password && <div>{errors.password}</div>}

      <button type="submit" disabled={isSubmitting}>
        Submit
      </button>
    </Form>
  );
};

// MyForm接收的 props 类型
interface MyFormProps {
  initialEmail?: string;
  message: string; // 如果全传递，则可以执行此操作或生成联合类型
}

// 使用 HoC withFormik 包装我们的表单
const MyForm = withFormik<MyFormProps, FormValues>({
  // 将外部 props 转换为形式值
  mapPropsToValues: props => {
    return {
      email: props.initialEmail || '',
      password: ''
    };
  },

  // 添加自定义验证函数（这也可以是异步的！）
  validate: (values: FormValues) => {
    let errors: FormikErrors = {};
    if (!values.email) {
      errors.email = 'Required';
    } else if (!isValidEmail(values.email)) {
      errors.email = 'Invalid email address';
    }
    return errors;
  },

  handleSubmit: values => {
    // 做提交的事情
  }
})(InnerForm);

// 其中使用<MyForm />
const Basic = () => (
  <div>
    <h1>My App</h1>
    <p>This can be anywhere in your application</p>
    <MyForm message="Sign up" />
  </div>
);

export default Basic;
```
