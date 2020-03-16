## 安装VuePress

> 注意
>
> 请确保你的 Node.js 版本 >= 8。

### 安装yarn

> 也可以安装npm

#### Debian / Ubuntu

在 Debian 或 Ubuntu 上，需要用yarn的 Debian 包仓库来安装 Yarn。 首先需要配置仓库：

```shell
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
```

更新库和下载yarn

```shell
sudo apt-get update && sudo apt-get install yarn
```

运行命令来测试 Yarn 是否安装：

```shell
$ yarn --version
1.22.4
```

#### Windows

直接下安装包，然后在CMD或者Powershell里运行

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

### 安装VuePress

```shell
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

用浏览器打开vuepress生成的网页的地址



或者构建静态文件

```bash
# 构建静态文件
vuepress build .
```

但会因为路径不对，网页的样式显示不出来





### VuePress目录结构

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
│   ├── README.md     //vuepress首页展示用的markdown文件
├── deploy.sh     //用于编写TravisCI上传、发布的脚本文件
├── LISENSE     //许可证文件
└── package.json     //Node.js项目描述文件
```



## 页面的内容设置

### 配置package.json

在 `package.json` 里加一些脚本:

```json
{
  "scripts": {
    "docs:dev": "vuepress dev docs",
    "docs:build": "vuepress build docs"
  },
  "dependencies": {
    "vuepress": "^1.3.1"
  }
}
```

可加载依赖的命令

```bash
yarn docs:dev # 或者：npm run docs:dev
yarn docs:build # 或者：npm run docs:build
```



###  配置主题

####  首页

`/docs/README.md`

```yaml
---
home: true
heroImage: /space_invader.png
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



####  导航栏

`/docs/.vuepress/config.js`

```
module.exports = {
  themeConfig: {
    nav: [
      { text: 'Home', link: '/' },
      { text: 'Guide', link: '/guide/' },
      { text: 'External', link: 'https://google.com' },
    ]
  }
}
```



####  侧边栏