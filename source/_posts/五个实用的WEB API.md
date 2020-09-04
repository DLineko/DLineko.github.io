---
title: 五个实用的WEB API
tags:
  - 前端
  - WEB API
abbrlink: 281cbd2d
date: 2020-09-03 23:44:43
---
- Fullscreen API<br>
- Clipboard Async API<br>
- Resize Observer API<br>
- Image Capture API<br>
- Broadcast Channel API<br>
<!-- more -->

## 1.Fullscreen API

实现浏览器全屏效果

{% asset_img 1599112470994.png  %}
FireFox自动为该节点增加Css规则: width:100%;height:100%

Chrome将该节点放在屏幕中央，保持原来大小，其余变黑。

```css
:-webkit-full-screen #myvideo {
  width: 100%;
  height: 100%;
}
```

语法：Element.requestFullscreen(options);

```javascript
const manageFullscreen = () => {    document.getElementById('fs_id').requestFullscreen();
```



## 2.Clipboard Async API

实现剪切、复制和粘贴功能

{% asset_img 1599114657119.png  %}
以往使用`document.execCommand`操作剪切板，这种方法是同步操作，只能读取和写入DOM。Clipboard API是基于promise的API，旨在解决这个问题。

```javascript
async function performCopy(event) {
   event.preventDefault();
   try {
      await navigator.clipboard.writeText(copyText);
      console.log(`${copyText} copied to clipboard`);
   } catch (err) {
      console.error('Failed to copy: ', err);
   }
}
async function performPaste(event) {
   event.preventDefault();
   try {
       const text = await navigator.clipboard.readText();
       setPastetext(text);
       console.log('Pasted content: ', text);
   } catch (err) {
      console.error('Failed to read clipboard contents: ', err);
   }
}
```

## 3.Resize Observer API

检测DOM元素尺寸的变化

{% asset_img 1599122262248.png  %}
```javascript
 let elWrapper = document.querySelector(`.domWrapper`)
 let vm = this
 if (window.hasOwnProperty('ResizeObserver')) {
     const resizeObserver = new ResizeObserver(entries => {
         let eTarget = entries[0]
         if (eTarget) {
             const dimensions = entry.contentRect;
             // 实现代码
         }
     })
     resizeObserver.observe(elWrapper)
 }
```

## 4.Image Capture API

从视频设备（例如网络摄像头）捕获图像或抓取帧

{% asset_img 1599122972056.png  %}
```javascript
// 从设备中提取出mediaStream，再创建一个ImageCapture对象
navigator.mediaDevices.getUserMedia({video: true})
  .then(mediaStream => {
    document.querySelector('video').srcObject = mediaStream;

    const track = mediaStream.getVideoTracks()[0];
    imageCapture = new ImageCapture(track);
  })
  .catch(error => console.log(error));

//实现拍摄实时视频快照，返回imageBitmap
 imageCapture.grabFrame()
  .then(imageBitmap => {
    const canvas = document.querySelector('#grabFrameCanvas');
    drawCanvas(canvas, imageBitmap);
  })
  .catch(error => console.log(error));

//获取照片
 imageCapture.takePhoto()
  .then(blob => createImageBitmap(blob)) //返回promise，裁剪图片（坑，很多浏览器不支持）
  .then(imageBitmap => {
    const canvas = document.querySelector('#takePhotoCanvas');
    drawCanvas(canvas, imageBitmap);
  })
  .catch(error => console.log(error));


function drawCanvas(canvas, img) {
  canvas.width = getComputedStyle(canvas).width.split('px')[0];
  canvas.height = getComputedStyle(canvas).height.split('px')[0];
  let ratio  = Math.min(canvas.width / img.width, canvas.height / img.height);
  let x = (canvas.width - img.width * ratio) / 2;
  let y = (canvas.height - img.height * ratio) / 2;
  canvas.getContext('2d').clearRect(0, 0, canvas.width, canvas.height);
  canvas.getContext('2d').drawImage(img, 0, 0, img.width, img.height,
      x, y, img.width * ratio, img.height * ratio);
}

```

## 5.Broadcast Channel API

允许**同源**的窗口，标签，iframe之间进行通信

{% asset_img 1599124775576.png  %}
```javascript
// 创建广播频道
 const CHANNEL_NAME = "greenroots_channel";
 const bc = new BroadcastChannel(CHANNEL_NAME);
 const message = 'I am wonderful!';

// postMessage()发送消息
const sendMessage = () => {
   bc.postMessage(message);
}
 const CHANNEL_NAME = "greenroots_channel";
 const bc = new BroadcastChannel(CHANNEL_NAME);

//接受信息
 bc.addEventListener('message', function(event) {
    console.log(`Received message, "${event.data}", on the channel, "${CHANNEL_NAME}"`);
    const output = document.getElementById('msg');
    output.innerText = event.data;
 });
```



**参考文章**

[10 lesser-known Web APIs you may want to use](https://blog.greenroots.info/10-lesser-known-web-apis-you-may-want-to-use-ckejv75cr012y70s158n85yhn) 
[Fullscreen API：全屏操作](https://javascript.ruanyifeng.com/htmlapi/fullscreen.html#toc1)
[Accessing the Clipboard in JavaScript Using the Async Clipboard API](https://alligator.io/js/async-clipboard-api/)
[ImageCapture API](https://developer.mozilla.org/en-US/docs/Web/API/ImageCapture)
