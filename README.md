# puge_clock_container
![Alt text](http://puge-10017157.cossh.myqcloud.com/github8600/clock-container/%E5%BD%95%E5%88%B6_2018_01_29_22_02_51_78.gif)

使用方法 
安装：npm i -save puge_clock_container

引入并注册

```
<template>
  <div class="my-chart">
    <Clock :width="200"></Chart>
  </div>
</template>

<script>
  import Clock from 'puge_clock_container'
  export default {
    components: {
      Clock
    }
  }
</script>
```

|参数名 | 类型 | 必填 | 描述 | 默认值|
|-------|-------|-------|-------|-------|
|width | Number| false | 时钟宽高 | 无|
|spacing | Number| false | 间隙宽度 | 无|
|lineWidth | Number| false | 线宽 | 无|