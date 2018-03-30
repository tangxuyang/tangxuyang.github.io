---
layout: post
comments: true
---
一直以来，只会用fiddler抓包，今天学习了一招。在fiddler左下角的这个位置点击一下就可以加断点了，点击一次是在请求前加断点，点击两次是请求返回后加断点。我用的是点击两次，这样就可以修改返回来的数据达到调试的效果了！  

![开启调试](/images/2017-12-20_163131.png)
请求前断点  
![请求前断点](/images/2017-12-20_174451.png)
请求后断点  
![请求后断点](/images/2017-12-20_174511.png)

{% if page.comments %}
<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/
/*
var disqus_config = function () {
this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};
*/
(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');
s.src = 'https://txyzqc.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>
                            

{% endif %}
