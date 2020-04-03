## 安装 VuePress

::: warning 注意
请确保你的 Node.js 版本 >= 8。
:::

### 安装 yarn

> 也可以安装 npm

#### Debian / Ubuntu

在 Debian 或 Ubuntu 上，需要用 yarn 的 Debian 包仓库来安装 Yarn。 首先需要配置仓库：

```shell
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
```

更新库和下载 yarn

```shell
sudo apt-get update && sudo apt-get install yarn
```

运行命令来测试 Yarn 是否安装：

```shell
$ yarn --version
1.22.4
```

#### Windows

直接下安装包，然后在 CMD 或者 Powershell 里运行

#### 更换国内的源

先看一下当前的源

```shell
$ yarn config get registry
https://registry.yarnpkg.com
```

更换阿里淘宝的源

```shell
yarn config set registry https://registry.npm.taobao.org
```

### 安装 VuePress

```shell
# 先进入安装目录，就是刚刚克隆的仓库
cd ~/VuePress-GithubPages-TravisCI
# 安装
sudo yarn global add vuepress # 或者：npm install -g vuepress
```

然后试一下看是否安装成功

```shell
# 新建一个 markdown 文件
echo '# Hello VuePress!' > README.md

# 开始写作
vuepress dev .
```

```shell
ℹ ｢wds｣: Project is running at http://0.0.0.0:8081/
ℹ ｢wds｣: webpack output is served from /
ℹ ｢wds｣: Content not from webpack is served from /mnt/k/Git_Bash/VuePress-GithubPages-TravisCI/.vuepress/public
ℹ ｢wds｣: 404s will fallback to /index.html
success [00:00:17] Build 471ee0 finished in 8465 ms!
> VuePress dev server listening at http://localhost:8081/

# 生成的地址 http://localhost:8081/
```

用浏览器打开 vuepress 生成的网页的地址

或者构建静态文件

```bash
# 构建静态文件
vuepress build .
```

但会因为路径不对，网页的样式显示不出来

### VuePress 目录结构

官方给的结构

> VuePress 遵循 **“约定优于配置”** 的原则，推荐的目录结构如下：
>
> ```shell
> .
> ├── docs
> │   ├── .vuepress (可选的)
> │   │   ├── components (可选的)
> │   │   ├── theme (可选的)
> │   │   │   └── Layout.vue
> │   │   ├── public (可选的)
> │   │   ├── styles (可选的)
> │   │   │   ├── index.styl
> │   │   │   └── palette.styl
> │   │   ├── templates (可选的, 谨慎配置)
> │   │   │   ├── dev.html
> │   │   │   └── ssr.html
> │   │   ├── config.js (可选的)
> │   │   └── enhanceApp.js (可选的)
> │   │
> │   ├── README.md
> │   ├── guide
> │   │   └── README.md
> │   └── config.md
> │
> └── package.json
> ```

这里用到的结构

```shell
.
├── README.md     // Github项目展示文件
├── docs     //vuepress项目根目录
│   ├── .vuepress      //存放核心内容的文件夹
│   │   ├── public     //存放静态文件，如图片等
│   │   └── config.js     //设定顶部导航栏、侧边导航栏等项目配置的核心文件
│   ├── pages      //存放markdown页面的文件
│   └── README.md     //vuepress首页展示用的markdown文件
├── deploy.sh     //用于编写TravisCI上传、发布的脚本文件
├── LISENSE     //许可证文件
├── package.json     //Node.js项目描述文件
└── .travis.yml	//Travis CI 自动部署文件
```

## 配置依赖和脚本

### 配置 package.json

在 `package.json` 里加一些脚本和后面要用的依赖:

```json
{
  "dependencies": {
    "@vuepress/plugin-active-header-links": "^1.3.1",
    "@vuepress/plugin-medium-zoom": "^1.3.1",
    "@vuepress/plugin-nprogress": "^1.3.1",
    "@vuepress/plugin-back-to-top": "^1.3.1",
    "vuepress": "^1.3.1"
  },
  "scripts": {
    "docs:build": "vuepress build docs",
    "docs:dev": "vuepress dev docs"
  }
}
```

加载依赖

```shell
yarn
```

命令

```bash
yarn docs:dev # 或者：npm run docs:dev
yarn docs:build # 或者：npm run docs:build
```

## 页面的设置

### 首页

`/docs/README.md`

```yaml
---
home: true
heroImage: https://cdn-image.tsanfer.xyz/img/vuepress_githubpages_travisCI.svg
actionText: 快速上手 →
actionLink: /pages/思路.md
features:
  - title: 简洁至上
    details: 以 Markdown 为中心的项目结构，以最少的配置帮助你专注于写作。
  - title: Vue驱动
    details: 享受 Vue + webpack 的开发体验，在 Markdown 中使用 Vue 组件，同时可以使用 Vue 来开发自定义主题。
  - title: 高性能
    details: VuePress 为每个页面预渲染生成静态的 HTML，同时在页面被加载的时候，将作为 SPA 运行。
footer: MIT Licensed | Copyright © 2020 Tsanfer
---

```

### 文档属性

`/docs/.vuepress/config.js`

```js
module.exports = {
  base: "/VuePress-GithubPages-TravisCI/", //目录根地址，应与Github仓库名字相同
  title: "VuePress + GithubPages + TravisCI", // 显示在左上角的网页名称以及首页在浏览器标签显示的title名称
  description: "创建 VuePress + GithubPages + TravisCI 在线文档", // meta 中的描述文字，用于SEO
  head: [
    [
      "link",
      { rel: "icon", href: "/gamepad_game_128px.ico" } //浏览器的标签栏的网页图标,基地址/docs/.vuepress/public
    ]
  ]
};
```

### markdown 扩展

`/docs/.vuepress/config.js`

```js
module.exports = {
  markdown: {
    lineNumbers: true //是否在每个代码块的左侧显示行号
  }
};
```

### 默认主题设置

#### 导航栏

`/docs/.vuepress/config.js`

```js
module.exports = {
  themeConfig: {
    nav: [
      //链接页面链接的根地址为/docs
      { text: "思路", link: "/pages/flow.md" },
      { text: "创建Github仓库", link: "/pages/Github.md" },
      { text: "配置VuePress", link: "/pages/VuePress.md" },
      { text: "TravisCI生成和发布", link: "/pages/TravisCI.md" },
      { text: "博客", link: "https://tsanfer.xyz" }
    ]
  }
};
```

#### 侧边栏

`/docs/.vuepress/config.js`

```js
module.exports = {
  themeConfig: {
    sidebarDepth: 2, //侧边栏深度
    sidebar: [
      ["/pages/flow.md", "思路"],
      ["/pages/Github.md", "创建Github仓库"],
      ["/pages/VuePress.md", "配置VuePress"],
      ["/pages/TravisCI.md", "TravisCI生成和发布"]
    ]
  }
};
```

#### Git 仓库

`/docs/.vuepress/config.js`

```js
module.exports = {
  themeConfig: {
    // 假定是 GitHub. 同时也可以是一个完整的 GitLab URL
    repo: "Tsanfer/VuePress-GithubPages-TravisCI",
    // 自定义仓库链接文字。默认从 `themeConfig.repo` 中自动推断为
    // "GitHub"/"GitLab"/"Bitbucket" 其中之一，或是 "Source"。
    repoLabel: "Github",
    // 以下为可选的编辑链接选项
    // 假如文档不是放在仓库的根目录下：
    docsDir: "docs/pages",
    // 假如文档放在一个特定的分支下：
    docsBranch: "master",
    // 默认是 false, 设置为 true 来启用
    editLinks: true,
    // 默认为 "Edit this page"
    editLinkText: "在 Github 上编辑此页"
  }
};
```

#### 其他

`/docs/.vuepress/config.js`

```js
module.exports = {
  themeConfig: {
    smoothScroll: true, //页面滚动效果
    lastUpdated: "最后更新" // string | boolean
  }
};
```

### 插件

`/docs/.vuepress/config.js`

```js
module.exports = {
  plugins: [
    "@vuepress/medium-zoom", //zooming images like Medium（页面弹框居中显示）
    "@vuepress/nprogress", //网页加载进度条
    "@vuepress/plugin-back-to-top" //返回页面顶部按钮
  ]
};
```

::: tip 提示
到这里其实已经完成配置了，可以执行 `yarn docs:dev` 来浏览配置的页面，

只是由于没有对应的 md 文件，打开的链接都会 404

:::

### config.js 所有内容

```js
module.exports = {
  base: "/VuePress-GithubPages-TravisCI/", //目录根地址，应与Github仓库名字相同
  title: "VuePress + GithubPages + TravisCI", // 显示在左上角的网页名称以及首页在浏览器标签显示的title名称
  description: "创建 VuePress + GithubPages + TravisCI 在线文档", // meta 中的描述文字，用于SEO
  head: [
    [
      "link",
      { rel: "icon", href: "/gamepad_game_128px.ico" } //浏览器的标签栏的网页图标,基地址/docs/.vuepress/public
    ]
  ],

  //markdown扩展
  markdown: {
    lineNumbers: true //是否在每个代码块的左侧显示行号
  },

  //默认主题配置
  themeConfig: {
    //导航栏
    nav: [
      //链接页面链接的根地址为/docs
      { text: "思路", link: "/pages/flow.md" },
      { text: "创建Github仓库", link: "/pages/Github.md" },
      { text: "配置VuePress", link: "/pages/VuePress.md" },
      { text: "TravisCI生成和发布", link: "/pages/TravisCI.md" },
      { text: "博客", link: "https://tsanfer.xyz" }
    ],
    sidebarDepth: 2, //侧边栏深度
    //侧边栏
    sidebar: [
      ["/pages/flow.md", "思路"],
      ["/pages/Github.md", "创建Github仓库"],
      ["/pages/VuePress.md", "配置VuePress"],
      ["/pages/TravisCI.md", "TravisCI生成和发布"]
    ],

    // 假定是 GitHub. 同时也可以是一个完整的 GitLab URL
    repo: "Tsanfer/VuePress-GithubPages-TravisCI",
    // 自定义仓库链接文字。默认从 `themeConfig.repo` 中自动推断为
    // "GitHub"/"GitLab"/"Bitbucket" 其中之一，或是 "Source"。
    repoLabel: "Github",
    // 以下为可选的编辑链接选项
    // 假如文档不是放在仓库的根目录下：
    docsDir: "docs",
    // 假如文档放在一个特定的分支下：
    docsBranch: "master",
    // 默认是 false, 设置为 true 来启用
    editLinks: true,
    // 默认为 "Edit this page"
    editLinkText: "在 Github 上编辑此页",

    smoothScroll: true, //页面滚动效果
    lastUpdated: "最后更新" // string | boolean
  },

  //插件
  plugins: [
    "@vuepress/medium-zoom", //zooming images like Medium（页面弹框居中显示）
    "@vuepress/nprogress", //网页加载进度条
    "@vuepress/back-to-top", //返回页面顶部按钮
    "@vuepress/nprogress", //提示加载进度
    "reading-progress" //提示阅读进度
  ]
};

```
