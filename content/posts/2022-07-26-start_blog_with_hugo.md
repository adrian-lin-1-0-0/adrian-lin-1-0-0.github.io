---
title: "Start blog with HUGO"
author: adrian
type: post
url: /2022/07/start_blog_with_hugo/
categories:
  - Golang
  - Blog
tags:
  - golang
  - hugo
date: 2022-07-27T22:17:25+08:00
---


![](https://raw.githubusercontent.com/adrian-lin-1-0-0/blog-img/master/2022/07/27/hugo-main-page.webp)

### 1. Download and Install HUGO

Hugo currently provides pre-built binaries for the following:

- macOS (Darwin) for x64, i386, and ARM architectures
- Windows
- Linux
- OpenBSD
- FreeBSD

You can get the release distribution from [HUGO's github](https://github.com/gohugoio/hugo/releases) 

or using command line :



Snap (Linux)

> The extended version with Sass/SCSS support

```
$ snap install hugo --channel=extended
```

> The non-extended version without Sass/SCSS support

```
$ snap install hugo
```



Homebrew (macOS)

```
$ brew install hugo
```



Chocokatey (Windows)

```
$ choco install hugo -confirm
```



### 2. Create a New Site

Create a HUGO template

> new : Create new content for your site

```
$ hugo new site blog
```

```
$ tree blog
```

You can see the project layout

```
blog
├── archetypes
│   └── default.md
├── config.toml
├── content
├── data
├── layouts
├── public
├── static
└── themes
```

Try to add a theme

> you can find HUGO themes in [`themes.gohugo.io`](https://themes.gohugo.io/ )
>
> e.g. [`hugo-theme-meme` ](https://themes.gohugo.io/themes/hugo-theme-meme/)



```
$ cd blog
/blog $ git init  // if you didn't initialize your repository
/blog $ git submodule add --depth 1 https://github.com/reuixiy/hugo-theme-meme.git themes/meme
```

Replace `config.toml`with `hugo-theme-meme` config example

```
/blog $  rm config.toml && cp themes/meme/config-examples/en/config.toml config.toml
```

Create a new post and the about page

```
/blog $ hugo new "posts/hello-world.md"
/blog $ hugo new "about/_index.md"
```

Run the hugo server

>   -D, --buildDrafts  : include content marked as draft

```
/blog $ hugo server -D
```


Web Server is available at http://localhost:1313/ 


![](https://raw.githubusercontent.com/adrian-lin-1-0-0/blog-img/master/2022/07/27/hugo-theme-meme-hello.webp)

Let's type something in `/blog/content/posts/hello.world`

![](https://raw.githubusercontent.com/adrian-lin-1-0-0/blog-img/master/2022/07/27/hugo-hello-world-with-Hamlet.webp)

After saving it, Hugo will rebuild the site

![](https://raw.githubusercontent.com/adrian-lin-1-0-0/blog-img/master/2022/07/27/hugo-hello-world-example.webp)