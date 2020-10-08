# imglazy

> A Vue.js project

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

# run unit tests
npm run unit

# run e2e tests
npm run e2e

# run all tests
npm test
```

## 备注
<img-lazy :imgList="imgList" :itemHeight="itemHeight"></img-lazy>
- imgList:图片数组
- itemHeight：每行图片的高
- 需要给组件一个高度
