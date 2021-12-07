[TOC]

# Formik

## Fomik 简介

### Formik 介绍

增强表单处理能⼒. 简化表单处理流程. 

[官⽹](https://jaredpalmer.com/formik/)



### Formik 下载

```bash
npm install formik
```





## Formik 增强表单

### Formik 基本使⽤

使⽤ formik 进⾏表单数据绑定以及表单提交处理

```jsx
import { useFormik } from "formik";

function App() {
  const formik = useFormik({
    initialValues: { username: "张三" },
    onSubmit: (values) => {},
  });

  return (
    <form onSubmit={formik.handleSubmit}>
      <input
        type="text"
        name="username"
        value={formik.values.username}
        onChange={formik.handleChange}
      />
      <input type="submit" />
    </form>
  );
}
```



### 表单验证

#### 初始验证⽅式

```jsx
  const formik = useFormik({
    validate: (values) => {
      const errors = {};
      if (!values.username) errors.username = "请输入用户名";
      return errors;
    },
  });

  return (
    <form>
      {formik.errors.username ? <div>{formik.errors.username}</div> : null}
    </form>
  );
```



#### 完善错误信息提示时的⽤户体验

开启离开焦点时触发验证

```jsx
onBlur={formik.handleBlur}
```



提示信息时检查表单元素的值是否被改动过

```jsx
{formik.touched.username && formik.errors.username ? <div>{formik.errors.username}</div> : null}
```



#### 使⽤ yup 验证

下载

```bash
npm install yup
```



引⼊ yup

```bash
import * as Yup from 'yup';
```



定义验证规则

```jsx
    validationSchema: Yup.object({
      username: Yup.string()
        .max(15, "用户名的长度不能大于15")
        .required("请输入用户名"),
      password: Yup.string()
        .max(20, "密码的长度不能大于20")
        .required("请输入密码"),
    }),
```



### 减少样板代码

```jsx
{...formik.getFieldProps('username')}
```



### 使⽤组件的⽅式构建表单

```jsx
import { Formik, Form, Field, ErrorMessage } from "formik";

function App() {
  return (
    <Formik
      initialValues={initialValues}
      onSubmit={handleSubmit}
      validationSchema={schema}
    >
      <Form>
        <Field name="username">
          <ErrorMessage name="username"></ErrorMessage>
          <button type="submit">提交</button>
        </Field>
      </Form>
    </Formik>
  );
}

```



### 构建其他表单项

默认情况下, Field组件渲染的是⽂本框. 如要⽣成其他表单元素可以使⽤以下语法

```jsx
<Field name="content" as="textarea"/>
<Field name="subject" as="select">
  <option value="前端">前端</option>
  <option value="Java">Java</option>
  <option value="python">python</option>
</Field>
```



### 使⽤ useField 构建 ⾃定义表单控件

```jsx
import { useField } from "formik";

function MyInputField({ label, ...props }) {
  const [field, meta] = useField(props);
  return (
    <div>
      <label htmlFor={props.id}>{label}</label>
      <input {...field} {...props} />
      {meta.touched && meta.error ? <div>meta.error</div> : null}
    </div>
  );
}

```



### ⾃定义表单控件 Checkbox

```jsx
import React from "react";
import { Formik, Form, Field, ErrorMessage, useField } from "formik";
import * as Yup from "yup";

function Checkbox ({label, ...props}) {
  const [field, meta, helper] = useField(props);
  const { value } = meta;
  const { setValue } = helper;
  const handleChange = () => {
    const set = new Set(value);
    if (set.has(props.value)) {
      set.delete(props.value);
    }else {
      set.add(props.value);
    }
    setValue([...set])
  }
  return <div>
    <label htmlFor="">
      <input checked={value.includes(props.value)} type="checkbox" {...props} onChange={handleChange}/> {label}
    </label>
  </div>
}

function App() {
  const initialValues = {username: '', hobbies: ['足球', '篮球']};
  const handleSubmit = (values) => {
    console.log(values);
  };
  const schema = Yup.object({
    username: Yup.string()
      .max(15, "用户名的长度不能大于15")
      .required("请输入用户名"),
  });
  return (
    <Formik
      initialValues={initialValues}
      onSubmit={handleSubmit}
      validationSchema={schema}
    >
      <Form>
        <Field name="username" />
        <ErrorMessage name="username" />
        <Checkbox value="足球" label="足球" name="hobbies"/>
        <Checkbox value="篮球" label="篮球" name="hobbies"/>
        <Checkbox value="橄榄球" label="橄榄球" name="hobbies"/>
        <input type="submit"/>
      </Form>
    </Formik>
  );
}

export default App;
```

