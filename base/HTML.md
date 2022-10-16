## H5的新特性
1. `<!DOCTYPE>`标签

2. 8个语义化标签
```html
<header>头部</header>
<footer>底部</footer>
<aside>侧边栏</aside>
<main>主体</main>
<article>文章</article>
<nav>导航</nav> 
<section>区域</section> 
<details>详情信息</details>
```

3. 2个媒体化标签
```html
audio  音频标签
video  视频标签
```
|属性       |      描述
| :------- | :-------------------- |
|src       |  视频（音频）的url地址|
|controls  | 显示播放控件|
|autoplay  | 视频（音频）自动播放|
|loop      | 循环播放|
|muted	   | 静音播放|
> 关于autoplay：Chrome的新版本屏蔽了autoplay功能,需要js主动开启

4. 表单的输入类型
- date、month、week、time、datetime、datetime-local
- color、email、tel、url、range、number、search

5. 5个API
- 本地存储：localStorage、sessionStorage
- 画布：canvas
- 地理：Geolocation
- 拖拽释放
- web workers
