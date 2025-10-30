---
title: DOM动画
layout: post
categories: JavaScript
tags: 前端
date: 2021-07-13 16:00:00
---
# 16：DOM动画

# 缓冲运动的封装
```javascript
function move(ele, obj, cb){
  clearInterval(ele.t)
  ele.t = setInterval(()=>{
    let state = true;
    for(let key in obj){
      let now = parseInt(getStyle(ele, key));
      let speed = (obj[key] - now) / 10;
      speed = speed < 0 ? Math.floor(speed) : Math.ceil(speed);
      ele.style[key] = now + speed + "px";
      if(now !== obj[key]) state = false;
    }
    if(state){
      clearInterval(ele.t);
      cb && cb();
    }
  }, 30);
}
function getStyle(ele, attr){
  if(ele.currentStyle){
    return ele.currentStyle[attr];
  }else{
    return getComputedStyle(ele)[attr];
  }
}
```

# 运动案例：轮播图
# 第三方轮播图：Swiper