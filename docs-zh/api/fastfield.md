---
id: fastfield
title: <FastField />
custom_edit_url: https://github.com/jaredpalmer/formik/edit/master/docs/api/fastfield.md
---
## 在我们开始之前

`<FastField />`是为了表演*优化*. 然而，你真的不需要使用它，直到你这样做。只有在你熟悉反应的情况下才能继续[`shouldComponentUpdate()`](https://reactjs.org/docs/react-component.html#shouldcomponentupdate)作品。有人警告过你。\*\*

**不严重。在继续之前，请检查官方反应文件的以下部分**

-   [反应`shouldComponentUpdate()`参考文献](https://reactjs.org/docs/react-component.html#shouldcomponentupdate)
-   [`shouldComponentUpdate`在行动中](https://reactjs.org/docs/optimizing-performance.html#shouldcomponentupdate-in-action)

## 概述

`<FastField />`是的优化版本`<Field />`用于大型表单（约30多个字段）或字段具有非常昂贵的验证要求时。`<FastField />`具有与相同的API`<Field>`，但实现`shouldComponentUpdate()`在内部阻止所有其他重新呈现，除非直接更新`<FastField />`'s formik状态的相关部分/切片。

例如，`<FastField name="firstName" />`只有在以下情况下才会重新渲染：

-   改变到`values.firstName`，`errors.firstName`，`touched.firstName`或`isSubmitting`.这是通过比较来确定的。注意：支持点路径。
-   支柱被添加/移除到`<FastField name="firstName" />`
-   这个`name`道具更改

除上述情况外，`<FastField />`当Formik状态的其他部分更改时不会重新呈现。但是，所有由`<FastField />`将触发重新渲染到其他“香草色”`<Field />`组件。

## 何时使用`<FastField />`

**如果A`<Field />`“独立”于所有其他`<Field />`在您的表单中，然后您可以使用`<FastField />`**.

更具体地说，如果`<Field />`不更改行为或呈现基于更新到其他人的任何内容`<Field />`或`<FastField />`福米克状态的一部分，它不依赖于顶层的其他部分`<Formik />`状态（例如`isValidating`，请`submitCount`，然后您可以使用`<FastField />`作为替代品`<Field />`.

## 例子

```jsx
import React from 'react';
import { Formik, Field, FastField, Form } from 'formik';

const Basic = () => (
  <div>
    <h1>Sign Up</h1>
    <Formik
      initialValues={{
        firstName: '',
        lastName: '',
        email: '',
      }}
      validationSchema={Yup.object().shape({
        firstName: Yup.string().required(),
        middleInitial: Yup.string(),
        lastName: Yup.string().required(),
        email: Yup.string()
          .email()
          .required(),
      })}
      onSubmit={values => {
        setTimeout(() => {
          alert(JSON.stringify(values, null, 2));
        }, 500);
      }}
      render={formikProps => (
        <Form>
          {/** This <FastField> only updates for changes made to
           values.firstName, touched.firstName, errors.firstName */}
          <label htmlFor="firstName">First Name</label>
          <FastField name="firstName" placeholder="Weezy" />

          {/** Updates for all changes because it's from the
           top-level formikProps which get all updates */}
          {form.touched.firstName &&
            form.errors.firstName && <div>{form.errors.firstName}</div>}

          <label htmlFor="middleInitial">Middle Initial</label>
          <FastField
            name="middleInitial"
            placeholder="F"
            render={({ field, form }) => (
              <div>
                <input {...field} />
                {/**
                 * This updates normally because it's from the same slice of Formik state,
                 * i.e. path to the object matches the name of this <FastField />
                 */}
                {form.touched.middleInitial ? form.errors.middleInitial : null}

                {/** This won't ever update since it's coming from
                 from another <Field>/<FastField>'s (i.e. firstName's) slice   */}
                {form.touched.firstName && form.errors.firstName
                  ? form.errors.firstName
                  : null}

                {/* This doesn't update either */}
                {form.submitCount}

                {/* Imperative methods still work as expected */}
                <button
                  type="button"
                  onClick={form.setFieldValue('middleInitial', 'J')}
                >
                  J
                </button>
              </div>
            )}
          />

          {/** Updates for all changes to Formik state
           and all changes by all <Field>s and <FastField>s */}
          <label htmlFor="lastName">LastName</label>
          <Field
            name="lastName"
            placeholder="Baby"
            render={({ field, form }) => (
              <div>
                <input {...field} />
                {/** Works because this is inside
                 of a <Field/>, which gets all updates */}
                {form.touched.firstName && form.errors.firstName
                  ? form.errors.firstName
                  : null}
              </div>
            )}
          />

          {/** Updates for all changes to Formik state and
           all changes by all <Field>s and <FastField>s */}
          <label htmlFor="email">Email</label>
          <Field name="email" placeholder="jane@acme.com" type="email" />

          <button type="submit">Submit</button>
        </Form>
      )}
    />
  </div>
);
```
