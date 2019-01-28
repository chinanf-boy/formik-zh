---
id: react-native
title: React Native
custom_edit_url: https://github.com/jaredpalmer/formik/edit/master/docs/guides/react-native.md
---

**Formik 与 React Native 和 React Native Web 100％兼容。**但是，由于 ReactDOM 和 React Native 处理表单和文本输入之间存在差异，因此需要注意一些差异。本节将向您介绍我们认为的最佳实践。

### 要旨

在进一步讨论之前，这里有一个关于如何使用 Formik 和 React Native 的超小 gist，它演示了关键的区别：

```jsx
// Formik x React Native example
import React from 'react';
import {Button, TextInput, View} from 'react-native';
import {Formik} from 'formik';

export const MyReactNativeForm = props => (
  <Formik initialValues={{email: ''}} onSubmit={values => console.log(values)}>
    {props => (
      <View>
        <TextInput
          onChangeText={props.handleChange('email')}
          onBlur={props.handleBlur('email')}
          value={props.values.email}
        />
        <Button onPress={props.handleSubmit} title="Submit" />
      </View>
    )}
  </Formik>
);
```

如您所见，使用 Formik 与 React DOM 和 React Native 之间的显着差异是：

1.  Formik 的`props.handleSubmit`被传递给了`<Button onPress={...} />`而不是 HTML`<form onSubmit={...} />`组件（因为 React Native 中没有`<form />`的元素）。
2.  `<TextInput />`使用 Formik 的`props.handleChange(fieldName)`和`handleBlur(fieldName)`，而不是直接将回调分配给 props，因为我们必须从 ReactNative 某个地方得到`fieldName`，我们无法像 web 一样自动获取它（使用输入名称属性）。你也可以使用`setFieldValue(fieldName, value)`和`setFieldTouched(fieldName, bool)`作为备选。
