---
id: fieldarray
title: <FieldArray />
custom_edit_url: https://github.com/jaredpalmer/formik/edit/master/docs/api/fieldarray.md
---

`<FieldArray />`是一个有助于常见数组/列表操作的组件。你传了一个`name`具有密钥路径的属性`values`持有相关数组。`<FieldArray />`然后，您将通过渲染道具访问数组助手方法。为方便起见，调用这些方法将触发验证并进行管理`touched`为了你。

```jsx
import React from 'react';
import {Formik, Form, Field, FieldArray} from 'formik';

// Here is an example of a form with an editable list.
// Next to each input are buttons for insert and remove.
// If the list is empty, there is a button to add an item.
export const FriendList = () => (
  <div>
    <h1>Friend List</h1>
    <Formik
      initialValues={{friends: ['jared', 'ian', 'brent']}}
      onSubmit={values =>
        setTimeout(() => {
          alert(JSON.stringify(values, null, 2));
        }, 500)
      }
      render={({values}) => (
        <Form>
          <FieldArray
            name="friends"
            render={arrayHelpers => (
              <div>
                {values.friends && values.friends.length > 0 ? (
                  values.friends.map((friend, index) => (
                    <div key={index}>
                      <Field name={`friends.${index}`} />
                      <button
                        type="button"
                        onClick={() => arrayHelpers.remove(index)} // remove a friend from the list
                      >
                        -
                      </button>
                      <button
                        type="button"
                        onClick={() => arrayHelpers.insert(index, '')} // insert an empty string at a position
                      >
                        +
                      </button>
                    </div>
                  ))
                ) : (
                  <button type="button" onClick={() => arrayHelpers.push('')}>
                    {/* show this when user has removed all friends from the list */}
                    Add a friend
                  </button>
                )}
                <div>
                  <button type="submit">Submit</button>
                </div>
              </div>
            )}
          />
        </Form>
      )}
    />
  </div>
);
```

### `name: string`

相关密钥的名称或路径[`values`](api/formik.md#values-field-string-any)。

### `validateOnChange?: boolean`

默认是`true`。确定是否应该运行表单验证*后*任何数组操作。

## FieldArray 对象数组

您还可以按照约定迭代对象数组`object[index]property`要么`object.index.property`对于名称属性`<Field />`要么`<input />`中的元素`<FieldArray />`。

```jsx
<Form>
  <FieldArray
    name="friends"
    render={arrayHelpers => (
      <div>
        {values.friends.map((friend, index) => (
          <div key={index}>
            <Field name={`friends[${index}].name`} />
            <Field name={`friends.${index}.age`} /> // both these conventions do
            the same
            <button type="button" onClick={() => arrayHelpers.remove(index)}>
              -
            </button>
          </div>
        ))}
        <button
          type="button"
          onClick={() => arrayHelpers.push({name: '', age: ''})}
        >
          +
        </button>
      </div>
    )}
  />
</Form>
```

## FieldArray 验证问题

验证可能很棘手`<FieldArray>`。

如果你使用[`validationSchema`](api/formik.md#validationschema-schema-schema)并且您的表单具有数组验证要求（如最小长度）以及嵌套数组字段要求，显示错误可能会非常棘手。Formik / Yup 将在内部显示验证错误。例如，

```js
const schema = Yup.object().shape({
  friends: Yup.array()
    .of(
      Yup.object().shape({
        name: Yup.string()
          .min(4, 'too short')
          .required('Required'), // these constraints take precedence
        salary: Yup.string()
          .min(3, 'cmon')
          .required('Required') // these constraints take precedence
      })
    )
    .required('Must have friends') // these constraints are shown if and only if inner constraints are satisfied
    .min(3, 'Minimum of 3 friends')
});
```

由于 Yup 和您的自定义验证函数应始终将错误消息作为字符串输出，因此您需要在显示它时嗅探嵌套错误是数组还是字符串。

所以...要显示`'Must have friends'`和`'Minimum of 3 friends'`（我们的示例的数组验证约束）...

**_坏_**

```jsx
// within a `FieldArray`'s render
const FriendArrayErrors = errors =>
  errors.friends ? <div>{errors.friends}</div> : null; // app will crash
```

**_好_**

```jsx
// within a `FieldArray`'s render
const FriendArrayErrors = errors =>
  typeof errors.friends === 'string' ? <div>{errors.friends}</div> : null;
```

对于嵌套字段错误，您应该假定除非您已检查过，否则不会定义对象的任何部分。因此，你可能想帮自己一个忙，做一个习惯`<ErrorMessage />`看起来像这样的组件：

```jsx
import {Field, getIn} from 'formik';

const ErrorMessage = ({name}) => (
  <Field
    name={name}
    render={({form}) => {
      const error = getIn(form.errors, name);
      const touch = getIn(form.touched, name);
      return touch && error ? error : null;
    }}
  />
);

// Usage
<ErrorMessage name="friends[0].name" />; // => null, 'too short', or 'required'
```

_注意_：在 Formik v0.12 / 1.0 中，一个新的`meta`可以添加道具`Field`和`FieldArray`这将为您提供相关的元数据，如`error`＆`touch`，这将使您免于必须使用 Formik 或 lodash 的 getIn 或检查路径是否由您自己定义。

## FieldArray 助手

通过渲染道具可以使用以下方法。

- `push: (obj: any) => void`：将值添加到数组的末尾
- `swap: (indexA: number, indexB: number) => void`：在数组中交换两个值
- `move: (from: number, to: number) => void`：将数组中的元素移动到另一个索引
- `insert: (index: number, value: any) => void`：将给定索引处的元素插入到数组中
- `unshift: (value: any) => number`：将一个元素添加到数组的开头并返回其长度
- `remove<T>(index: number): T | undefined`：删除数组索引处的元素并将其返回
- `pop<T>(): T | undefined`：从数组末尾删除并返回值
- `replace: (index: number, value: any) => void`：将给定索引处的值替换为数组

## FieldArray 渲染方法

有三种方法可以渲染`<FieldArray />`

- `<FieldArray name="..." component>`
- `<FieldArray name="..." render>`
- `<FieldArray name="..." children>`

### `render: (arrayHelpers: ArrayHelpers) => React.ReactNode`

```jsx
import React from 'react';
import { Formik, Form, Field, FieldArray } from 'formik'

export const FriendList = () => (
  <div>
    <h1>Friend List</h1>
    <Formik
      initialValues={{ friends: ['jared', 'ian', 'brent'] }}
      onSubmit={...}
      render={formikProps => (
        <FieldArray
          name="friends"
          render={({ move, swap, push, insert, unshift, pop }) => (
            <Form>
              {/*... use these however you want */}
            </Form>
          )}
        />
    />
  </div>
);
```

### `component: React.ReactNode`

```jsx
import React from 'react';
import { Formik, Form, Field, FieldArray } from 'formik'


export const FriendList = () => (
  <div>
    <h1>Friend List</h1>
    <Formik
      initialValues={{ friends: ['jared', 'ian', 'brent'] }}
      onSubmit={...}
      render={formikProps => (
        <FieldArray
          name="friends"
          component={MyDynamicForm}
        />
      )}
    />
  </div>
);


// In addition to the array helpers, Formik state and helpers
// (values, touched, setXXX, etc) are provided through a `form`
// prop
export const MyDynamicForm = ({
  move, swap, push, insert, unshift, pop, form
}) => (
 <Form>
  {/**  whatever you need to do */}
 </Form>
);
```

### `children: func`

```jsx
import React from 'react';
import { Formik, Form, Field, FieldArray } from 'formik'


export const FriendList = () => (
  <div>
    <h1>Friend List</h1>
    <Formik
      initialValues={{ friends: ['jared', 'ian', 'brent'] }}
      onSubmit={...}
      render={formikProps => (
        <FieldArray name="friends">
          {({ move, swap, push, insert, unshift, pop, form }) => {
            return (
              <Form>
                {/*... use these however you want */}
              </Form>
            );
          }}
        <FieldArray/>
      )}
    />
  </div>
);
```
