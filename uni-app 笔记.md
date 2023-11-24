# uni-app 开发笔记



## 功能实现



##### 拉起地图APP列表，选择并导航

```js
const mapCtx = uni.createMapContext('mapId')
if (mapCtx?.openMapApp) {
  mapCtx.openMapApp({
    longitude,
    latitude,
    destination,
    success: () => {},
    fail: () => {},
    complete: () => {}
  })
}
```

参考自：[uni-app doc - api - location - map # `uni.createMapContext(mapId,this)`](https://uniapp.dcloud.net.cn/api/location/map.html#createmapcontext)