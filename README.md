# 3-level Cascader

> A Vue.js single-file component

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```
## 依赖模块：vue-awesome
$ npm i --save vue-awesome
"dependencies": { "vue-awesome": "^3.2.0" }

## 输入（props） districts: {Array} [ {Object}, ... ]
``` HTML
<Cascader :districts="[...]" />
```

``` JavaScript
[
  {
    name: '北京',
    children: [
      {
        name: '海淀',
        children: [
          {
            name: '黄庄',
            children: null
          },
          {
            name: '五道口',
            children: null
          }
        ]
      },
      {
        name: '通州',
        children: [
          {
            name: '永顺',
            children: null
          },
          {
            name: '马驹桥',
            children: null
          }
        ]
      }
    ]
  },
  {
    name: '河北',
    children: [
      {
        name: '石家庄',
        children: [
          {
            name: '长安区',
            children: null
          },
          {
            name: '桥西区',
            children: null
          }
        ]
      },
      {
        name: '唐山',
        children: [
          {
            name: '路北',
            children: null
          },
          {
            name: '路南',
            children: null
          }
        ]
      }
    ]
  }
]
```
## 输出：emit事件：'cascader-submit'
事件携带参数 {Array} [ {Object}, ... ]

``` HTML
<Cascader @cascader-submit="{Function} eventHandler" />
```

``` JavaScript
[
  {
    name: '北京',
    children: [
      {
        name: '海淀',
        children: [
          {
            name: '二里庄',
            children: null
          },
          {
            name: '成府路',
            children: null
          }
        ]
      }
    ]
  }
]
```


For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).
