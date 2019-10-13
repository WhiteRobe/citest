# 基于Github的自动持续集成测试和报告分析

> 持续集成：简单来说，就是能够多次将代码快速部署到主要版本，用于敏捷开发和保证主干颗粒度不至于过大。
 
利用Github和**Travis-CI**+**Coveralls**，我们每次向Github提交的代码都将会被**Travis CI**自动测试，而测试结果可以通过模块发送给**Coveralls**，由其收集这些测试信息、记录历史并发布数据分析。


## 自动持续集成测试-Travis CI
[Travis-CI](https://www.travis-ci.org/) 是一款可以进行在线自动测试的应用。通过授权其你的Github指定仓库，你每次推送新的代码，它都会根据配置文件自动拉取、构建以来、按照测试模块进行自动测试。

- 需要注意的是，Travis CI只是一个自动测试应用而不是测试工具，你仍需要使用[jest](https://jestjs.io/zh-Hans/)等工具编写、运行测试代码。

其依赖于【.travis.yml】进行配置，参考教程：[Travis CI Tutorial](https://docs.travis-ci.com/user/tutorial/)，或者可以简单参考本仓库的该文件进行配置，以快速上手整个流程。

## 测试报告-Coveralls

[Coveralls](https://coveralls.io/) 是自动测试报告的历史追踪和分析应用。

其依赖于【.coveralls.yml】文件进行配置，对于公有仓库，你并不太需要关心此事。

- 需要注意的是，`repo_token`是私密的，所以你应当把`.coveralls.yml`文件列入`.gitignore`，或是通过加密工具将其[加密](https://docs.coveralls.io/api-introduction)。

## 测试实例

本仓库为:【node.js + jest + travis-ci + coveralls】的实例，其中，测试代码放置于`/test`中。

从零构建一个像这样基于Node.js的实验样例，你需要安装`jest`：

```
npm init
npm install jest
```

由于用到了coveralls，所以需要安装一个额外的[报告发送模块](https://github.com/nickmerwin/node-coveralls)：`npm install coveralls --save-dev`。


### 搭建自动测试的流程


1. 首先，在[Travis-CI](https://www.travis-ci.org/)和[Coveralls](https://coveralls.io/)勾选本仓库。

2. 然后，修改【package.json】中的

```
"scripts": {
    "test": "......."
  },
```

为：

```
"scripts": {
    "test": "jest --coverage --coverageReporters=text-lcov | coveralls"
  },
```

3. 接着，在仓库目录下新建一个【.travis.yml】文件，键入以下内容：
```
language: node_js
node_js:
  - "10"
```

再构建一个【.coveralls.yml】文件，填入内容：（注意将该文件列入`.gitignore`）

```
service_name: travis-pro
repo_token: $YourToken
```

> 由于`repo_token`是私密的，所以你应当把`.coveralls.yml`文件列入`.gitignore`，或是通过加密工具将其[加密](https://docs.coveralls.io/api-introduction)，当然你也可以直接省略。

4. 编写你的测试单元，如`/test`中所示。

5. 最后，向Github提交代码，你会在[Travis-CI](https://www.travis-ci.org/)上看到构建结果，结果会被发送到[Coveralls](https://coveralls.io/)生成历史和报告。


### 持续集成测试结果

Travis-CI 测试结果：![](https://api.travis-ci.org/WhiteRobe/citest.svg?branch=master)

Coveralls 报告结果：[![Coverage Status](https://coveralls.io/repos/github/WhiteRobe/citest/badge.svg?branch=master)](https://coveralls.io/github/WhiteRobe/citest?branch=master)

### BetterCodeHub

BetterCodeHub 测试结果: <img src='https://bettercodehub.com/edge/badge/WhiteRobe/citest?branch=master'>
