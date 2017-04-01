关于npm version prerelease的作用我这里不再赘述，你可以查看[这个文章](NPM模块的TAG管理)。我只是记录一下关于npm version与npm dist-tag的使用：

第一步：发布第一个稳定版本
```js
 npm publish//1.0.0
```
第二步：修改文件继续发布第二个版本
```js
git add -A && git commit -m "c"
npm version patch
npm publish//1.0.1
```
第三步：继续修改文件发布一个prerelease版本
```js
 git add -A && git commit -m "c"
 npm version prerelease
 npm publish --tag -beta//版本n-n-n-n@1.0.2-0
```
第四步：继续修改发布第二个prerelease版本
```js
git add -A && git commit -m "c"
npm version prerelease
npm publish --tag -beta//版本n-n-n-n@1.0.2-1
```
第五步：npm info查看我们的版本信息
```js
{ name: 'n-n-n-n',
  'dist-tags': { latest: '1.0.1', '-beta': '1.0.2-1' },
  versions: [ '1.0.0', '1.0.1', '1.0.2-0', '1.0.2-1' ],
  maintainers: [ 'liangklfang <liangklfang@163.com>' ],
  time:
   { modified: '2017-04-01T12:17:56.755Z',
     created: '2017-04-01T12:15:23.605Z',
     '1.0.0': '2017-04-01T12:15:23.605Z',
     '1.0.1': '2017-04-01T12:16:24.916Z',
     '1.0.2-0': '2017-04-01T12:17:23.354Z',
     '1.0.2-1': '2017-04-01T12:17:56.755Z' },
  homepage: 'https://github.com/liangklfang/n#readme',
  repository: { type: 'git', url: 'git+https://github.com/liangklfang/n.git' },
  bugs: { url: 'https://github.com/liangklfang/n/issues' },
  license: 'ISC',
  readmeFilename: 'README.md',
  version: '1.0.1',
  description: '',
  main: 'index.js',
  scripts: { test: 'echo "Error: no test specified" && exit 1' },
  author: '',
  gitHead: '8123b8addf6fed83c4c5edead1dc2614241a4479',
  dist:
   { shasum: 'a60d8b02222e4cae74e91b69b316a5b173d2ac9d',
     tarball: 'https://registry.npmjs.org/n-n-n-n/-/n-n-n-n-1.0.1.tgz' },
  directories: {} }
```
我们只要注意下面者两个部分：
```js
 'dist-tags': { latest: '1.0.1', '-beta': '1.0.2-1' },
  versions: [ '1.0.0', '1.0.1', '1.0.2-0', '1.0.2-1' ],
```
其中最新的稳定版本和最新的beta版本可以在dist-tags中看到，而versions数组中存储的是所有的版本。

第六步：npm dist-tag命令
```js
npm dist-tag ls n-n-n-n
```
即npm dist-tag获取到所有的最新的版本，包括prerelease与稳定版本，得到下面结果：
```js
-beta: 1.0.2-1
latest: 1.0.1
```
第七步：当我们的prerelease版本已经稳定了，重新设置为稳定版本
```js
npm dist-tag add n-n-n-n@1.0.2-1 latest
```
此时你通过npm info查看可以知道：
```js
{ name: 'n-n-n-n',
  'dist-tags': { latest: '1.0.2-1', '-beta': '1.0.2-1' },
  versions: [ '1.0.0', '1.0.1', '1.0.2-0', '1.0.2-1' ],
  maintainers: [ 'liangklfang <liangklfang@163.com>' ],
  time:
   { modified: '2017-04-01T12:24:55.800Z',
     created: '2017-04-01T12:15:23.605Z',
     '1.0.0': '2017-04-01T12:15:23.605Z',
     '1.0.1': '2017-04-01T12:16:24.916Z',
     '1.0.2-0': '2017-04-01T12:17:23.354Z',
     '1.0.2-1': '2017-04-01T12:17:56.755Z' },
  homepage: 'https://github.com/liangklfang/n#readme',
  repository: { type: 'git', url: 'git+https://github.com/liangklfang/n.git' },
  bugs: { url: 'https://github.com/liangklfang/n/issues' },
  license: 'ISC',
  readmeFilename: 'README.md',
  version: '1.0.2-1',
  description: '',
  main: 'index.js',
  scripts: { test: 'echo "Error: no test specified" && exit 1' },
  author: '',
  gitHead: '03189d2cc61604aa05f4b93e429d3caa3b637f8c',
  dist:
   { shasum: '41ea170a6b155c8d61658cd4c309f0d5d1b12ced',
     tarball: 'https://registry.npmjs.org/n-n-n-n/-/n-n-n-n-1.0.2-1.tgz' },
  directories: {} }
```
主要关注如下:
```js
 'dist-tags': { latest: '1.0.2-1', '-beta': '1.0.2-1' },
  versions: [ '1.0.0', '1.0.1', '1.0.2-0', '1.0.2-1' ]
```
此时latest版本已经是prerelease版本"1.0.2-1"了！此时用户如果直接运行npm install就会安装我们的prerelease版本了，因为版本已经更新了！

当然，我们的npm publish可以有很多tag的，比如上面是beta，也可以是stable, dev, canary等，比如下面你继续运行：
```js
 git add -A && git commit -m "c"
 npm version prerelease
 npm publish --tag -dev
```
此时你运行npm info就会得到下面的信息：
```js
{ name: 'n-n-n-n',
  'dist-tags': { latest: '1.0.2-1', '-beta': '1.0.2-1', '-dev': '1.0.2-2' },
  versions: [ '1.0.0', '1.0.1', '1.0.2-0', '1.0.2-1', '1.0.2-2' ],
  maintainers: [ 'liangklfang <liangklfang@163.com>' ],
  time:
   { modified: '2017-04-01T13:01:17.106Z',
     created: '2017-04-01T12:15:23.605Z',
     '1.0.0': '2017-04-01T12:15:23.605Z',
     '1.0.1': '2017-04-01T12:16:24.916Z',
     '1.0.2-0': '2017-04-01T12:17:23.354Z',
     '1.0.2-1': '2017-04-01T12:17:56.755Z',
     '1.0.2-2': '2017-04-01T13:01:17.106Z' },
  homepage: 'https://github.com/liangklfang/n#readme',
  repository: { type: 'git', url: 'git+https://github.com/liangklfang/n.git' },
  bugs: { url: 'https://github.com/liangklfang/n/issues' },
  license: 'ISC',
  readmeFilename: 'README.md',
  version: '1.0.2-1',
  description: '',
  main: 'index.js',
  scripts: { test: 'echo "Error: no test specified" && exit 1' },
  author: '',
  gitHead: '03189d2cc61604aa05f4b93e429d3caa3b637f8c',
  dist:
   { shasum: '41ea170a6b155c8d61658cd4c309f0d5d1b12ced',
     tarball: 'https://registry.npmjs.org/n-n-n-n/-/n-n-n-n-1.0.2-1.tgz' },
  directories: {} }
```
重点关注如下内容
```js
 'dist-tags': { latest: '1.0.2-1', '-beta': '1.0.2-1', '-dev': '1.0.2-2' },
  versions: [ '1.0.0', '1.0.1', '1.0.2-0', '1.0.2-1', '1.0.2-2' ],
```
此时你会看到-beta版本最新是1.0.2-1，而-dev版本最新是1.0.2-2


参考资料：

[NPM模块的TAG管理](http://cnodejs.org/topic/537b47d1cbcc39634983b739)

[npm-dist-tag](https://docs.npmjs.com/cli/dist-tag)

[npm-version](https://docs.npmjs.com/cli/version)

[node-semver](https://github.com/liangklfang/node-semver)