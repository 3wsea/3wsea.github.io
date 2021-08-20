---
title: Chirpy Jekyll主题安装
date: 2021-08-20 10:43:00 +0800
categories: [安装, 博客]
tags: [jekyll]
pin: false
---

<div align="right">
  <a href="https://github.com/cotes2020/jekyll-theme-chirpy#readme">EN</a> |
  <a href="https://github.com/cotes2020/jekyll-theme-chirpy/blob/master/docs/README.zh-CN.md">中文 (简体)</a>
</div>

<div align="center">
  <h1>Chirpy Jekyll Theme</h1>
  <p>
  </p>
</div>

一个采用了最简化、侧边栏、响应式设计的 Jekyll 主题，专注于文本展示，旨在帮助您轻松地记录和分享知识。

[在线体验 »](https://chirpy.cotes.info)

<p align="center">
  <a href="https://chirpy.cotes.info">
    <img alt="Devices Mockup" src="https://cdn.jsdelivr.net/gh/cotes2020/chirpy-images/commons/devices-mockup.png">
  </a>
</p>

## 功能一览

- 本地化外观语言
- 可配置的主题颜色
- 文章置顶
- 文章最后修改日期
- 文章目录
- 自动推荐相关文章
- 语法高亮
- 二级目录
- 数学表达式
- Mermaid 图表
- 搜索
- Atom 订阅
- Disqus 评论
- Google 分析
- GA 浏览报告（高级功能）
- SEO 优化
- 网站性能优化


## 前提要求

参考 [Jekyll Docs](https://jekyllrb.com/docs/installation/) 安装 `Ruby`，`RubyGems`，`Jekyll` 和 `Bundler`。需要注意的是，`Ruby` 的版本必须与主题在 [RubyGems.org](https://rubygems.org/gems/jekyll-theme-chirpy) 上的要求一致.

## 安装

有二法可得此主题:

- **[从 RubyGems 安装](#从-rubygems-安装)** - 易于版本升级，隔离无关的主题项目文件，让您的仓库舒适清爽。
- **[从 GitHub 上 Fork](#从-github-上-fork)** - 对个性化二次开发友好，但是难于升级，只适合专业开发人员使用。

### 从 RubyGems 安装

在您的 Jekyll 站点的 `Gemfile` 添加:

```ruby
gem "jekyll-theme-chirpy"
```

然后，添加这行到您的 Jekyll 站点的 `_config.yml`:

```yaml
theme: jekyll-theme-chirpy
```

接着执行:

```console
$ bundle
```

然后，进入主题的 gem 目录:

```console
$ cd "$(bundle info --path jekyll-theme-chirpy)"
```

拷贝运行站点所需主题的 gem 文件（详见 [starter 仓库][starter] 的文件目录）至您的 Jekyll 站点, 然后把主题的 `_config.yml` 全部内容附加到您的 Jekyll 站点的同名文件。

> ⚠️ **留意重叠的文件！**
>
> 如果您的网站是通过命令 `jekyll new` 创建的，那么站点的根目录会存在文件 `index.markdown` 和 `about.markdown`。 请务必删除它们, 否则在网站构建后将覆盖主题的 `index.html` 和 `_tabs/about.html`，引起空白或错乱的页面出现。

作为替代方案，同时也被我们力荐，您可以 [使用 starter template][use-starter] 来快速创建 Jekyll 站点，以省去复制主题 gem 文件的时间。那里早已为您准备好建站需要的一切！


### 从 GitHub 上 Fork

[Fork **Chirpy**](https://github.com/cotes2020/jekyll-theme-chirpy/fork) 然后克隆到本地。(友情提示：默认分支的代码处于开发状态，如果您想博客更加稳定，请切换到最新的 [Tag](https://github.com/cotes2020/jekyll-theme-chirpy/tags) 开始写作。)

安装依赖：

```console
$ bundle
```

接着执行文件初始化:

```console
$ bash tools/init.sh
```

> 如果您不打算部署到 GitHub Pages, 在上述命令后附加参数选项 `--no-gh`。

上述脚本完成了以下工作:

1. 从您的仓库中删除了:
    - `.travis.yml`
    - `_posts` 下的文件
    - `docs` 目录

2. 如果使用了参数 `--no-gh`，则会怒删 `.github`。否则，将会配置 GitHub Actions：把 `.github/workflows/pages-deploy.yml.hook` 的后缀 `.hook` 去除，然后删除 `.github` 里的其他目录和文件。

3. 自动提交一个 Commit 以保存上述文件的更改。

## 使用

### 配置文件

根据个人需要去修改 `_config.yml` 的变量，大部分都有注释介绍用法。典型的几个选项是：

- `url`
- `avatar`
- `timezone`
- `lang`

### 自定义样式

如果您需要自定义样式, 拷贝主题的文件 `assets/css/style.scss` 到您站点的相同路径上，然后在该文件末尾添加样式。

自 `v4.1.0` 起，如果您想覆盖文件 `_sass/addon/variables.scss` 里定义的 SASS 变量的默认值，新建文件 `_sass/variables-hook.scss`，然后重写您需要的变量即可。

### 本地运行

发布之前，在本地预览:

```terminal
$ bundle exec jekyll s
```

或者用 Docker 运行:

```terminal
$ docker run -it --rm \
    --volume="$PWD:/srv/jekyll" \
    -p 4000:4000 jekyll/jekyll \
    jekyll serve
```

访问本地服务：<http://localhost:4000>

### 部署

部署开始前，把  `_config.yml` 的 `url` 改为 `https://<username>.github.io`(或者您的私有域名，如：`https://yourdomain.com`)。另外，如果您想使用 [Project 类型网站](https://help.github.com/en/github/working-with-github-pages/about-github-pages#types-of-github-pages-sites)，修改配置文件的 `baseurl` 为项目名称，以斜杠开头，如：`/project`。

现在您可以选择下列其中一个方式去站点部署。

#### 部署到 GitHub Pages

由于安全原因，GitHub Pages 的构建强制加了 `safe`参数，这导致了我们不能使用插件去创建所需的附加页面。因此，我们可以使用 GitHub Actions 去构建站点，把站点文件存储在一个新分支上，再指定该分支作为 Pages 服务的源。

快速检查 GitHub Actions 构建需要的文件:

- 确保您的 Jekyll 站点存在文件 `.github/workflows/pages-deploy.yml`。没有的话，新建并填入[示例工作流][workflow]的内容, 注意参数 `on.push.branches` 的值必须和您的仓库默认分支名相同。
- 检查您的 Jekyll 站点是否有文件 `tools/test.sh` 和 `tools/deploy.sh`. 没有的话, 从本仓库拷贝到您的 Jekyll 项目.

在 GitHub 把您的仓库命名为 `<GH-USERNAME>.github.io`，然后开始发布：

1. 推送任意一个 commit 到 `origin/master` 以触发 GitHub Actions workflow。一旦 build 完毕并且成功，远端将会自动出现一个新分支 `gh-pages` 用来存储构建的站点文件。

2. 回到 GitHub 上的仓库， 通过 _Settings_ → _Options_ → _GitHub Pages_ 选择分支 `gh-pages` 作为[_发布源_](https://docs.github.com/en/github/working-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site):

    ![gh-pages-sources](https://cdn.jsdelivr.net/gh/cotes2020/chirpy-images/posts/20190809/gh-pages-sources.png)

3. 按照 GitHub 指示的地址去访问您的网站。

#### 部署到其他 Pages 平台

在 GitHub 之外的平台，例如 GitLab，就没法享受 **GitHub Actions** 的便利了。因此我们需要在本地构建站点（或者在其他第三方 CI 平台），然后推送站点文件到服务器上。

在项目根目录，运行:

```console
$ JEKYLL_ENV=production bundle exec jekyll b
```

或者通过 Docker 构建:

```terminal
$ docker run -it --rm \
    --env JEKYLL_ENV=production \
    --volume="$PWD:/srv/jekyll" \
    jekyll/jekyll \
    jekyll build
```

生成的静态文件将会在 `_site`， 把内部的文件上传到服务器即可。

## 文档

若想要更多细节以及更佳的阅读体验，请参阅 [线上教程](https://chirpy.cotes.info/categories/tutorial/)。 与此同时，[Wiki](https://github.com/cotes2020/jekyll-theme-chirpy/wiki) 也有一份教程的拷贝。请注意，Demo 网站和 Wiki 上的文档都是基于最新的发行版本，而 `master` 分支的功能往往领先于现有文档。

[starter]: https://github.com/cotes2020/chirpy-starter
[use-starter]: https://github.com/cotes2020/chirpy-starter/generate
[workflow]: https://github.com/cotes2020/jekyll-theme-chirpy/blob/master/.github/workflows/pages-deploy.yml.hook

<!-- ReadMe links -->

[jb]: https://www.jetbrains.com/?from=jekyll-theme-chirpy
[cn-donation]: https://cotes.gitee.io/alipay-wechat-donation/
