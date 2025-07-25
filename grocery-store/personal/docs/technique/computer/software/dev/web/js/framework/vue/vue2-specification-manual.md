# Vue.js 2 规范手册 / Vue.js 2 Specification Manual

Minimal requires: node@'^16.13.0 || ^18.12.0 || ^20.9.0 || >=22.0.0', npm@>=8, pnpm@>=7, yarn@>=1.

Recommend requires: node@'^18.20.0 || ^20.10.0 || >=22.0.0', npm@>=9, pnpm@>=7, yarn@>=1.

This article is based on node@18.20.8, npm@10.9.2, corepack@0.32.0, pnpm@10.12.3.

Main dependencies:

- vue@^2.7.16, vue-cli@^4 (webpack@^4, babel@^7, core-js@^3)
- eslint@latest, stylelint@latest
- simple-git-hooks@latest, lint-staged@latest, commitlint@latest

## 🔧 更新 vscode 配置和 git 配置

### 快速配置

shell（For command `gsp`, please see [README.md#script_setup](/README.md#script_setup)）

```shell
# vscode 配置
# -- 推荐扩展
gsp vue/extensions.json .vscode/ -f
# -- 工作区设置
gsp vue/settings.json .vscode/ -f
# -- js 编译器设置
gsp vue/jsconfig.json -f
# -- 通用代码格式设置
gsp .editorconfig -f

# git 配置
# -- 文件属性
gsp .gitattributes -f
# -- 忽略文件
gsp nodejs.gitignore .gitignore -f
```

### 手动配置

.vscode/extensions.json

See [here](/grocery-store/personal/preferences/editor/vscode-workspace/vue/extensions.json).

.vscode/settings.json

See [here](/grocery-store/personal/preferences/editor/vscode-workspace/vue/settings.json).

jsconfig.json

See [here](/grocery-store/personal/preferences/editor/vscode-workspace/vue/jsconfig.json).

.editorconfig

See [here](/grocery-store/personal/preferences/editor/.editorconfig).

.gitattributes

See [here](/grocery-store/personal/preferences/vcs/git/.gitattributes).

.gitignore

See [here](/grocery-store/personal/preferences/vcs/git/nodejs.gitignore).

## 📦 配置包管理器和 .npmrc

### 前置任务

shell

```shell
npm i corepack@latest -g
npm i @antfu/ni@latest -g
```

### 快速配置

shell（This syntax of command `npm pkg set` requires npm@>=10.9.2）

```shell
corepack use pnpm@latest-10

# 运行本项目的“最小”要求:
# node 版本符合 ^16.13.0 || ^18.12.0 || ^20.9.0 || >=22.0.0，
# npm 版本符合 >=8
# pnpm 版本符合 >=7
# yarn 版本符合 >=1
npm pkg set 'engines.node=^16.13.0 || ^18.12.0 || ^20.9.0 || >=22.0.0' 'engines.npm=>=9' 'engines.pnpm=>=7' 'engines.yarn=>=1'

gsp npm/.npmrc -f
```

### 手动配置

package.json

```json
{
  // ...

  // Used by corepack
  "packageManager": "pnpm@10.12.3+sha512.467df2c586056165580ad6dfb54ceaad94c5a30f80893ebdec5a44c5aa73c205ae4a5bb9d5ed6bb84ea7c249ece786642bbb49d06a307df218d03da41c317417",
  "engines": {
    "node": "^16.13.0 || ^18.12.0 || ^20.9.0 || >=22.0.0",
    "npm": ">=8",
    "pnpm": ">=7",
    "yarn": ">=1"
  }

  // ...
}
```

.npmrc

See [here](/grocery-store/personal/preferences/package-manager/npm/.npmrc).

## 🥡 基础依赖升级

shell

```shell
# vue, vue-router, vuex, core-js
ni vue@^2.7.16 vue-router@legacy vuex@^3.6.2 core-js@latest
# vue-cli, vue-template-compiler
ni @vue/cli-service@^4.5.19 @vue/cli-plugin-babel@latest vue-template-compiler@latest -D
```

## 🌟 设置代码检查与格式化

> 真心期待前端有一个大统一的、完整的生态工具链！！！

### 前置任务

shell

```shell
# eslint
ni eslint@latest -D
# eslint config
# Since the version of 4.15.0, `@antfu/eslint-config` requires node@>=20
ni @antfu/eslint-config@~4.14.1 -D
# eslint & prettier plugin
ni eslint-plugin-format@latest @prettier/plugin-xml@latest -D
```

### 快速配置

shell

```shell
gsp vue2/eslint.config.mjs -f
```

### 手动配置

eslint.config.mjs

See [here](/grocery-store/personal/preferences/linter/eslint/vue2/eslint.config.mjs).

## ✨ 设置样式检查与格式化

> 真心期待前端有一个大统一的、完整的生态工具链！！！

### 前置任务

shell

```shell
# stylelint
ni stylelint@latest -D
# stylelint config for html
ni stylelint-config-html@latest -D
# stylelint config for scss
# Since the version of 15.0.0, `stylelint-config-standard-scss` requires node@>=20
ni stylelint-config-standard-scss@^14.0.0 -D
# stylelint config for vue
ni stylelint-config-standard-vue@latest -D
# stylelint config for stylistic
# Since the version of 7.0.0, `stylelint-config-recess-order` requires node@>=20
ni @stylistic/stylelint-config@latest stylelint-config-recess-order@^6.1.0 -D
```

### 快速配置

shell

```shell
gsp vue/stylelint.config.mjs -f
```

### 手动配置

stylelint.config.mjs

See [here](/grocery-store/personal/preferences/linter/stylelint/vue/stylelint.config.mjs).

## 📜 配置 npm 快速检查与格式化脚本

### 前置任务

shell

```shell
# Since the version of 8.0.0, `npm-run-all2` requires node@>=20
ni npm-run-all2@^7.0.2 -D
```

### 快速配置

shell

```shell
npm pkg set 'scripts.lint=run-s lint:*'
npm pkg set 'scripts.lint:js=eslint --cache .'
npm pkg set 'scripts.lint:style=stylelint --cache **/*.{css,postcss,scss,html,vue}'
npm pkg set 'scripts.fix=run-s fix:*'
npm pkg set 'scripts.fix:js=eslint --cache --fix .'
npm pkg set 'scripts.fix:style=stylelint --cache --fix **/*.{css,postcss,scss,html,vue}'
```

### 手动配置

package.json

```json
{
  // ...
  "scripts": {
    // ...

    "lint": "run-s lint:*",
    "lint:js": "eslint --cache .",
    "lint:style": "stylelint --cache **/*.{css,postcss,scss,html,vue}",
    "fix": "run-s fix:*",
    "fix:js": "eslint --cache --fix .",
    "fix:style": "stylelint --cache --fix **/*.{css,postcss,scss,html,vue}"

    // ...
  }
}
```

## 🤖 配置提交检查与格式化

### 前置任务

shell

```shell
# The performance of `simple-git-hooks` is much better than `husky`
ni simple-git-hooks@latest -D
# lint-staged & commitlint
ni lint-staged@latest @commitlint/cli@latest @commitlint/config-conventional@latest -D
```

### 快速配置

shell

```shell
npm pkg set 'scripts.prepare=simple-git-hooks'
npm pkg set 'simple-git-hooks.pre-commit=npx lint-staged'
npm pkg set 'simple-git-hooks.commit-msg=npx commitlint --edit $1'
npm pkg set 'lint-staged.*=eslint --fix'
npm pkg set 'lint-staged[*.{css,postcss,scss,html,vue}]=stylelint --cache --fix'

gsp commitlint/commitlint.config.mjs -f
```

### 手动配置

package.json（配置 simple-git-hooks）

```json
{
  // ...

  "scripts": {
    // ...
    "prepare": "simple-git-hooks"
  },

  // ...

  "simple-git-hooks": {
    "pre-commit": "npx lint-staged",
    "commit-msg": "npx commitlint --edit $1"
  },
  "lint-staged": {
    "*": "eslint --cache --fix",
    "*.{css,postcss,scss,html,vue}": "stylelint --cache --fix"
  }

  // ...
}
```

commitlint.config.mjs

See [here](/grocery-store/personal/preferences/linter/commitlint/commitlint.config.mjs).

## 💪🏼 使用 Dart Sass 提供 Sass 支持，移除 Node Sass

### 前置任务

shell

```shell
# 限制 node 版本的罪魁祸首！
nun node-sass

# sass 和 sass-loader
ni sass@latest sass-loader@version-10 -D
```

### 手动配置

vue.config.js

```js
module.exports = {
  // ...

  css: {
    loaderOptions: {
      scss: {
        sassOptions: {
          // scss 支持本身不需要任何配置
          // 只有代码中使用到大量的弃用 API 时，才需要禁用警告（因为警告输出实在是太多咧）
          silenceDeprecations: [
            'legacy-js-api',
            'mixed-decls',
            'import',
            'slash-div',
            'global-builtin',
            'function-units',
          ],
        },
      },
    },
  },

  // ...
}
```

## 🧹 项目兼容性 & 可维护性

### [taze](https://www.npmjs.com/package/taze)

#### 使用

```shell
# taze：帮你轻松完成依赖升级
npx taze minor -Iw
```
