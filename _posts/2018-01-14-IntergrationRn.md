---
layout: post
title: Jekyll使用教程草稿
publised: false
categroy: [jekyll, test]
---

- # Jekyll网站目录
```.
├── _config.yml //配置信息
├── _drafts //草稿
|   ├── begin-with-the-crazy-ideas.textile
|   └── on-simplicity-in-technology.markdown
├── _includes
|   ├── footer.html
|   └── header.html
├── _layouts
|   ├── default.html
|   └── post.html
├── _posts //博客
|   ├── 2007-10-29-why-every-programmer-should-play-nethack.textile
|   └── 2009-04-26-barcamp-boston-4-roundup.textile
├── _site //生成站点内容
├── .jekyll-metadata
└── index.html //首页
```

## _drafts
通过
    >jekyll server --draft
启动，可以将草稿箱中的draft进行预览
## _includes
组件重用
## _layouts
布局模板
## _posts
文章
## _data
网站数据，例如members.json，可以site.data.members访问
## _site
转换后的网站内容

<h4>Category</h4>
<ul>
    //这里使用了 jekyll 语法，会被编译，所以加多个"\"
    {\% for category in site.categories %\}
    <li><a href="/categories/{\{ category | first }\}/" title="view all
posts">{\{ category | first }\} {\{ category | last | size }\}</a>
    </li>
    {\% endfor %\}
</ul>
