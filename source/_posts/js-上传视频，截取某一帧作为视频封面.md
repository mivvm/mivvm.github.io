---
layout: 
title: js-上传视频，截取某一帧作为视频封面
date: 2023-12-17 12:57:19
tags: [js]
categories: 
- js
---

工作中在上传视频后，前台页面展示的时候默认需要一张图片，但是没有人去维护，所以就需要从视频中截取一张图片出来。
<!--more-->

```js
/** 将base64转换为blob
 * */
function b64toBlob(base64) {
  var mimeString = base64.split(",")[0].split(":")[1].split(";")[0]; // mime类型
  var byteString = atob(base64.split(",")[1]); // base64 解码
  var arrayBuffer = new ArrayBuffer(byteString.length); // 创建缓冲数组
  var intArray = new Uint8Array(arrayBuffer); // 创建视图

  for (var i = 0; i < byteString.length; i++) {
    intArray[i] = byteString.charCodeAt(i);
  }
  return new Blob([intArray], { type: mimeString });
}

/** 将blob转换为file
 * */
function blobToFile(theBlob, fileName) {
  return new window.File([theBlob], fileName + ".jpg", {
    type: "image/jpg",
    lastModified: Date.now(),
  });
}
function createVideoImg(e) {
  return new Promise((resolve, reject) => {
    const files = e;
    const reader = new FileReader(); // 创建读取文件对象
    reader.readAsDataURL(files); // 发起异步请求，读取文件
    reader.onload = function (t: any) {
      // 文件读取完成后
      // console.log(t.target.result)//视频
      let video = document.createElement("video");
      video.setAttribute("src", t.target.result);
      video.setAttribute("controls", "controls");
      video.setAttribute("width", '708px'); //设置大小，如果不设置，下面的canvas就要按需设置
      video.setAttribute("height", '300px');
      video.currentTime = 7; //视频时长，一定要设置，不然大概率白屏
      video.addEventListener("loadeddata", async function () {
        let canvas = document.createElement("canvas"),
          width = video.width * 2, //canvas的尺寸和图片一样
          height = video.height * 2;
        canvas.width = width; //画布大小，默认为视频宽高
        canvas.height = height;
        canvas.getContext("2d").drawImage(video, 0, 0, width, height); //绘制canvas
        let dataURL = canvas.toDataURL("image/jpg", 2.0); //转换为base64
        let url = b64toBlob(dataURL);
        let file = blobToFile(url, new Date().getTime());
        resolve(file)
      });
    };
  })
}

```
