### Rollup - 另一个前端模块化的打包工具

> Rollup 是前端模块化的一个打包工具，可以将小块代码编译成大块复杂的代码，例如 library 或应用程序。简单地说，**它可以从一个入口文件开始，将所有使用的模块根据命令或者根据 Rollup 配置文件打包成一个目标文件，并且 Rollup 会自动过滤掉那些没有被使用过的函数或变量，从而使代码最小化，如果想使用直接导入这一个目标文件即可，因此 Rollup 极其适合构建一个工具库。**
>
> 这里提到 Rollup 的两个特别重要的特性，**第一个就是它使用了 ES2015 的模板标准，这意味着你可以直接使用 import 和 export 而不需要引入 babel。另一个重要特性叫做 tree-shaking，这个特性可以帮助你将无用代码（即没有使用的代码）从最终的目标文件中过滤掉。**举个简单的例子，我们在 foo.js 文件定义了 f1 和 f2 两个方法，然后在入口文件 index.js 只引入了 foo.js 文件中的 f1 方法，那么在最后打包 index.js 文件时，Rollup 就不会将 f2 方法打包到最终文件中（这个特性是基于 ES6 模块的静态分析的，也就是说，只有 export 而没有 import 的变量是不会被打包到最终代码中的）。

#### 为什么用前端模块化的打包工具 Rollup

之前的构建系统是基于 Gulp/Grunt+Browserify 手搓的一套工具，后来在扩展方面受限于工具，例如：

Node 环境下性能不好：频繁的`process.env.NODE_ENV`访问拖慢了`SSR 性能`，但又没办法从类库角度解决，因为`Uglify`依靠这个去除无用代码，所以`React SSR`性能最佳实践一般都有一条“`重新打包 React，在构建时去掉 process.env.NODE_ENV`”.

丢弃了过于复杂（overly-complicated）的自定义构建工具，改用更合适的 Rollup：

> It solves one problem well: how to combine multiple modules into a flat file with minimal junk code in between.

无论 Haste -> ES Module 还是 Gulp/Grunt+Browserify -> Rollup 的切换都是从非标准的定制化方案切换到标准的开放的方案，应该在“手搓”方面吸取教训，为什么业界规范的东西在我们的场景不适用，非要自己造吗？

#### 如何用前端模块化的打包工具 Rollup

关于如何使用前端模块化的打包工具 Rollup，这里就不做过多介绍了，可参考我之前写的一篇文章：[Rollup使用指南2](http://www.sosout.com/2018/08/04/rollup-tutorial.html)，更详细的使用文档可参考：[官网](https://www.rollupjs.com/guide/zh)。

#### Webpack 和 Rollup 有什么不同

2017年4月初，Facebook 将一个巨大的 pull 请求合并到了 React 主分支(master)中，将其现有的构建流程替换为基于 Rollup，这一举动促使一些人产生很大的疑惑“React 为什么选择 Rollup 而抛弃 webpack”，难道webpack要跌下神坛了？

Webpack 是目前使用最为火热的打包工具，没有之一，每月有数百万的下载量，为成千上万的网站和应用提供支持。相比之下，Rollup 并不起眼。但 React 并不孤单 – Vue，Ember，Preact，D3，Three.js，Moment 以及其他许多知名的库也使用 Rollup 。世界到底怎么了？为什么我们不能只有一个大众认可的 JavaScript 模块化打包工具？

Webpack 始于2012年，由 Tobias Koppers 发起，用于解决当时现有工具未解决的的一个难题：**构建复杂的单页应用程序(SPA)。**特别是 webpack 的两个特性改变了一切：

**第一个特性：**代码拆分(Code Splitting)

代码拆分也就是说你可以将应用程序分解成可管理的代码块，可以按需加载，这意味着用户可以快速获取网站内容，而不必等到整个应用程序下载和解析完成。

**第二个特性：**各式各样的加载器（loader）

不管是图像，css，还是 html ，在 Webpack 看来一切都可作为模块，然后通过不同的加载器 loader 来加载它们。

ES6 发布之后，其中引入的模块机制使得静态分析成为了可能，于是 Rollup 发布了：其中 Rollup 有两个特别重要的特性，第一个就是它利用 ES2015 巧妙的模块设计，尽可能高效的构建出能够直接被其他 Javascript 库的。另一个重要特性叫做 tree-shaking，这个特性可以帮助你将无用代码（即没有使用的代码）从最终的目标文件中过滤掉。

紧接着 Webpack2 发布，仿照 Rollup 增加了 tree-shaking。 在之后， Webpack3 发布，仿照 Rollup 又增加了 Scope Hoisting。在在之后， Parcel 发布了一个快速、零配置的打包工具。于是，Webpack4 仿照 Parcel 发布了。

说了这么多，工作中我们到底该用哪个工具？

对于应用使用 webpack，对于类库使用 Rollup。如果你需要代码拆分(Code Splitting)，或者你有很多静态资源需要处理，再或者你构建的项目需要引入很多 CommonJS 模块的依赖，那么 webpack 是个很不错的选择。如果您的代码库是基于 ES2015 模块的，而且希望你写的代码能够被其他人直接使用，你需要的打包工具可能是 Rollup。

#### 小结

无论 Haste -> ES Module 还是 Gulp/Grunt+Browserify -> Rollup 的切换都是从非标准的定制化方案切换到标准的开放的方案，可以看出 React 团队也在积极拥抱标准方案并非一味造轮子。其实 Vue.js 1.0.10 就已经使用 Rollup 了，而 React v16.0 改用 Rollup 肯定也有借鉴之意，因此，好技术都是在借鉴的大背景下诞生的（Vue 就是一个典型的例子）。在这里通过对 Rollup 的认识，有助于我们了解 React 的构建以及源码目录结构。