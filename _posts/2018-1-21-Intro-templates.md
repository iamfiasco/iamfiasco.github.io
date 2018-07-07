---
layout: post
title: Delicate touch of web templating !
---
# Prologue

During my school days I used to wonder how does facebook generate pages for every user, posts, chatroom and etc. At that time I knew that nobody is dumb enough to manually write html pages for every users,post and etc. I thought these dynamic content was resultant of pure string concatenation. My thoughts were as follows.

```python
a="""<html>
        <head>
            <title> {} </title>
        </head>
        <body>
            {}
        </body>
    </html>"""
rendered_a = a.format("Title Here","Body Content")

```
Here we have a variable __a__ which contains the placeholders for values. In one placeholder we place title and in another one we place the content. A Kid from 9th standard figured out a way
to generate dynamic content. Things do work in the same way (Sorta). We have some scripts which transforms strings and we have placeholders where we add dynamic content. The industry calls these strings as __templates__ and dynamic content which are placed in the placeholders are called __Locals__. It took me 4 years to realize that Templating is done to achieve this. I skimmed through
cornucopia of documents available in the internet for the best templating engine. I tried a few of them which made me pleased, they were ejs, Pugjs (formerly Jade) and mustache.js. Express.js was the first Web Framework which I experimented with. If you haven't tried it and you are in the web developement business or are trying to get into it then you must try it. It has a simple API and 
its powerful and fast, thanks to Node.js for that. Express.js is somewhat similar to Flask in the ease of use and they are both unopinionated i.e. they dont force you to use specific modules for your workflow. You have complete freedom of choosing what to use as template engine , session management and etc. 

## Pugjs vs ejs vs Mustache.js

I personally like Pugjs since it has pythonic way of writing code so instead of using opening and closing tags you just use indentation so you wont have to get frustrated on __< or >__ keypresses.
a simple html page with Hello world on it is like

```javascript
doctype html
head
    title titlea
    meta(charset="UTF-8")
    link(rel="stylesheet" type="text/css")
body
    h1 content
```
here titlea="Title Here" and contenta="Content Here" and these variables are called locals and need to be passed to the template engine with the template to convert it to plain HTML.

But the code is so pretty. 

I'll tell you about side-effects of abstraction. Highly abstracted code like the above will require to be converted to low level code in this case base HTML because browsers dont understand Pugjs or any tmeplate code. Higher the abstraction or easier the code is for humans higher will be the time of conversion and henceforth will be slower. So yeah, Pugjs is slow and is not suitable for large scale deployement, but its okay for your personal use.

EJS on the other hand uses

```javascript
<html>
    <head>
        <meta charset="utf-8">
        <title><%= titlea %></title>
    </head>
    <body>
        <h1><%= contenta %></h1>
    </body>
</html>
```

This code looks similar to HTML and for regular people might not look so lucid but believe me it is faster. Since there's nothing much to change for HTML generation. The engine just needs to replace <% %> with the locals. In pugjs the engine needs to generate tags which needs bit more effort.

Mustache.js is similar to EJS but for locals it uses {{ local_here }} for locals. It eliminates use of __< >__ so it looks better. Mustache has many  implementations so you may find it for Ruby on Rails or other Web Frameworks. You may also find some other template engines like jinja, mako or etc. 

Apart from locals templates offer partials, interpolations and etc. These things tend to make you more productive in your work.

### My opinions.
When you are into something new and need to quickly learn about the domain and are unsure about the tools you might need to learn. Just do a simple web search and select the tools which you find impressive. Try them out and eventually you'll know what to use in which case. Sometimes its  better to learn multiple tools just to find the best one and to master them.
In my college days I juggled across different web frameworks Hapi.js, Express.js, Flask and Django. Since I am not that comfortable in working with regular expressions I skipped learning Django. Flask and Express.js are still in my journey. Working in few  mini projects  I realized that Express.js or more specifically Node.js(Express.js is a Node.js Framework ) is  very fast since it has asynchronocity out of the box and  handles requests really quickly but tends to lose the war against other frameworks on high IO load. Node.js is single threaded and offcourse you can spawn multiple threads manually but it tends to get in the way. Ruby on Rails on the other hand beats Node.js in this war since it can handle high IO load. Github uses RoR and Jekyll is based on it. It happens to be that this blog is itself created on Jekyll (RoR based).Flask is written in python and does a great job. It handles request in a synchronous way but  you may install 3rd party modules to deal with the situation and you might also use gunicorn to accelatate the processing. The whole point of this paragraph is to there's no one size that fits all, there are different tools to handle different problems.

Hope you enjoyed the post.
If you find errors in the post please do mail the corrections at [stormivofficial@gmail.com](mailto:stormivofficial@gmail.com)
