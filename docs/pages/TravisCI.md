## 创建gh-pages分支

![](https://cdn-image.tsanfer.xyz/img/20200316170443.png)

这时Github已经自动部署gh-pages分支为Github pages的生成源

![](https://cdn-image.tsanfer.xyz/img/20200316170600.png)



## deploy.sh部署文件

每当 Github 仓库更新时，会触发 Travis CI 执行 `deploy.sh` 脚本

创建一个如下的 `deploy.sh` 文件（请自行判断去掉高亮行的注释）:

在项目根目录下创建

```shell {9}
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
├── package.json     //Node.js项目描述文件
└── .travis.yml	//Travis CI 自动部署文件
```

```bash
#!/usr/bin/env sh

# 确保脚本抛出遇到的错误
set -e

# 生成静态文件
yarn docs:build

# 进入生成的文件夹
cd docs/.vuepress/dist

# 如果是发布到自定义域名
# echo 'www.example.com' > CNAME

git init
git add -A
git commit -m 'deploy'

# 如果发布到 https://<USERNAME>.github.io
# git push -f git@github.com:<USERNAME>/<USERNAME>.github.io.git master

# 如果发布到 https://<USERNAME>.github.io/<REPO>
git push -f git@github.com:<USERNAME>/<REPO>.git master:gh-pages
# 比如
# git push -f git@github.com:Tsanfer/VuePress-GithubPages-TravisCI.git master:gh-pages

cd -
```

上面的 git 地址其实就是仓库的SSH地址

![](https://cdn-image.tsanfer.xyz/img/20200316171506.png)



## Travis CI 部署文件

在项目的根目录创建一个名为 `.travis.yml` 的文件

```js {12}
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
├── package.json     //Node.js项目描述文件
└── .travis.yml	//Travis CI 自动部署文件
```

```yaml
language: node_js
node_js:
  - lts/*
install:
  - yarn install # npm ci
script:
  - yarn docs:build # npm run docs:build
deploy:
  provider: pages
  skip_cleanup: true
  local_dir: docs/.vuepress/dist
  github_token: $GITHUB_TOKEN # 在 GitHub 中生成，用于允许 Travis 向你的仓库推送代码。在 Travis 的项目设置页面进行配置，设置为 secure variable
  keep_history: true
  on:
    branch: master #这里指的是部署前的源文件分支
```

上面的 github_token 需要在 Github 上生成



## 生成和使用 Token 

### 生成Token

在 Settings --> Developer settings --> Personal access tokens 右上角 Generate new toekn 生成新Token 名字随便写，权限不清楚的可以全部选上，也可以参考我下面的配置

![](https://cdn-image.tsanfer.xyz/img/20200316174659.png)

![](https://cdn-image.tsanfer.xyz/img/20200316174841.png)

![](https://cdn-image.tsanfer.xyz/img/20200316175236.png)

::: warning 注意
下面的口令只出现一次，需及时保存
:::

![](https://cdn-image.tsanfer.xyz/img/20200316175539.png)



## Travis CI 绑定和配置

### 绑定 Github 账号

在 Travis CI 里面 Settings ---> Repositories 点击  Manage repositories on GitHub 

![](https://cdn-image.tsanfer.xyz/img/20200316175949.png)

选择给权限的仓库，为了方便也可以把所有仓库的权限都给了

![](https://cdn-image.tsanfer.xyz/img/20200316180225.png)



### 添加 Token

在项目的 Settings --> Environment Variables 中输入 Token

```yaml {12}
language: node_js
node_js:
  - lts/*
install:
  - yarn install # npm ci
script:
  - yarn docs:build # npm run docs:build
deploy:
  provider: pages
  skip_cleanup: true
  local_dir: docs/.vuepress/dist
  github_token: $GITHUB_TOKEN # 在 GitHub 中生成，用于允许 Travis 向你的仓库推送代码。在 Travis 的项目设置页面进行配置，设置为 secure variable
  keep_history: true
  on:
    branch: master #这里指的是部署前的源文件分支
```

- NAME : GITHUB_TOKEN （刚刚的 `github_token: $GITHUB_TOKEN` 这个变量）
- VALUE : \*\*\*\*刚刚的 Token\*\*\*\*

![](https://cdn-image.tsanfer.xyz/img/20200316181315.png)



## 推送到Github

```shell
git add .
git commit -m '初步完成'
git push -f git@github.com:{Username}/{Repo}.git master

# 比如
# git push -f git@github.com:Tsanfer/VuePress-GithubPages-TravisCI.git master
```



## 完成

如果 Travis CI 触发成功构建没有问题的话就完成了

![](https://cdn-image.tsanfer.xyz/img/20200316183049.png)

> 本文由[Tsanfer's Blog](https://tsanfer.xyz) 发布！ 