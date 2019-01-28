---
id: tutorial
title: 教程
---

## 通过构建 formik，来学习 它

我在 React Alicante 的演讲确实是开始的最好方式。它介绍了库（通过观看我构建它的迷你版本），并演示了如何使用现实的东西构建一个非凡的表单（使用数组、自定义输入等）。大约 45 分钟长。

<iframe width="800" height="315" src="https://www.youtube.com/embed/oiNtnehlaTo" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen title="Taming Forms in React - Jared Palmer"></iframe>

## 基础

_如果您只想跳到一些代码中，这里有一个基本的快速演练…_

假设您想要构建一个允许您编辑用户数据的表单。但是，您的用户 API 有类似这样的嵌套对象。

```js
{
   id: string,
   email: string,
   social: {
     facebook: string,
     twitter: string,
     // ...
   }
}
```

```jsx
// EditUserDialog.js
import React from 'react';
import Dialog from 'MyImaginaryDialogComponent'; // 这不是真的包, 假设它存在就好了
import {Formik} from 'formik';

const EditUserDialog = ({user, updateUser, onClose}) => {
  return (
    <Dialog onClose={onClose}>
      <h1>Edit User</h1>
      <Formik
        initialValues={user /** { email, social } */}
        onSubmit={(values, actions) => {
          MyImaginaryRestApiCall(user.id, values).then(
            updatedUser => {
              actions.setSubmitting(false);
              updateUser(updatedUser);
              onClose();
            },
            error => {
              actions.setSubmitting(false);
              actions.setErrors(transformMyRestApiErrorsToAnObject(error));
              actions.setStatus({msg: 'Set some arbitrary status or data'});
            }
          );
        }}
        render={({
          values,
          errors,
          status,
          touched,
          handleBlur,
          handleChange,
          handleSubmit,
          isSubmitting
        }) => (
          <form onSubmit={handleSubmit}>
            <input
              type="email"
              name="email"
              onChange={handleChange}
              onBlur={handleBlur}
              value={values.email}
            />
            {errors.email && touched.email && <div>{errors.email}</div>}
            <input
              type="text"
              name="social.facebook"
              onChange={handleChange}
              onBlur={handleBlur}
              value={values.social.facebook}
            />
            {errors.social && errors.social.facebook && touched.facebook && (
              <div>{errors.social.facebook}</div>
            )}
            <input
              type="text"
              name="social.twitter"
              onChange={handleChange}
              onBlur={handleBlur}
              value={values.social.twitter}
            />
            {errors.social && errors.social.twitter && touched.twitter && (
              <div>{errors.social.twitter}</div>
            )}
            {status && status.msg && <div>{status.msg}</div>}
            <button type="submit" disabled={isSubmitting}>
              Submit
            </button>
          </form>
        )}
      />
    </Dialog>
  );
};
```

为了书写表单不那么冗长。Formik 有几个助手来帮你保存按键。

- `<Field>`
- `<Form />`

以下内容与之前的表单**完全**相同，但使用了`<Form />`和`<Field />`，：

```jsx
// EditUserDialog.js
import React from 'react';
import Dialog from 'MySuperDialog';
import {Formik, Field, Form} from 'formik';

const EditUserDialog = ({user, updateUser, onClose}) => {
  return (
    <Dialog onClose={onClose}>
      <h1>Edit User</h1>
      <Formik
        initialValues={user /** { email, social } */}
        onSubmit={(values, actions) => {
          MyImaginaryRestApiCall(user.id, values).then(
            updatedUser => {
              actions.setSubmitting(false);
              updateUser(updatedUser);
              onClose();
            },
            error => {
              actions.setSubmitting(false);
              actions.setErrors(transformMyRestApiErrorsToAnObject(error));
              actions.setStatus({msg: 'Set some arbitrary status or data'});
            }
          );
        }}
        render={({errors, status, touched, isSubmitting}) => (
          <Form>
            <Field type="email" name="email" />
            {errors.email && touched.email && <div>{errors.email}</div>}
            <Field type="text" name="social.facebook" />
            {errors.social.facebook && touched.social.facebook && (
              <div>{errors.social.facebook}</div>
            )}
            <Field type="text" name="social.twitter" />
            {errors.social.twitter && touched.social.twitter && (
              <div>{errors.social.twitter}</div>
            )}
            {status && status.msg && <div>{status.msg}</div>}
            <button type="submit" disabled={isSubmitting}>
              Submit
            </button>
          </Form>
        )}
      />
    </Dialog>
  );
};
```

这更好，但所有这些`errors`和`touched`逻辑仍然是相当重复的。formik 有一个组件`<ErrorMessage>`，可以简化更多的事情。它接受渲染 props 或组件 props 以获得最大的灵活性。

```jsx
// EditUserDialog.js
import React from 'react';
import Dialog from 'MySuperDialog';
import {Formik, Field, Form, ErrorMessage} from 'formik';

const EditUserDialog = ({user, updateUser, onClose}) => {
  return (
    <Dialog onClose={onClose}>
      <h1>Edit User</h1>
      <Formik
        initialValues={user /** { email, social } */}
        onSubmit={(values, actions) => {
          MyImaginaryRestApiCall(user.id, values).then(
            updatedUser => {
              actions.setSubmitting(false);
              updateUser(updatedUser);
              onClose();
            },
            error => {
              actions.setSubmitting(false);
              actions.setErrors(transformMyRestApiErrorsToAnObject(error));
              actions.setStatus({msg: 'Set some arbitrary status or data'});
            }
          );
        }}
        render={({errors, status, touched, isSubmitting}) => (
          <Form>
            <Field type="email" name="email" />
            <ErrorMessage name="email" component="div" />
            <Field type="text" className="error" name="social.facebook" />
            <ErrorMessage name="social.facebook">
              {errorMessage => <div className="error">{errorMessage}</div>}
            </ErrorMessage>
            <Field type="text" name="social.twitter" />
            <ErrorMessage
              name="social.twitter"
              className="error"
              component="div"
            />
            {status && status.msg && <div>{status.msg}</div>}
            <button type="submit" disabled={isSubmitting}>
              Submit
            </button>
          </Form>
        )}
      />
    </Dialog>
  );
};
```
