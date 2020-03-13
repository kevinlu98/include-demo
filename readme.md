# 静态页面实现include

## 需求引入
>有时我们在开发纯静态页面或者前后端分离的静态页面时会遇到这样的问题，像导航栏或者网站的footer通常都是一样的，但每个页面都有，所以就有了如下的解决方案

## 案例
>如下页面，我们不希望nav部分和footer出现在每个页面，想将其抽取出来

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="css/typo.css">
    <link rel="stylesheet" href="css/style.css">
    <script src="https://cdn.bootcss.com/jquery/3.4.1/jquery.min.js"></script>
</head>
<body>

<div class="lw-header">
    <nav class="lw-content">
        <ul>
            <li><a href="./index.html">首页</a></li>
            <li><a href="./page1.html">页面1</a></li>
            <li><a href="./page2.html">页面2</a></li>
            <li><a href="./page3.html">页面3</a></li>
        </ul>
    </nav>
</div>
<div class="lw-content lw-body" style="margin-top: 30px;">
    <h1>中文网页重设与排版：<i class="serif">Typo.css</i></h1><br/>

    <h2 id="tagline" class="serif">一致化浏览器排版效果，构建最适合中文阅读的网页排版</h2>

    <ol id="table">
        <li><a href="#section1">关于 <i class="serif">Typo.css</i></a></li>
        <li><a href="#section2">排版实例</a>
            <ul>
                <li><a href="#section2-1">例1：论语学而篇第一</a></li>
                <li><a href="#section2-2">例2：英文排版</a></li>
            </ul>
        </li>
        <li><a href="#section3">附录</a>
            <ul>
                <li><a href="#appendix1"><i class="serif">Typo.css</i> 排版偏重点</a></li>
                <li><a href="#appendix2">开源许可</a></li>
            </ul>
        </li>
    </ol>
</div>

<div class="lw-footer">
    <footer class="lw-content" style="padding-top: 30px;">
        <p>&copy; 2019-2020 <a href="http://www.kevinlu98.cn/">冷文博客-冷文学习者</a></p>
        <p style="margin: 10px 0;">本站资源全部来自互联网收集，仅供用于学习和交流，请勿用于商业用途。如有侵权、不妥之处，请联系站长并出示版权证明以便删除。联系邮箱：kevinlu98@qq.com 或
            QQ 1628048198</p>
        <p><a href="http://www.kevinlu98.cn" target="_blank">陕ICP备19024566号</a> &nbsp; </p>
    </footer>
</div>
</body>
</html>
```

>访问一下

[![https://gitee.com/kevinlu98/imgbed/raw/master/20200313/dbfd6dea-c446-4299-96f4-2d20a0d67295.png](https://gitee.com/kevinlu98/imgbed/raw/master/20200313/dbfd6dea-c446-4299-96f4-2d20a0d67295.png)](https://gitee.com/kevinlu98/imgbed/raw/master/20200313/dbfd6dea-c446-4299-96f4-2d20a0d67295.png)

## 分析
>其实想要实现我们的需求也非常简单，我们只需要在页面加载的时候发送ajax请求，去请求公共页面，然后将返回结果放入指定的div里面就可以了

## 实现
>先看下目录结构


[![https://gitee.com/kevinlu98/imgbed/raw/master/20200313/0e85c875-4a23-437e-a52c-3d9d94b43f81.png](https://gitee.com/kevinlu98/imgbed/raw/master/20200313/0e85c875-4a23-437e-a52c-3d9d94b43f81.png)](https://gitee.com/kevinlu98/imgbed/raw/master/20200313/0e85c875-4a23-437e-a52c-3d9d94b43f81.png)
>将导航栏抽取出来放入`_header.html`页面

```html
<!--_header.html-->
<nav class="lw-content">
    <ul>
        <li><a href="./index.html">首页</a></li>
        <li><a href="./page1.html">页面1</a></li>
        <li><a href="./page2.html">页面2</a></li>
        <li><a href="./page3.html">页面3</a></li>
    </ul>
</nav>
```
>此时原来页面的导航栏部分

[![https://gitee.com/kevinlu98/imgbed/raw/master/20200313/c49b3691-dc47-4051-b944-7751abcf8200.png](https://gitee.com/kevinlu98/imgbed/raw/master/20200313/c49b3691-dc47-4051-b944-7751abcf8200.png)](https://gitee.com/kevinlu98/imgbed/raw/master/20200313/c49b3691-dc47-4051-b944-7751abcf8200.png)

>抽取footer部分到`_footer.html`页面
```html
<!--_footer.html-->
<footer class="lw-content" style="padding-top: 30px;">
    <p>&copy; 2019-2020 <a href="http://www.kevinlu98.cn/">冷文博客-冷文学习者</a></p>
    <p style="margin: 10px 0;">本站资源全部来自互联网收集，仅供用于学习和交流，请勿用于商业用途。如有侵权、不妥之处，请联系站长并出示版权证明以便删除。联系邮箱：kevinlu98@qq.com 或
        QQ 1628048198</p>
    <p><a href="http://www.kevinlu98.cn" target="_blank">陕ICP备19024566号</a> &nbsp; </p>
</footer>
```

>抽取之后的footer


[![https://gitee.com/kevinlu98/imgbed/raw/master/20200313/0a65ea3a-7dd0-468d-b7d3-619cd507be28.png](https://gitee.com/kevinlu98/imgbed/raw/master/20200313/0a65ea3a-7dd0-468d-b7d3-619cd507be28.png)](https://gitee.com/kevinlu98/imgbed/raw/master/20200313/0a65ea3a-7dd0-468d-b7d3-619cd507be28.png)

>发送ajax的方式有很多，我们这里采用jQuery，并且jQuery为我们提供了load方法，可以直接将ajax返回结果以HTML的方式渲染到页面，我们在页面底部加上如下代码

```html
<script>
    $(function () {
        $("#header").load("./common/_header.html")
        $("#footer").load("./common/_footer.html")
    })
</script>
```

>我们访问试试

[![https://gitee.com/kevinlu98/imgbed/raw/master/20200313/32b3f8ea-6d38-4bad-a49c-957d30765c3e.png](https://gitee.com/kevinlu98/imgbed/raw/master/20200313/32b3f8ea-6d38-4bad-a49c-957d30765c3e.png)](https://gitee.com/kevinlu98/imgbed/raw/master/20200313/32b3f8ea-6d38-4bad-a49c-957d30765c3e.png)

## 代码下载
- GitHub: [https://github.com/kevinlu98/include-demo.git](https://github.com/kevinlu98/include-demo.git)
- 码云: [https://gitee.com/kevinlu98/include-demo.git](https://gitee.com/kevinlu98/include-demo.git)