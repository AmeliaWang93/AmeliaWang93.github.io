---
layout: post
title: '给自己找一个角落'
subtitle: '开坑篇'
date: 2020-08-16
categories: 技术
cover: 'http://on2171g4d.bkt.clouddn.com/jekyll-theme-h2o-postcover.jpg'
tags: 
---

> 可以尝试来记录点东西，或者说从今天开始慢慢成为一个有生活的人。
>加油，WIM！



----------------------

2020-08-22 更新

想来还是记录一下github上利用模板搭建博客的过程吧，一为我的朋友，二为感谢乐于在网上分享的同学们。



### 搭建过程

- 默认已经有了一个github账号

- 操作1-3步，都可参照https://zhuanlan.zhihu.com/p/28321740 ，遂不另附图片

####  1. 创建仓库【New repository】

​	**注意：**仓库名称组好是 `用户名.github.io`， 请替换成自己的用户名

#### 2. Settings配置

​	仓库的【Settings】- `GitHub Pages`部分，`Source`选择构建页面分支，选择`master branch`-【Save】

​	`Theme Chooser`  可选择一个主题，【Select theme】点击后你已经可以查看你的页面了，页面的地址为

`用户名.github.io`

​	**注意：**想要用`用户名.github.io`访问页面，仓库的名称必须是 `用户名.github.io`

#### 3. 克隆仓库

​	这一步可在本地安装git，使用命令行处理。更简单的方法，下载一个"GitHub桌面版"。

​	【Clone repository...】将github上的仓库拉取到本地。

#### 4. 换主题

​	用了[H2O]( https://github.com/kaeyleo/jekyll-theme-H2O)，觉得还OK，有心情可以自己写，先就着简单的来。

##### 基本操作：

- 把整个主题库clone下来
- 将自己库里除了`.git`以外的文件及文件夹删除，把H2O里除了`.git`以外的文件和文件夹拷贝进去
- 修改package.json，文件中的`name`可改换，另需要更新gulp相关库的版本，如下：

```json
"devDependencies": {
    "gulp": "^4.0.2",
    "gulp-clean-css": "^4.0.2",
    "gulp-rename": "^1.4.0",
    "gulp-sass": "^4.1.0",
    "gulp-uglify": "^3.0.2"
  }
```

​	**注意**： H2O老了，直接使用github会提示很多安全漏洞

- 其他配置修改参照主题 [H2O](https://github.com/kaeyleo/jekyll-theme-H2O )中的说明

##### 本地调试：

​	如果不满意网页的某些部分，可以自行搭建环境进行修改

- 首先你需要有nodejs

- 安装 Ruby + DevKit + Jekyll并启动调试，可参照[https://www.jianshu.com/p/b11c45afd3e6](https://www.jianshu.com/p/b11c45afd3e6)
  - 其中："进入你博客所在的文件夹，打开配置文件_config.yml，找到 gems: [jekyll-paginate] 这句语句，并将其改成 plugins: [jekyll-paginate]" 这个不需要做
  - 运行`jekyll s` 或者`jekyll server` 后，可在浏览器https://127.0.0.1:4000查看页面
  - `Ctrl+ c`可停止

- 将修改通过"GitHub桌面版"推送即可

**注意**：`.gitignore`文件中可过滤不需要上传的文件或文件夹，如`.idea`,`.jekyll-cache`等

#### 5. 添加gitalk

​	可参考[https://www.jianshu.com/p/796469455a43](https://www.jianshu.com/p/796469455a43)

- 个人账户的【Settings】-【Developer Settings】-【New OAuth App】-填写

```json
* Application name: 应用的名字，可以随便取（其他人想要评论的话，需要登录Github授权，授权的时候显示的就是这里的Application name）
* Homepage URL: 应用主页的完整URL，如: https://用户名.github.io/
* Application description: 应用描述，选填项
* Authorization callback URL: 登录授权后回调的页面，直接填成与Homepage URL一样
```

申请到的Client ID和Client Secret有用

- 添加gitalk: `_config.yml`中的`comments`

```js
comments:
  gitalk: true
  clientID: 申请的Client ID
  secret: 申请的Client Secret
  repo: 用户名.github.io
  owner: 用户名
  admin: 用户名
  distractionFreeMode: false
```

- 修改`_layouts/`下的`post.html`文件
  1. 可将`gitalk.min.js`和`gitalk.css`下载到assets文件夹中，引用方式如下：

```html
<link rel="stylesheet" href="/assets/css/gitalk.css">
<script src="/assets/js/gitalk.min.js"></script>
<script src="/assets/js/md5.min.js"></script>
```

​		2. 在需要显示评论框的地方加入div，此处的`id`在初始化中会用到：

```heml
 <div>
    {% if site.comments.gitalk %}
    <section class="post-footer-item comment">
      <div id="gitalk-container"></div>
    </section>
    {% endif %}
  </div>
```

​		3. 加入一段srcipt，初始化代码：

```js
{% if site.comments.gitalk %}
  <script>
    var gitalk = new Gitalk({
      id: md5(location.pathname),
      clientID: '{{ site.comments.clientID }}',  //这里其实可以直接填值，但是考虑到页面安全性，还是通过配置的方式添加
      clientSecret: '{{ site.comments.secret }}',
      repo: '{{ site.comments.repo }}',
      owner: '{{ site.comments.owner }}',
      admin: '{{ site.comments.admin }}',
      distractionFreeMode: '{{ site.comments.distractionFreeMode }}'
    })

    gitalk.render('gitalk-container')
  </script>
  {% endif %}
```



注意：添加gittalk最重要的两点：

- id要小于50，因此添加了`md5`库；
- 网页要推到github，需要自己先访问登录
- 相关问题可参考：[https://blog.csdn.net/weiwosuoai/article/details/90573929](https://blog.csdn.net/weiwosuoai/article/details/90573929)

#### 6. 写在最后

页面上任何想diy的地方，都可进行调整。



----

某一天我可能会再改改或者索性自己写个模板，就是有可能，O(∩_∩)O哈哈~