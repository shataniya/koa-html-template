##### koa-html-template
##### Introduction
- Koa-html-template makes it easy to render html files
- Simple operation and simple syntax
##### Download
```
npm  i koa-html-template
```
##### Instructions
```javascript
const koa = require("koa")
const Router = require("koa-router")
const Static = require("static-resource-plugin")
// 引入 koa-html-template
const template = require("koa-html-template")

const server = new koa()
const router = new Router()

server.use(Static ()) // The default static folder here is the folder where the static resources are stored.
server.use(template()) // If there is no custom setting to store the path to the html file, it will be stored in the static folder of the project root directory by default.
server.use(router.routes()).use(router.allowedMethods())

router.get("/",async ctx=>{
    ctx.body = "Hello World"
})

router.get("/demo",async ctx=>{
	/*
	* @params path   Path to html template file
	* @params data   The data to be rendered by the template
	* ctx.template(path,data)
	*/
    await ctx.template("html/index.html",{
        demo:"this is a demo page..."
    })
})

server.listen(3000,function(){
    console.log("server is running at http://localhost:3000")
})
```
```html
<!-- index.html -->
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>demo</title>
    </head>
    <body>
        <h1>Hello World</h1>
        <h2>{{demo}}</h2>
        <br>
        <img src="demo1.jpg" width="300px" alt="">
    </body>
</html>
```
- Html file after data rendering
```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>demo</title>
    </head>
    <body>
        <h1>Hello World</h1>
        <h2>this is a demo page...</h2>
        <br>
        <img src="demo1.jpg" width="300px" alt="">
    </body>
</html>
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190930090952964.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNjcyMDA4,size_16,color_FFFFFF,t_70)
- You can see {{demo}} rendered into this is a demo page...
##### Basic grammar
##### if statement
```html
<!-- index.html -->
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>demo</title>
    </head>
    <body>
        <h1>Hello World</h1>
        <!-- if statement -->
        {if flag}
        <h1>this is show if statement...</h1>
        {/if}
    </body>
</html>
```
```javascript
// server.js
router.get("/demo",async ctx=>{
	/*
	* @params path   Path to html template file
	* @params data   The data to be rendered by the template
	* ctx.template(path,data)
	*/
    await ctx.template("html/index.html",{
        flag:true // the flag is true
    })
})
```
- the flag is true
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190930091500882.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNjcyMDA4,size_16,color_FFFFFF,t_70)
- if the flag is false
```javascript
router.get("/demo",async ctx=>{
    await ctx.template("html/index.html",{
        flag:false // the flag is false
    })
})
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190930091629423.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNjcyMDA4,size_16,color_FFFFFF,t_70)
- You can see that the dom element has disappeared.
##### for statement
- If you make a for loop on an array
```html
<!-- index.html -->
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>demo</title>
    </head>
    <body>
        <h1>Hello World</h1>
        {for list}
        <p>{$index}==>{$value}</p>
        {/for}
    </body>
</html>
```
```javascript
// server.js
router.get("/demo",async ctx=>{
    await ctx.template("html/index.html",{
        list:[1,2,3,4]
    })
})
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019093009202628.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNjcyMDA4,size_16,color_FFFFFF,t_70)
- If you are doing a for loop on an object
```html
<!-- index.html -->
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>demo</title>
    </head>
    <body>
        <h1>Hello World</h1>
        {for list}
        <p>{$key}==>{$value}</p>
        {/for}
    </body>
</html>
```
```javascript
// server.js
router.get("/demo",async ctx=>{
    await ctx.template("html/index.html",{
        list:{
			name:"zhangsan",
			age:23,
			gender:"man"
		}
    })
})
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190930092407588.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNjcyMDA4,size_16,color_FFFFFF,t_70)
- If you want a loop on an array containing objects
```html
<!-- index.html -->
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>demo</title>
    </head>
    <body>
        <h1>Hello World</h1>
        {for list}
        <p>{$value.id}===>{$value.name}</p>
        {/for}
    </body>
</html>
```
```javascript
// server.js
router.get("/demo",async ctx=>{
    await ctx.template("html/index.html",{
        list:[
            {
                id:1,
                name:"zhangsan"
            },
            {
                id:2,
                name:"lisi"
            },
            {
                id:3,
                name:"wangwu"
            },
            {
                id:4,
                name:"laowang"
            }
        ]
    })
})
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190930092650242.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNjcyMDA4,size_16,color_FFFFFF,t_70)
##### Support for embedding other html templates
- Use {-include "othet html path" } to introduce other html templates
```html
<!-- footer.html -->
<h1>this is a footer...</h1>
<p>{{demo}}</p>
```
```html
<!-- index.html -->
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>demo</title>
    </head>
    <body>
        <h1>Hello World</h1>
        {-include "html/footer.html" }
    </body>
</html>
```
```javascript
// server.js
router.get("/demo",async ctx=>{
    await ctx.template("html/index.html",{
        demo:"this is a demo page..."
    })
})
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190930093326217.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxNjcyMDA4,size_16,color_FFFFFF,t_70)
##### Almost like these