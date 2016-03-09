---
title: Disqus Comments in Github Jekyll
layout: post
comments: true
---

My website is built on [Jekyll Now](https://github.com/barryclark/jekyll-now) through Github. It is already setup to include [Disqus](https://disqus.com/) comments in posts. Add comments to your blog posts with with a few steps.

1. Sign up at [Disqus](https://disqus.com/).  Add your website and note the unique shortname.
2. Edit the _config.yml file such that you add your disqus shortname where prompted.
3. Profit.


Optionally, alter _layouts/post.html slightly so that it will only allow comments to posts you specify.  Specifically, adjust these lines toward the end:

~~~
{% raw  %}
	{% if page.comments %}  
	{% include disqus.html %}  
	{% endif %}  
{% endraw  %}
~~~

Then, in the front matter at beginning of each blog post, add "comments: true" to enable comments section for that post.
