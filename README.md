[![Commitizen friendly](https://img.shields.io/badge/commitizen-friendly-brightgreen.svg)](http://commitizen.github.io/cz-cli/)


> 1.初始化 npm

```
npm init
```
设置相关信息

> 2.初始化 git

```
git init
```
添加 .gitignore

> 3.通用配置

`.editorconfig`、`README.md`

> 4.规范commit

```
npm install -g commitizen
commitizen init cz-conventional-changelog --save-dev --save-exact
```
提交 commit `npx git-cz`

加下来 lint commit
```
npm install --save-dev @commitlint/{cli,config-conventional}
```

在根目录添加 文件 `.commitlintrc`. 其作用添加默认一些配置。[详情](https://github.com/conventional-changelog/commitlint/blob/master/@commitlint/config-conventional/index.js)

```json
{
  "extends": ["@commitlint/config-conventional"]
}

```

add husky
```
npm install --save-dev husky
```

在 `package.json` 的根目录添加 添加 `git hooks`

``` json
{
  "husky": {
    "hooks": {
      "commit-msg": "commitlint -E HUSKY_GIT_PARAMS"
    }  
  }}
```
