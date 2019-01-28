---
id: arrays
title: Arrays and Nested Objects
custom_edit_url: https://github.com/jaredpalmer/formik/edit/master/docs/guides/arrays.md
---
Formik支持嵌套对象和开箱即用的数组。这两个主题有些相关，因为它们都使用相同的语法。

## 嵌套对象

该`name`Formik中的道具可以使用类似lodash的点路径来引用嵌套的Formik值。这意味着您不再需要压缩表单的值。

```jsx
import React from 'react';
import { Formik, Form, Field } from 'formik';

export const NestedExample = () => (
  <div>
    <h1>Social Profiles</h1>
    <Formik
      initialValues={{
        social: {
          facebook: '',
          twitter: '',
        },
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

Formik还支持开箱即用的数组和对象数组。使用类似lodash的括号语法`name`字符串，您可以快速为列表中的项目构建字段。

```jsx
import React from 'react';
import { Formik, Form, Field } from 'formik';

export const BasicArrayExample = () => (
  <div>
    <h1>Friends</h1>
    <Formik
      initialValues={{
        friends: ['jared', 'ian'],
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

有关在列表中操作（添加/删除/ etc）项的更多信息，请参阅上面的API参考部分`<FieldArray>`零件。
