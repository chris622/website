---
authors:
- admin
categories:
- website
- 教程
date: "2023-04-27"
draft: false
featured: false
lastmod: "2023-04-27"
links:
- icon: database
  icon_pack: fas
  name: Raw Tutorial
  url: https://bookdown.org/yihui/blogdown/
- icon: book
  icon_pack: fas
  name: Tutorial on Rpubs
  url: https://rpubs.com/Chris622/build-website
projects: []
subtitle: "🙌使用R：blogdown，Git和Netlify搭建个人网站"
summary: "🙌使用R：blogdown，Git和Netlify搭建个人网站"
tags:
- website
- blogdown
- Netlify
- Git
- R
title: Website | 用blogdown制作个人网站
---


# 准备工作

## R studio，Git desktop，Github

-   下载[R](https://cloud.r-project.org/bin/)
-   下载[R studio](https://posit.co/products/open-source/rstudio/)
-   下载[Git desktop](https://desktop.github.com/)
-   注册[github](https://github.com/)
-   下载[hugo](https://github.com/gohugoio/hugo/releases)

## 创建仓库和R项目

-   github创建新仓库 

<div class="figure" style="text-align: center">
<img src="fig/build_web/create_repo.png" alt="创建github仓库" width="100%" />
<p class="caption">Figure 1: 创建github仓库</p>
</div>

-   复制仓库URL

<div class="figure" style="text-align: center">
<img src="fig/build_web/clone_URL.png" alt="复制仓库URL" width="100%" />
<p class="caption">Figure 2: 复制仓库URL</p>
</div>

-   创建和Github连接的R项目:`R studio-File-New project-Version control-Git`

<div class="figure" style="text-align: center">
<img src="fig/build_web/R_proj.png" alt="创建和Github连接的R项目" width="100%" />
<p class="caption">Figure 3: 创建和Github连接的R项目</p>
</div>

# 创建网站

-   在建好的项目中安装`blogdown`包


```r
install.packages('blogdown')
library(blogdown)
```

-   [hugo](https://themes.gohugo.io/)提供了许多模板,选择喜欢的模板，点击`download`跳转到github

<div class="figure" style="text-align: center">
<img src="fig/build_web/download.png" alt="模板主题" width="100%" />
<p class="caption">Figure 4: 模板主题</p>
</div>

-   复制github地址中的`user/hugoname`即为`theme`,如`https://github.com/wowchemy/starter-hugo-academic`对应的theme为`wowchemy/starter-hugo-academic`，使用theme创建网站


```r
new_site(theme='wowchemy/starter-hugo-academic')
```

## 预览网站

-   使用`serve_site()`预览网站


```r
serve_site()#预览网站
stop_server()#停止预览
```

## 检查与设置


```r
check_gitignore()
check_content()
```

-   根据TODO建议，在右下角`Files`查看当前项目路径中的文件，打开`gitignore`,加入


```gitignore
.DS_Store
Thumbs.db
/public
/resources
```

# 上传R项目到github

-   打开github desktop， `File-add local repo-选择R项目文件夹-commit-fetch`

<div class="figure" style="text-align: center">
<img src="fig/build_web/commit.png" alt="上传项目至github" width="100%" />
<p class="caption">Figure 5: 上传项目至github</p>
</div><div class="figure" style="text-align: center">
<img src="fig/build_web/fetch.png" alt="上传项目至github" width="100%" />
<p class="caption">Figure 6: 上传项目至github</p>
</div>

-   打开github可以看到项目内容已成功上传

<div class="figure" style="text-align: center">
<img src="fig/build_web/update_repo.png" alt="上传项目至github" width="100%" />
<p class="caption">Figure 7: 上传项目至github</p>
</div>

# 使用netlify创建网站

-   用github账号登录[netlify](https://app.netlify.com/signup/start)
-   `Add new site-Import an existing project-Github-deploy`

<div class="figure" style="text-align: center">
<img src="fig/build_web/add_site.png" alt="deploy网站" width="100%" />
<p class="caption">Figure 8: deploy网站</p>
</div>

-   更改`site settings`,`change site name`可以更改网站名

<div class="figure" style="text-align: center">
<img src="fig/build_web/site_settings.png" alt="更改site设置" width="100%" />
<p class="caption">Figure 9: 更改site设置</p>
</div>

-   点击网站名即可跳转到建好的网站

<div class="figure" style="text-align: center">
<img src="fig/build_web/site.png" alt="网站建好啦！" width="100%" />
<p class="caption">Figure 10: 网站建好啦！</p>
</div>

-   复制网站名，打开R studio项目文件，找到`config.yaml`，更改`baseURL`为复制好的网站名

<div class="figure" style="text-align: center">
<img src="fig/build_web/change_URL.png" alt="最后一步也完成啦！" width="100%" />
<p class="caption">Figure 11: 最后一步也完成啦！</p>
</div>

# What's next

-   网站基本搭建之后，就可以使用R studio对网站内容进行个性化设置，具体教程可参见：
    -   [blogdown 教程](https://bookdown.org/yihui/blogdown/)
    -   如果使用的是wowchemy的网站模板，可以参见[wowchemy教程](https://wowchemy.com/docs/getting-started/page-builder/)
-   每次修改之后都需要使用github desktop进行更新，具体操作和之前一样，打开`github desktop-commit-fetch`
-   github的更新会自动关联到Netlify的网站更新
