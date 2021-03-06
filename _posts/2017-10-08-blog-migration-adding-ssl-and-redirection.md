---
layout: post
published: true
title: Blog Migration - Adding SSL and Redirection
subtitle: >-
  I added SSL support via CloudFfare and some javascript to redirect old posts
  to the new site.
---
## SSL Support

Taking advice from Adrian, I setup cloudflare in about 30 seconds. Then just waited for the DNS nameserver change to take place and https just works. I did choose to pay the uplift to get a dedicated SSL Certificate it just seemed odd having a multidomain certificate with random other sites.


<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">You might wanna consider using <a href="https://twitter.com/Cloudflare?ref_src=twsrc%5Etfw">@Cloudflare</a> to deal with the HTTPS issues.</p>&mdash; Adrian Png (@fuzziebrain) <a href="https://twitter.com/fuzziebrain/status/916327296872493056?ref_src=twsrc%5Etfw">October 6, 2017</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

# 

![Screen Shot 2017-10-08 at 8.01.06 AM.png]({{site.baseurl}}/img/Screen Shot 2017-10-08 at 8.01.06 AM.png)

## Redirecting 

I'll preface with there's probably a better way but here's what I did.

My blogger site had been up for quite sometime so it shows up in searches sometimes. I didn't want to blanket forward someone to the home page of the new imporived site. Since Jeykyll translated the blogger posts to ne file names, it's a very repeatable operation. Here's some javascript I threw together that grabs the canonical link from the blogger page and converts that to the new link and does a redirect.  For the case of not-a-single-post page ( list, categories ), this javascript just does nothing with out the canonical link present except print a console message ( my left over debugging )



{% highlight javascript %}
window.onload = function () {
 var canonical = "";
var el = document.getElementsByTagName("link");
for (var i = 0; i < el.length; i ++) {
    if (el[i].getAttribute("rel") === "canonical") {
        canonical = el[i].getAttribute("href")
    }
}
var page = canonical.substring(canonical.lastIndexOf('/')+1)
page = page.substring(0,page.indexOf(".html"))
var dt;              
var el = document.getElementsByTagName("abbr");
for (var i = 0; i < el.length; i ++) {
    if (el[i].getAttribute("itemprop") === "datePublished") {
        dt = el[i].getAttribute("title")
    }
}        
var parts = dt.split('-')
var yr = parts[0];
var mon = parts[1];
var d = parts[2].substring(0,parts[2].indexOf('T'));
if ( page ){
var newurl = 'http://krisrice.io/' + yr + '-' + mon + '-' + d + '-' +page + '/'
console.log(newurl)
window.location=newurl;
} else {
 console.log('no page found')
}
}

{% endhighlight %}


## Add Javascript to Blogger

Last thing now is just add this javascript to blogger. I just added a "HTML/JavaScript Gadget" and put it in there and now everything redirects to the new site. Now my old blog is my new blog's biggest reffer in google analytics.

![Screen Shot 2017-10-08 at 8.32.50 AM.png]({{site.baseurl}}/img/Screen Shot 2017-10-08 at 8.32.50 AM.png)

