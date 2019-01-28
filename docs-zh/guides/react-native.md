---
id: react-native
title: React Native
custom_edit_url: https://github.com/jaredpalmer/formik/edit/master/docs/guides/react-native.md
---
**Formik与React Native和React Native Web 100％兼容。**但是，由于ReactDOM和React Native处理表单和文本输入之间存在差异，因此需要注意一些差异。本节将向您介绍它们以及我们认为最佳实践。

### 要旨

在进一步讨论之前，这里有一个关于如何使用Formik和React Native的超级最小要点，它演示了关键的区别：

```jsx
// Formik x React Native example
import React from 'react';
import { Button, TextInput, View } from 'react-native';
import { Formik } from 'formik';

export const MyReactNativeForm = props => (
  <Formik
    initialValues={{ email: '' }}
    onSubmit={values => console.log(values)}
  >
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

如您所见，使用Formik与React DOM和React Native之间的显着差异是：

1.  Formik的`props.handleSubmit`被传递给了`<Button onPress={...} />`而不是HTML`<form onSubmit={...} />`组件（因为没有`<form />`React Native中的元素）。
2.  `<TextInput />`使用Formik的`props.handleChange(fieldName)`和`handleBlur(fieldName)`而不是直接将回调分配给道具，因为我们必须得到`fieldName`从某个地方和ReactNative，我们无法像web一样自动获取它（使用输入名称属性）。你也可以使用`setFieldValue(fieldName, value)`和`setFieldTouched(fieldName, bool)`作为备选。
