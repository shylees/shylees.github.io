---
title: 记录 windicss 使用过程中遇到的属性
data: 2022/5/12 15:46
updata: 2022/5/18 11:48
tag:
  - vite
  - vue3
  - windicss
category:
  - workNotes
---

1. 常规使用

   **文字 背景**

   - font-size --- text-[12px]
   - font-weight --- font-[500]
   - line-height --- leading-[14px]
   - color --- text-[#5E6673]
   - background --- bg-[#1A2437]
   - opacity --- opacity-60
   - 文本溢出 --- line-clamp-3

   **盒子模型**

   - padding --- px(横向) py(纵向) pt pb pl pr
   - margin
   - width --- w
   - height --- h
   - 圆角边框 --- rounded-[4px]

   **定位**

   - position --- relative/absolute
   - display --- block/flex
   - flex --- flex-col/justify-between/flex-grow-0/items-center
   - z-index --- z-index-1

- first-of-type:mt-0

2. 使用 backgroundImage 做渐变失效的解决方案

   1. 在 `windi.config.js` 文件中添加

   ```js
     extend: {
         // ...
         backgroundImage: {
           "gradient-61":
             "linear-gradient(184.29deg, rgba(0, 0, 0, 0) 3.48%, rgba(0, 0, 0, 0.8) 96.51%)",
         },
       },
   ```

   2. 在 vue 中使用

   ```vue
   <div class="bg-gradient-61" >
   ```
