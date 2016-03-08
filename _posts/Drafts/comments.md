---
title: Disqus Comments in Github Jekyll
layout: post
comments: true
---

I have added [Disqus](https://disqus.com/) comment support to the blog section.  Key steps:

1. Sign up at [Disqus](https://disqus.com/).  Add your website and note the unique shortname.

2. Update _layouts/post.html
- My website was built on [Jekyll Now](https://github.com/barryclark/jekyll-now). You will need to add to your code before the closing tag:

```
{% if page.comments %}
{% include disqus.html %}
{% endif %}
```

3. Edit the _includes/disqus.html such that you put your shortname, and remove the comments ({% if site.disqus %}{% endif %}) so that it will include.

4. In the front matter at beginning of each blog post, add "comments: true" to enable comments section for that po