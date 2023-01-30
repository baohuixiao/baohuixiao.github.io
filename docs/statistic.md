# 埋点监控

### 数据发送

1. 利用图片src属性发送统计数据，原因有两点

   * 没有跨域限制
   * 兼容性好

   这个图片不是用来展示的，目的只是传递数据，借助img标签的src属性，在其url后面拼接上参数，服务端收到后再去解析

   通常请求的url会是一张1✖️1px的gif图片

   1. 同样大小，不同格式的图片中gif大小是最小的
   2. 如果饭回204，会走到img的onerror事件，并抛出一个全局错误

2. 更优雅的web beacon

   这种打点标记的方式被称为web beacon（网络信标），浏览器逐渐实现专门的API来更优雅地完成这件事：Navigator.sendBeacon，但由于浏览器兼容性的问题，还需要图片的src兜底