---
layout : post
tltle : wordpress迁移到jekyll+github
tags : github 
---

##为什么是jekyll + github
用git像管理代码一样管理文章是不是很酷？github是能在网上实现这个的一个最好的选择，它支持gh-pages，不限流量，没有比github更好的服务了。
##安装jekyll

```bash
gem install jekyll
jekyll new blog
cd blog
jekyll serve 
```

##导出旧的blog数据
jekyll默认的文章文件格式是`年-月-日-title.md`，文章的开头需要指明一些文章的数据。

```yaml
---
layout : post
title : 这里写标题
tags : 这里写tag 可以空格 分隔
---

```

##标签的生成
jekyll还是数据组织还是相当不错的，可以通过下面的方式获取所有的标签及它们的数量，在需要展示的地方放上这段，这里用到了[Liquid](https://github.com/Shopify/liquid)的一个标准[过滤器](https://github.com/shopify/liquid/wiki/liquid-for-designers#standard-filters)。

```html
{% raw %}
{% for tag in site.tags %}
<a class="tag" href="/tag.html#{{tag[0]}}">{{tag[0]}}<span class="tagcount">{{tag[1] | size}}</span></a>
{% endfor %}
{% endraw %}
```

##获取相关日志
相关日志jekyll默认就支持，可以通过site.related_posts来获取，默认情况下，这个并不会太精确，如果需要很精确的相关，需要开启`_config.yml`里的选项`lsi:true`。

```html
{% raw %}
{% for post in site.related_posts limit:5 %}
    <div class="article">
        <span class="datetime">{{ post.date | date:"%Y-%m-%d"}}</span>
        <a href="{{ post.url }}">{{ post.title }}</a>
    </div>
{% endfor %}
{% endraw %}
```

##按年份输出列表
这是一种我喜欢的方式，有点像归档，可以用下面的代码搞定

```html
{% raw %}
{% for post in site.posts %}
   {% unless post.next %}
     <h1>{{ post.date | date: '%Y' }}</h1>
   {% else %}
     {% capture year %}{{ post.date | date: '%Y' }}{% endcapture %}
     {% capture nyear %}{{ post.next.date | date: '%Y' }}{% endcapture %}
     {% if year != nyear %}
       <h1>{{ post.date | date: '%Y' }}</h1>
     {% endif %}
   {% endunless %}
   <div class="article">
       <span class="datetime">{{ post.date | date:"%m-%d" }} </span>
       <a href="{{ post.url }}">{{ post.title }}</a>
   </div>
{% endfor %}
{% endraw %}
```


##github上的设置
创建仓库，<username>.github.com, 建立一个这样的仓库，可以在master上提交一个jekyll的站点后，github自动帮你生成所有的静态文件。

