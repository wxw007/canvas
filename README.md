# canvas
canvas合成图片并下载 踩坑

### 外部图片对 canvas 跨域污染问题
如果需要合成的图片素材中,包含外部链接的图片,会导致画布污染,无法调用canvas.toDataUrl()方法,此时不能用img标签直接放入dom, 需要把素材图片先绘制到内部的小canvas中。
```
//插入图片
    function insertWxCode() {
        // 此处图片不能直接用img标签,被跨域图片污染的画布无法导出,必须先把二维码图片画到画布上
        let canvas = document.getElementById("codeCanvas")
        let ctx = canvas.getContext("2d"),
            img = new Image(),
            base64 = '';
        img.src = 'http://xxx.png';
        //允许画布接受跨域图片
        img.setAttribute("crossOrigin", 'Anonymous')
        img.style.width = '420px';
        img.style.height = '420px';
        img.onload = function() {
            ctx.drawImage(img, 0, 0, canvas.width, canvas.height);
            //下载动作必须在二维码图片绘制完成后进行,否则二维码图片为空
            download()
        };
    }
```
### base64太长,会导致超出 a.href的范围,导致点击 a 无法下载
解决办法: 将base64转换为 blob
```
https://www.cnblogs.com/richard1015/p/10349259.html
```
