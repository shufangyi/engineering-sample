[![Commitizen friendly](https://img.shields.io/badge/commitizen-friendly-brightgreen.svg)](http://commitizen.github.io/cz-cli/)
[![semantic-release](https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg)](https://github.com/semantic-release/semantic-release)

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
  }
}
```

> 5.自动发布和CI

```shell
npm install -D semantic-release-cli
npx semantic-release-cli setup

```
根目录创建文件 `.travis.yml` [参考](https://github.com/semantic-release/semantic-release/blob/master/docs/recipes/travis.md)

> 6.自动生成changelog
1.
```shell
npm install -D @jedmao/semantic-release-npm-github-config
```
和semantic-release 配套的工具
根目录创建文件 `.releaserc.json` [参考](https://github.com/semantic-release/apm-config)

2.
```shell
 npm install -D conventional-changelog-cli
```

```shell
conventional-changelog -p angular -i CHANGELOG.md -s
```
以上命令中参数-p angular用来指定使用的 commit message 标准

参数-i CHANGELOG.md表示从 CHANGELOG.md 读取 changelog, -s 表示读写 changelog 为同一文件。需要注意的是，上面这条命令产生的 changelog 是基于上次 tag 版本之后的变更（Feature、Fix、Breaking Changes等等）所产生的，所以如果你想生成之前所有 commit 信息产生的 changelog 则需要使用这条命令：

```shell
conventional-changelog -p angular -i CHANGELOG.md -s -r 0
```
其中 -r 表示生成 changelog 所需要使用的 release 版本数量，默认为1，全部则是0。

> 7.自动打版

```shell
npm install -D standard-version
```
```
1. git pull origin master
2. 根据 pacakage.json 中的 version 更新版本号，更新 changelog
3. git add -A, 然后 git commit
4. git tag 打版本操作
5. push 版本 tag 和 master 分支到仓库
```
其中2，3，4则是 standard-version 工具会自动完成的工作，配合本地的 shell 脚本，则可以自动完成一系列版本发布的工作了。

* --release-as, -r 指定版本号

默认情况下，工具会自动根据 主版本（major）,次版本（ minor） or 修订版（patch） 规则生成版本号，例如如果你package.json 中的version 为 1.0.0, 那么执行后版本号则是：1.0.1。自定义可以通过：

```shell
$ standard-version -r minor
# output 1.1.0
$ standard-version -r 2.0.0
# output 2.0.0
$ standard-version -r 2.0.0-test
# output 2.0.0-test
```

* --prerelease, -p 预发版本命名
用来生成预发版本, 如果当期的版本号是 2.0.0，例如

```bash
$ standard-version --prerelease alpha
# output 2.0.0-alpha.0
```

* --tag-prefix, -t 版本 tag 前缀
用来给生成 tag 标签添加前缀，例如如果前版本号为 2.0.0，则：

```bash
$ standard-version --tag-prefix "stable-"
# output tag: stable-v2.0.0
```