---
title: 语法糖JSX
date: 2019-04-13 11:23:25
tags:
---
# 一、JSX简介
1、JSX就是Javascript和XML结合的一种格式。当遇到<，JSX就当HTML解析，遇到{就当JavaScript解析。
2、ES6是一个新版本的JavaScript，添加了一些不错的语法和功能添加。为了让更多的浏览器支持它，我们需要安装babel编译器。
3、实例：
```
class Header extends React.Component{
  render(){
    return{
      <h1>Hello World</h1>
    }
  }
}
```

# 二、在VUE中使用JSX

### 1、VUE-Cli需要添加依赖包
1：babel-plugin-jsx-v-model
2：babel-plugin-syntax-jsx
3：babel-plugin-transform-vue-jsx
4：babel-plugin-vue-jsx-sync

### 2、在VUE中使用
```
<!-- 创建 -->
export default {
  name: 'SasButton',
  render (h) {
    const categoryMap = {
	    primary: ‘sas-button--primary‘,…
    }
    const sizeMap = {
      default: 'lvx-button--default’,…
    }
    const data = {
      class: [categoryMap[this.category], sizeMap[this.size]],
      attrs: this.$props,
    }
    return (
      <lvx-button class="sas-button" {...data} on-click={this.clickHandle} >
        {this.$slots.default}
      </lvx-button>
    );
  },
  data: function () {…},
  methods: {…},
  props: {
    disabled: Boolean,
    autofocus: Boolean,
  }
}
<!-- 使用 -->
<sas-button category="primary" @click="changeSelect" size="small">双皮奶</sas-button>
```

### 3、在VUE中使用JSX封装组件的好处
1:编写多层级html更加符合前端思路
2:更多的配置项存入json变量里，使用…data添加到模板，使得代码结构清晰。
3:缩短学习react技术栈的门槛