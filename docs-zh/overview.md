---
id: overview
title: Overview
---

让我们面对现实，[React](https://github.com/facebook/react)的(form)表单真的很冗长。更糟糕的是，大多数表单的帮助库都做了太多魔术(wayyyy)，并且往往会产生与之相关的显着性能成本。Formik 是一个小型库，可以帮助您解决 3 个最烦人的部分:

- 1. **获取表单状态的值和表单状态**
- 2. **验证和错误消息**
- 3. **处理表单提交**

formik 通过把上面的所有内容放在一个地方，使事情变得井然有序——让您的表单的测试、重构和理解变得轻而易举。

## 动机

我（我）[jaredpalmer@](https://twitter.com/jaredpalmer)）在构建[@eonwhite](https://twitter.com/eonwhite)大型内部管理仪表盘搞出了 Formik。 由于大约有 30 种独特的表单，很快我们就可以通过标准化输入组件以及数据，以此贯通表单联系的方式，很有帮助。

### 为什么不用 Redux-Form？

现在，你可能在想，“你为什么不直接用[Redux-Form](https://github.com/erikras/redux-form)？”，这问得好

1.  据我们的'先知' Dan Abramov 所说，[**form 状态本质上是短暂的且局部的** 因此，在 redux（或任何类型的 flux 库）中跟踪它是不必要的。](https://github.com/reactjs/redux/issues/1287#issuecomment-175351978)
2.  每次触发一个键(ON EVERY
    SINGLE KEYSTROKE)，Redux-Form 都会多次调用整个顶级 Redux Reducer。对于小型应用程序来说，这很好，但是随着 Redux 应用程序的增长，如果使用 Redux-Form，输入延迟将继续增加。
3.  Redux-Form 缩 gzip 格式为 22.5 kb （Formik 为 12.7 kb）

**formik 的目标是创建一个可扩展的、性能良好的表单助手，它使用一个最小的 API 来完成真正厌烦的事情，剩下的就交给你了。**

---

我在 React Alicante 的演讲更深入地探讨了 Formik 的动机和理念，介绍了该库（观看我构建一个迷你版本），并演示了如何使用生产上的东西构建一个非凡的表单（使用数组、自定义输入等）。

<iframe width="600" height="315" src="https://www.youtube.com/embed/oiNtnehlaTo" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen title="Taming Forms in React - Jared Palmer"></iframe>

## 影响

Formik 由[Brent Jackson](https://github.com/jxnblk)的[这个小的高阶组件](https://github.com/jxnblk/rebass-recomposed/blob/master/src/withForm.js)开始诞生的，，具有 Redux-Form 的一些命名约定，以及（最近）[React-Motion](https://github.com/chenglou/react-motion)和[React-Router 4](https://github.com/ReactTraining/react-router)的渲染 props 方法。 无论你是否使用过上述任何一种，Formik 只需要几分钟就可以入门。

## 安装

您可以用[NPM](https://npmjs.com)，[yarn](https://yarnpkg.com)安装，或者一个好的`<script>`[unpkg.com](https://unpkg.com).

### NPM

```sh
npm install formik --save
# or
yarn add formik
```

formik 与 react v15+兼容，与 reactdom 和 react native 兼容。

你可以 **[在 codesandbox.io 上演示 formik。](https://codesandbox.io/s/zKrK5YLDZ)**，不甜不收钱。

### CDN

如果您不使用模块绑定器或包管理器，我们也有一个全局（“UMD”）构建托管在[unpkg.com](https://unpkg.com)CDN。只需添加以下内容`<script>`标签到 HTML 文件的底部：

```html
<script src="https://unpkg.com/formik/dist/formik.umd.production.js"></script>
```

添加后，您将可以访问`window.Formik.<Insert_Component_Name_Here>`变量。

> 此安装方式/的使用，是要求[React cdn 脚本包](https://reactjs.org/docs/cdn-links.html)也会出现在页面上。

### 在浏览器中的游乐场

你可以在网络浏览器中，使用这些在线游戏场来玩 formik。

- CodeSandbox（reactdom）<https://codesandbox.io/s/zKrK5YLDZ>
- Expo Snack（React Native）<https://snack.expo.io/Bk9pPK87X>

## 要旨

formik 跟踪表单的状态，然后将其公开，再加上一些可重用的方法和事件处理程序（`handleChange`，`handleBlur`和`handleSubmit`），本质上是通过`props`. `handleChange`和`handleBlur`来达到预期工作 —— 他们使用`name`或`id`属性来确定要更新的字段。

```jsx
import React from 'react';
import {Formik} from 'formik';

const Basic = () => (
  <div>
    <h1>Anywhere in your app!</h1>
    <Formik
      initialValues={{email: '', password: ''}}
      validate={values => {
        let errors = {};
        if (!values.email) {
          errors.email = 'Required';
        } else if (
          !/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,}$/i.test(values.email)
        ) {
          errors.email = 'Invalid email address';
        }
        return errors;
      }}
      onSubmit={(values, {setSubmitting}) => {
        setTimeout(() => {
          alert(JSON.stringify(values, null, 2));
          setSubmitting(false);
        }, 400);
      }}
    >
      {({
        values,
        errors,
        touched,
        handleChange,
        handleBlur,
        handleSubmit,
        isSubmitting
        /* and other goodies */
      }) => (
        <form onSubmit={handleSubmit}>
          <input
            type="email"
            name="email"
            onChange={handleChange}
            onBlur={handleBlur}
            value={values.email}
          />
          {errors.email && touched.email && errors.email}
          <input
            type="password"
            name="password"
            onChange={handleChange}
            onBlur={handleBlur}
            value={values.password}
          />
          {errors.password && touched.password && errors.password}
          <button type="submit" disabled={isSubmitting}>
            Submit
          </button>
        </form>
      )}
    </Formik>
  </div>
);

export default Basic;
```

### 还原样板

上面的代码非常明确地说明了 formik 正在做什么。`onChange`->`handleChange`，`onBlur`->`handleBlur`等等。不过，为了节省时间，Formik 还提供了一些额外的组件，以使生活更轻松、更不冗长：`<Form />`，`<Field />`和`<ErrorMessage />`. 它们使用 react 上下文，来钩住父级`<Formik />`的状态/方法。

```jsx
// Render Prop
import React from 'react';
import {Formik, Form, Field, ErrorMessage} from 'formik';

const Basic = () => (
  <div>
    <h1>Any place in your app!</h1>
    <Formik
      initialValues={{email: '', password: ''}}
      validate={values => {
        let errors = {};
        if (!values.email) {
          errors.email = 'Required';
        } else if (
          !/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,}$/i.test(values.email)
        ) {
          errors.email = 'Invalid email address';
        }
        return errors;
      }}
      onSubmit={(values, {setSubmitting}) => {
        setTimeout(() => {
          alert(JSON.stringify(values, null, 2));
          setSubmitting(false);
        }, 400);
      }}
    >
      {({isSubmitting}) => (
        <Form>
          <Field type="email" name="email" />
          <ErrorMessage name="email" component="div" />
          <Field type="password" name="password" />
          <ErrorMessage name="password" component="div" />
          <button type="submit" disabled={isSubmitting}>
            Submit
          </button>
        </Form>
      )}
    </Formik>
  </div>
);

export default Basic;
```

### 补充包

正如您在上面看到的，验证由您自己决定。您可以编写自己的验证器或使用第三方库。我个人用[Yup](https://github.com/jquense/yup)搞定对象结构验证。它有一个与[Joi](https://github.com/hapijs/joi) / [React PropTypes](https://github.com/facebook/prop-types)非常相似的 API，但是对于浏览器来说足够小，对于浏览引擎的使用来说足够快。因为 I :heart: Yup sooo much，Formik 为 yup 提供了一个特殊的配置 options /props，叫做[`validationSchema`](api/formik.md#validationschema-schema-schema)，它会自动将 yup 的验证错误转换成一个, key 匹配[`values`](api/formik.md#values-field-string-any)和[`touched`](api/formik.md#touched-field-string-boolean)的漂亮对象。 总的来说，你可以从 NPM 安装 Yup…

```
npm install yup --save
```
