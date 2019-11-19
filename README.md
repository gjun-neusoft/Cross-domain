# 几种前端跨域解决方案
跨域：浏览器同源策略

一、JSONP(Json With Padding)，前端+后端

原理：前端动态添加script到当前页面，其src指向接口URL，服务器返回在一个函数执行语句，该函数名由callback参数决定，拿到数据后，动态删除script标签

实现：

前端 ：指定请求方式为：jsonp，dataType: jsonp

后端：res.jsonp(data)

接口必须 是get方法，传递参数只能通过url的方式

二、proxy代理，后端 方案

原理：通过同源服务器发送请求至接口服务器，再由同源服务器转发结 果给前端 ，从而规避跨域

webpack-dev-server

proxy

三、cors，后端 方案

原理：cors是w3c规范，真正意义上解决跨域问题。他需要服务器对请求检查 ，对响应头做特殊处理，从而解决跨域

实现：

1.对于简单请求，添加：res.set('Access-controlAllow-Origin','http://localhost:8080')

请求方法是get、post、header同时对于post请求，不能有自定义请求头， 同时编码方式必须 是application/x-www-urlencoded,multipart/form-data,text/plain之一

2.对于预检测preflight请求

回和服务器通话两次。一次是预检测（把请求条件用option发送服务器，服务端检测是否满足请求条件，满足则返回成功状态），一次是真正的请求。

(1) 预检测路由router.options()

(2)响应头添加res.set('Access-Control-Allow-Headers', 'Content-Type');

(3)如果请求方法是put或delete，添加res.set('Access-Control-Allow-Methods','GET,POST,PUT')

```javascript
//第一次请求，判断 
router.options('/cors', (req, res) => {
    //..这里可以加判断 ，是否允许请求http://localhost:8080,如果 允许，则写下面的代码
    res.set('Access-Control-Allow-Origin', 'http://localhost:8080');
    ret.set('Access-Control-Allow-Headers', 'Content-Type');
    ret.set('Access-Control-Allow-Methodsd', 'GET,POST,PUT');
    res.sendStatus(204)
})
//第二次请求，返回数据
router.post('/cors', (req, res)=>{
    user.push(req.body)
    res.set('Access-Control-Allow-Origin', 'http://localhost:8080')
    res.json(users)
})
```

3.对于证书类型Credent

实现

前端 ：

ajax参数添加  xhrFields: {withCredentials: true}  携带cookie

后端：

res.set('Access-Control-Allow-Credentials','true')

4.cors模块 npm i cors -S

app.use(cors())，门户大开，所有请求都允许，除了证书credent

允许证书，如下设置

```javascript
const cors = require('cors')
app.use(cors({
    origin: 'http://localhost: 8080',
    credentials: true
}))
```





