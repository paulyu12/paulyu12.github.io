---
layout:     post
title:      "CSS盒子line-height属性使用"
# subtitle:   ""
date:       2024-09-08
author:     "Paul"
header-img: "img/post-bg-2020-12.jpg"
tags:
    - Frontend
---
# line-height 介绍
line-height, 行高。表示文字在盒子中占用到高度。

# 示例
1. box1 有font-size，但是line-height=0;
2. box2 font-size=0，但是有line-height;
3. **说明撑开盒子高度的是line-height，不是font-size;**

<iframe src="https://codesandbox.io/embed/xrjs85?view=editor+%2B+preview"
     style="width:100%; height: 500px; border:0; border-radius: 4px; overflow:hidden;"
     title="css-line-height-demo"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>


# 如果想让文字在盒子中垂直居中，可设置 line-height 等于盒子高度
<iframe src="https://codesandbox.io/embed/lylmyd?view=preview"
     style="width:100%; height: 500px; border:0; border-radius: 4px; overflow:hidden;"
     title="css-line-height-demo2"
     allow="accelerometer; ambient-light-sensor; camera; encrypted-media; geolocation; gyroscope; hid; microphone; midi; payment; usb; vr; xr-spatial-tracking"
     sandbox="allow-forms allow-modals allow-popups allow-presentation allow-same-origin allow-scripts"
   ></iframe>