---
id: arrays
title: Arrays 和嵌套 Objects
custom_edit_url: https://github.com/jaredpalmer/formik/edit/master/docs/guides/arrays.md
---

Formik 支持嵌套对象和开箱即用的数组。这两个主题有些相关，因为它们都使用相同的语法。

## 嵌套对象

Formik 中的`name` props 可以使用类似 lodash 的点路径，来引用嵌套的 Formik 值。这意味着您不再需要压平表单的值。

```jsx
import React from 'react';
import {Formik, Form, Field} from 'formik';

export const NestedExample = () => (
  <div>
    <h1>Social Profiles</h1>
    <Formik
      initialValues={{
        social: {
          facebook: '',
          twitter: ''
        }
      }}
      onSubmit={values => {
        // same shape as initial values
        console.log(values);
      }}
    >
      <Field name="social.facebook" />
      <Field name="social.twitter" />
      <button type="submit">Submit</button>
    </Formik>
  </div>
);
```

## 数组

Formik 还支持开箱即用的数组和对象数组。使用类似 lodash 的括号语法`name`字符串，您可以快速为列表中的项目构建字段。

```jsx
import React from 'react';
import {Formik, Form, Field} from 'formik';

export const BasicArrayExample = () => (
  <div>
    <h1>Friends</h1>
    <Formik
      initialValues={{
        friends: ['jared', 'ian']
      }}
      onSubmit={values => {
        // same shape as initial values
        console.log(values);
      }}
    >
      <Field name="friends[0]" />
      <Field name="friends[1]" />
      <button type="submit">Submit</button>
    </Formik>
  </div>
);
```

有关在列表中操作（添加/删除/ 更多）items 的更多信息，请参阅`<FieldArray>`组件的 API 参考部分。
