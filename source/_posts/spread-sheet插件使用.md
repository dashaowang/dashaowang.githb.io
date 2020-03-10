---
title: spread-sheet插件使用（操作员中心版）
date: 2020-01-09 14:59:47
tags:
---

## 1、插件安装

在需要使用该组件的模块的 config.js 文件的 plugins 中添加`plugins: ['spread-sheet --registry=http://192.168.102.232:6060/repository/npm-public/']`，项目中运行命令`npm run installChild` 进行依赖安装

## 2、使用

```javascript
<template>
  <div>
   <!-- showVerticalScrollbar  显示垂直滚动条 -->
  <!-- showHorizontalScrollbar 显示水平滚动条 -->
  <!-- newTabVisible 创建新的sheet-->
    <spread-sheet
        ref="spread"
        :showVerticalScrollbar="true"
        :showHorizontalScrollbar="true"
        :newTabVisible="false" >
    </spread-sheet>
  </div>
</template>
<script>
  // 2.1 引入组件
  import spreadSheet from 'spread-sheet';
  import 'spread-sheet/dist/spread-sheet.css';
  export default{
    name:'testSheet',
    components:{spreadSheet}, // 组件注册
    data(){
      return {

      }
    },
    methods:{
    // 解析数据回显excel res----二进制流
      openExcel (res) {
        const reader = new FileReader();
        let bstr;
        try {
          bstr = atob(res); // 后台返回的excel内容base64解密
        } catch (err) {
          // 错误处理
        }
       // 解密内容转成blob格式
        let n = bstr.length;
        const u8arr = new Uint8Array(n);
        while (n--) {
         u8arr[n] = bstr.charCodeAt(n);
        }
        reader.readAsArrayBuffer(new Blob([u8arr]));
        reader.onloadend = () => {
          this.$refs.spread.open(reader.result); // 加载显示excel内容
        };
      }
    }
  }
</script>
```

以上是在具体业务中使用的代码，将二进制流传入 openExcel 方法，在界面显示 excel
