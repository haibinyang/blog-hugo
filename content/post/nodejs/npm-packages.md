---
title: "NPM Packages"
description: 
date: 2024-01-10T13:24:47+08:00
image: 
math: 
license: 
hidden: false
comments: true
draft: false
---



# npm基础知识

配置文件路径

```
~/.npmrc
```



配置镜像（不建议）

```
registry=https://registry.npmjs.org/
disturl=https://npm.taobao.org/dist
```

> `disturl` 项被设置为了淘宝的 Node.js 镜像（`https://npm.taobao.org/dist`），这是 Node.js 二进制分发的镜像地址，用于加速 Node.js 版本的下载。



# 最受欢迎的NPM包

| 名称                                                         | 创建时间 | 最近的版本 | 最近发布时间 | 受欢迎程度 |
| ------------------------------------------------------------ | -------- | ---------- | ------------ | ---------- |
| [chalk](https://npmjs.org/package/chalk)                     | 2013-08  | 5.3.0      | 2023-06      | 0.99       |
| [commander](https://npmjs.org/package/commander)             | 2011-08  | 11.0.0     | 2023-06      | 0.98       |
| [debug](https://npmjs.org/package/debug)                     | 2011-11  | 4.3.4      | 2022-03      | 0.97       |
| [lodash](https://npmjs.org/package/lodash)                   | 2012-04  | 4.17.21    | 2021-02      | 0.95       |
| [fs-extra](https://npmjs.org/package/fs-extra)               | 2011-11  | 11.1.1     | 2023-03      | 0.95       |
| [axios](https://npmjs.org/package/axios)                     | 2014-08  | 1.4.0      | 2023-04      | 0.94       |
| [glob](https://npmjs.org/package/glob)                       | 2011-01  | 10.3.3     | 2023-07      | 0.94       |
| [uuid](https://npmjs.org/package/uuid)                       | 2011-03  | 9.0.0      | 2022-09      | 0.94       |
| [yargs](https://npmjs.org/package/yargs)                     | 2013-11  | 17.7.2     | 2023-04      | 0.93       |
| [semver](https://npmjs.org/package/semver)                   | 2011-02  | 7.5.4      | 2023-07      | 0.92       |
| [rimraf](https://npmjs.org/package/rimraf)                   | 2011-02  | 5.0.1      | 2023-05      | 0.91       |
| [express](https://npmjs.org/package/express)                 | 2010-12  | 4.18.2     | 2022-10      | 0.91       |
| [mkdirp](https://npmjs.org/package/mkdirp)                   | 2011-01  | 3.0.1      | 2023-04      | 0.9        |
| [minimist](https://npmjs.org/package/minimist)               | 2013-06  | 1.2.8      | 2023-02      | 0.9        |
| [react](https://npmjs.org/package/react)                     | 2011-10  | 18.2.0     | 2022-06      | 0.9        |
| [typescript](https://npmjs.org/package/typescript)           | 2012-10  | 5.1.6      | 2023-06      | 0.89       |
| [async](https://npmjs.org/package/async)                     | 2010-12  | 3.2.4      | 2022-06      | 0.89       |
| [body-parser](https://npmjs.org/package/body-parser)         | 2014-01  | 1.20.2     | 2023-02      | 0.89       |
| [dotenv](https://npmjs.org/package/dotenv)                   | 2013-07  | 16.3.1     | 2023-06      | 0.89       |
| [ws](https://npmjs.org/package/ws)                           | 2011-12  | 8.13.0     | 2023-03      | 0.89       |
| [moment](https://npmjs.org/package/moment)                   | 2011-10  | 2.29.4     | 2022-07      | 0.88       |
| [inquirer](https://npmjs.org/package/inquirer)               | 2013-05  | 9.2.7      | 2023-06      | 0.88       |
| [node-fetch](https://npmjs.org/package/node-fetch)           | 2015-01  | 3.3.1      | 2023-03      | 0.88       |
| [webpack](https://npmjs.org/package/webpack)                 | 2012-03  | 5.88.1     | 2023-06      | 0.87       |
| [eslint](https://npmjs.org/package/eslint)                   | 2013-07  | 8.44.0     | 2023-06      | 0.87       |
| [bluebird](https://npmjs.org/package/bluebird)               | 2013-09  | 3.7.2      | 2019-11      | 0.87       |
| [js-yaml](https://npmjs.org/package/js-yaml)                 | 2011-11  | 4.1.0      | 2021-04      | 0.86       |
| [react-dom](https://npmjs.org/package/react-dom)             | 2014-05  | 18.2.0     | 2022-06      | 0.85       |
| [minimatch](https://npmjs.org/package/minimatch)             | 2011-07  | 9.0.3      | 2023-07      | 0.84       |
| [ajv](https://npmjs.org/package/ajv)                         | 2015-05  | 8.12.0     | 2023-01      | 0.84       |
| [rxjs](https://npmjs.org/package/rxjs)                       | 2012-03  | 7.8.1      | 2023-04      | 0.84       |
| [prop-types](https://npmjs.org/package/prop-types)           | 2015-02  | 15.8.1     | 2022-01      | 0.84       |
| [lru-cache](https://npmjs.org/package/lru-cache)             | 2011-07  | 10.0.0     | 2023-06      | 0.83       |
| [colors](https://npmjs.org/package/colors)                   | 2011-03  | 1.4.0      | 2019-09      | 0.83       |
| [tslib](https://npmjs.org/package/tslib)                     | 2014-12  | 2.6.0      | 2023-06      | 0.83       |
| [underscore](https://npmjs.org/package/underscore)           | 2011-01  | 1.13.6     | 2022-09      | 0.82       |
| [ms](https://npmjs.org/package/ms)                           | 2011-12  | 2.1.3      | 2020-12      | 0.82       |
| [postcss](https://npmjs.org/package/postcss)                 | 2013-11  | 8.4.25     | 2023-07      | 0.82       |
| [jsonwebtoken](https://npmjs.org/package/jsonwebtoken)       | 2013-07  | 9.0.1      | 2023-07      | 0.82       |
| [classnames](https://npmjs.org/package/classnames)           | 2014-11  | 2.3.2      | 2022-09      | 0.82       |
| [through2](https://npmjs.org/package/through2)               | 2013-08  | 4.0.2      | 2020-06      | 0.81       |
| [qs](https://npmjs.org/package/qs)                           | 2011-02  | 6.11.2     | 2023-05      | 0.81       |
| [@types/node](https://npmjs.org/package/@types/node)         | 2016-05  | 20.4.1     | 2023-07      | 0.81       |
| [mime](https://npmjs.org/package/mime)                       | 2011-01  | 3.0.0      | 2021-11      | 0.81       |
| [chokidar](https://npmjs.org/package/chokidar)               | 2012-04  | 3.5.3      | 2022-01      | 0.81       |
| [ora](https://npmjs.org/package/ora)                         | 2016-03  | 6.3.1      | 2023-05      | 0.8        |
| [winston](https://npmjs.org/package/winston)                 | 2011-01  | 3.10.0     | 2023-07      | 0.8        |
| [form-data](https://npmjs.org/package/form-data)             | 2011-05  | 4.0.0      | 2021-02      | 0.79       |
| [execa](https://npmjs.org/package/execa)                     | 2015-12  | 7.1.1      | 2023-03      | 0.79       |
| [iconv-lite](https://npmjs.org/package/iconv-lite)           | 2011-11  | 0.6.3      | 2021-05      | 0.79       |
| [jsonfile](https://npmjs.org/package/jsonfile)               | 2012-09  | 6.1.0      | 2020-10      | 0.79       |
| [xml2js](https://npmjs.org/package/xml2js)                   | 2011-04  | 0.6.0      | 2023-05      | 0.79       |
| [open](https://npmjs.org/package/open)                       | 2012-04  | 9.1.0      | 2023-03      | 0.79       |
| [prettier](https://npmjs.org/package/prettier)               | 2017-01  | 3.0.0      | 2023-07      | 0.78       |
| [babel-loader](https://npmjs.org/package/babel-loader)       | 2015-02  | 9.1.3      | 2023-07      | 0.78       |
| [path-to-regexp](https://npmjs.org/package/path-to-regexp)   | 2012-08  | 6.2.1      | 2022-05      | 0.78       |
| [readable-stream](https://npmjs.org/package/readable-stream) | 2012-07  | 4.4.2      | 2023-07      | 0.78       |
| [cross-spawn](https://npmjs.org/package/cross-spawn)         | 2014-06  | 7.0.3      | 2020-05      | 0.78       |
| [shelljs](https://npmjs.org/package/shelljs)                 | 2012-03  | 0.8.5      | 2022-01      | 0.78       |
| [ejs](https://npmjs.org/package/ejs)                         | 2011-02  | 3.1.9      | 2023-03      | 0.78       |
| [globby](https://npmjs.org/package/globby)                   | 2014-06  | 13.2.2     | 2023-07      | 0.77       |
| [object-assign](https://npmjs.org/package/object-assign)     | 2014-02  | 4.1.1      | 2017-01      | 0.77       |
| [autoprefixer](https://npmjs.org/package/autoprefixer)       | 2013-04  | 10.4.14    | 2023-03      | 0.77       |
| [resolve](https://npmjs.org/package/resolve)                 | 2011-06  | 1.22.2     | 2023-04      | 0.77       |
| [jest](https://npmjs.org/package/jest)                       | 2012-02  | 29.6.1     | 2023-07      | 0.77       |
| [html-webpack-plugin](https://npmjs.org/package/html-webpack-plugin) | 2014-08  | 5.5.3      | 2023-06      | 0.76       |
| [css-loader](https://npmjs.org/package/css-loader)           | 2012-04  | 6.8.1      | 2023-05      | 0.76       |
| [cors](https://npmjs.org/package/cors)                       | 2013-01  | 2.8.5      | 2018-11      | 0.76       |
| [escape-string-regexp](https://npmjs.org/package/escape-string-regexp) | 2014-06  | 5.0.0      | 2021-04      | 0.76       |
| [eslint-plugin-react](https://npmjs.org/package/eslint-plugin-react) | 2014-12  | 7.32.2     | 2023-01      | 0.76       |
| [graceful-fs](https://npmjs.org/package/graceful-fs)         | 2011-07  | 4.2.11     | 2023-03      | 0.76       |
| [compression](https://npmjs.org/package/compression)         | 2014-01  | 1.7.4      | 2019-03      | 0.76       |
| [jquery](https://npmjs.org/package/jquery)                   | 2011-03  | 3.7.0      | 2023-05      | 0.76       |
| [handlebars](https://npmjs.org/package/handlebars)           | 2011-08  | 4.7.7      | 2021-02      | 0.76       |
| [source-map](https://npmjs.org/package/source-map)           | 2011-08  | 0.7.4      | 2022-06      | 0.76       |
| [style-loader](https://npmjs.org/package/style-loader)       | 2012-04  | 3.3.3      | 2023-05      | 0.75       |
| [jsdom](https://npmjs.org/package/jsdom)                     | 2011-11  | 22.1.0     | 2023-05      | 0.75       |
| [eslint-plugin-import](https://npmjs.org/package/eslint-plugin-import) | 2015-03  | 2.27.5     | 2023-01      | 0.75       |



[来源](https://blog.sandworm.dev/state-of-npm-2023-most-popular-best-quality-packages)