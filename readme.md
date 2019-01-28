# jaredpalmer/formik [![explain]][source] [![translate-svg]][translate-list]

<!-- [![size-img]][size] -->

[explain]: http://llever.com/explain.svg
[source]: https://github.com/chinanf-boy/Source-Explain
[translate-svg]: http://llever.com/translate.svg
[translate-list]: https://github.com/chinanf-boy/chinese-translate-list
[size-img]: https://packagephobia.now.sh/badge?p=Name
[size]: https://packagephobia.now.sh/result?p=Name

「 构建 React 表单，没有眼泪 😭 」

[中文](./readme.md) | [english](https://github.com/jaredpalmer/formik)

---

## 校对 🀄️

<!-- doc-templite START generated -->
<!-- repo = 'jaredpalmer/formik' -->
<!-- commit = 'dc4bcf97e61b14fb4e90baff76cc209c13db19bc' -->
<!-- time = '2019-01-09' -->

| 翻译的原文 | 与日期        | 最新更新 | 更多                       |
| ---------- | ------------- | -------- | -------------------------- |
| [commit]   | ⏰ 2019-01-09 | ![last]  | [中文翻译][translate-list] |

[last]: https://img.shields.io/github/last-commit/jaredpalmer/formik.svg
[commit]: https://github.com/jaredpalmer/formik/tree/dc4bcf97e61b14fb4e90baff76cc209c13db19bc

<!-- doc-templite END generated -->

- [x] README.md
- [x] [概略](./docs-zh/overview.md)
- [ ] [资源](./docs-zh/resources.md)
- [ ] [教程](./docs-zh/tutorial.md)
- [ ] [指南：react-native](./docs-zh/guides/react-native.md)
- [ ] [指南：validation](./docs-zh/guides/validation.md)
- [ ] [指南：arrays](./docs-zh/guides/arrays.md)
- [ ] [指南：form-submission](./docs-zh/guides/form-submission.md)
- [ ] [指南：typescript](./docs-zh/guides/typescript.md)
- [ ] [API 参考: fieldarray](./docs-zh/api/fieldarray.md)
- [ ] [API 参考: errormessage](./docs-zh/api/errormessage.md)
- [ ] [API 参考: form](./docs-zh/api/form.md)
- [ ] [API 参考: withFormik](./docs-zh/api/withFormik.md)
- [ ] [API 参考: fastfield](./docs-zh/api/fastfield.md)
- [ ] [API 参考: connect](./docs-zh/api/connect.md)
- [ ] [API 参考: field](./docs-zh/api/field.md)
- [ ] [API 参考: formik](./docs-zh/api/formik.md)

### 贡献

欢迎 👏 勘误/校对/更新贡献 😊 [具体贡献请看](https://github.com/chinanf-boy/chinese-translate-list#贡献)

## 生活

[help me live , live need money 💰](https://github.com/chinanf-boy/live-need-money)

---

![](https://user-images.githubusercontent.com/4060187/27243721-3b5219d0-52b1-11e7-96f1-dae8391a3ef6.png)

[![CircleCI](https://circleci.com/gh/jaredpalmer/formik.svg?style=svg)](https://circleci.com/gh/jaredpalmer/formik)
[![Stable Release](https://img.shields.io/npm/v/formik.svg)](https://npm.im/formik)
[![Blazing Fast](https://badgen.now.sh/badge/speed/blazing%20%F0%9F%94%A5/green)](https://npm.im/formik)
[![gzip size](http://img.badgesize.io/https://unpkg.com/formik@latest/dist/formik.umd.production.js?compression=gzip)](https://unpkg.com/formik@latest/dist/formik.umd.production.js)
[![license](https://badgen.now.sh/badge/license/MIT)](./LICENSE)
[![Discord](https://img.shields.io/discord/102860784329052160.svg?style=flat-square)](https://discord.gg/cU6MCve)
[![Join the community on Spectrum](https://withspectrum.github.io/badge/badge.svg)](https://spectrum.chat/palmer)

让我们面对现实，[React](https://github.com/facebook/react)的(form)表单真的很冗长。更糟糕的是，大多数表单的帮助库都做了太多魔术(wayyyy)，并且往往会产生与之相关的显着性能成本。Formik 是一个小型库，可以帮助您解决 3 个最烦人的部分:

- 1. **获取表单状态的值和表单状态**
- 2. **验证和错误消息**
- 3. **处理表单提交**

通过将所有上述内容集中在一个地方，Formik 将保持井井有条 - 对您的表单进行测试，重构和明确主题变得轻而易举。

<div id="handleblur-e-any--void"></div>
<div id="handlechange-e-any--void"></div>

## 文件

- [入门](https://jaredpalmer.com/formik/docs/overview)
- [API 参考](https://jaredpalmer.com/formik/docs/api/formik)
- [文件 / 教程](https://jaredpalmer.com/formik/docs/resources)
- [获得帮助](https://jaredpalmer.com/formik/help)
- [Release 记](https://github.com/jaredpalmer/formik/releases)

## 浏览器内，游乐场

您可以在这些实时在线游乐场的 Web 浏览器中使用 Formik。

- CodeSandbox(ReactDOM)<https://codesandbox.io/s/zKrK5YLDZ>
- Expo Snack(React Native)<https://snack.expo.io/Bk9pPK87X>

## 例子

- [基础](https://codesandbox.io/s/zKrK5YLDZ)
- [同步 验证](https://codesandbox.io/s/q8yRqQMp)
- [构建 属于 你自个的原始输入](https://codesandbox.io/s/qJR4ykJk)
- 使用第三方输入:
  - [react-select-v1](https://codesandbox.io/s/jRzE53pqR)
  - [react-select-v2](https://codesandbox.io/s/73jj9zom96)
  - [Draft.js](https://codesandbox.io/s/QW1rqjBLl)
- [访问 React lifecycle 函数](https://codesandbox.io/s/pgD4DLypy)
- [React Native](https://snack.expo.io/@ferrannp/react-native-x-formik)
- [TypeScript](https://codesandbox.io/s/8y578o8152)

## 使用 Formik 的组织和项目

[使用 Formik 的组织与项目列表](https://github.com/jaredpalmer/formik/issues/87)

## 作者

- 贾里德帕尔默[@jaredpalmer](https://twitter.com/jaredpalmer)
- 伊恩怀特[@eonwhite](https://twitter.com/eonwhite)

## 贡献者

Formik 由\<3 感谢这些精彩的人([emoji key](https://github.com/kentcdodds/all-contributors#emoji-key)):

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->

<!-- prettier-ignore -->
| [<img src="https://avatars2.githubusercontent.com/u/4060187?v=4" width="100px;"/><br /><sub><b>Jared Palmer</b></sub>](http://jaredpalmer.com)<br />[💬](#question-jaredpalmer "Answering Questions") [💻](https://github.com/jaredpalmer/formik/commits?author=jaredpalmer "Code") [🎨](#design-jaredpalmer "Design") [📖](https://github.com/jaredpalmer/formik/commits?author=jaredpalmer "Documentation") [💡](#example-jaredpalmer "Examples") [🤔](#ideas-jaredpalmer "Ideas, Planning, & Feedback") [👀](#review-jaredpalmer "Reviewed Pull Requests") [⚠️](https://github.com/jaredpalmer/formik/commits?author=jaredpalmer "Tests") | [<img src="https://avatars0.githubusercontent.com/u/109324?v=4" width="100px;"/><br /><sub><b>Ian White</b></sub>](https://www.stardog.io)<br />[💬](#question-eonwhite "Answering Questions") [🐛](https://github.com/jaredpalmer/formik/issues?q=author%3Aeonwhite "Bug reports") [💻](https://github.com/jaredpalmer/formik/commits?author=eonwhite "Code") [📖](https://github.com/jaredpalmer/formik/commits?author=eonwhite "Documentation") [🤔](#ideas-eonwhite "Ideas, Planning, & Feedback") [👀](#review-eonwhite "Reviewed Pull Requests") | [<img src="https://avatars0.githubusercontent.com/u/829963?v=4" width="100px;"/><br /><sub><b>Andrej Badin</b></sub>](http://andrejbadin.com)<br />[💬](#question-Andreyco "Answering Questions") [🐛](https://github.com/jaredpalmer/formik/issues?q=author%3AAndreyco "Bug reports") [📖](https://github.com/jaredpalmer/formik/commits?author=Andreyco "Documentation") | [<img src="https://avatars2.githubusercontent.com/u/91115?v=4" width="100px;"/><br /><sub><b>Adam Howard</b></sub>](http://adz.co.de)<br />[💬](#question-skattyadz "Answering Questions") [🐛](https://github.com/jaredpalmer/formik/issues?q=author%3Askattyadz "Bug reports") [🤔](#ideas-skattyadz "Ideas, Planning, & Feedback") [👀](#review-skattyadz "Reviewed Pull Requests") | [<img src="https://avatars1.githubusercontent.com/u/6711845?v=4" width="100px;"/><br /><sub><b>Vlad Shcherbin</b></sub>](https://github.com/VladShcherbin)<br />[💬](#question-VladShcherbin "Answering Questions") [🐛](https://github.com/jaredpalmer/formik/issues?q=author%3AVladShcherbin "Bug reports") [🤔](#ideas-VladShcherbin "Ideas, Planning, & Feedback") | [<img src="https://avatars3.githubusercontent.com/u/383212?v=4" width="100px;"/><br /><sub><b>Brikou CARRE</b></sub>](https://github.com/brikou)<br />[🐛](https://github.com/jaredpalmer/formik/issues?q=author%3Abrikou "Bug reports") [📖](https://github.com/jaredpalmer/formik/commits?author=brikou "Documentation") | [<img src="https://avatars0.githubusercontent.com/u/5314713?v=4" width="100px;"/><br /><sub><b>Sam Kvale</b></sub>](http://skvale.github.io)<br />[🐛](https://github.com/jaredpalmer/formik/issues?q=author%3Askvale "Bug reports") [💻](https://github.com/jaredpalmer/formik/commits?author=skvale "Code") [⚠️](https://github.com/jaredpalmer/formik/commits?author=skvale "Tests") |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
| [<img src="https://avatars0.githubusercontent.com/u/13765558?v=4" width="100px;"/><br /><sub><b>Jon Tansey</b></sub>](http://jon.tansey.info)<br />[🐛](https://github.com/jaredpalmer/formik/issues?q=author%3Ajontansey "Bug reports") [💻](https://github.com/jaredpalmer/formik/commits?author=jontansey "Code") | [<img src="https://avatars0.githubusercontent.com/u/6819171?v=4" width="100px;"/><br /><sub><b>Tyler Martinez</b></sub>](http://slightlytyler.com)<br />[🐛](https://github.com/jaredpalmer/formik/issues?q=author%3Aslightlytyler "Bug reports") [📖](https://github.com/jaredpalmer/formik/commits?author=slightlytyler "Documentation") |  |  |  |  |  |

<!-- ALL-CONTRIBUTORS-LIST:END -->

该项目遵循[all-contributors](https://github.com/kentcdodds/all-contributors)规格.欢迎任何形式的贡献!

---

MIT 许可证.

---
