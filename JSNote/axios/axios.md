---
title: Axios
date: 2020-10-13 22:19:09
---
[TOC]

# *Axios*

## 案例

## 案例

执行 `GET` 请求

```js
// 为给定 ID 的 user 创建请求
axios.get('/user?ID=12345')
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });

// 上面的请求也可以这样做
axios.get('/user', {
    params: {
      ID: 12345
    }
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```

执行 `POST` 请求

```
axios.post('/user', {
    firstName: 'Fred',
    lastName: 'Flintstone'
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```

执行多个并发请求

```
function getUserAccount() {
  return axios.get('/user/12345');
}

function getUserPermissions() {
  return axios.get('/user/12345/permissions');
}

axios.all([getUserAccount(), getUserPermissions()])
  .then(axios.spread(function (acct, perms) {
    // 两个请求现在都执行完成
  }));
```

## axios的全局配置

![1570541353621](imgs\1570541353621.png)

常见配置选项，具体可看官网

![1570541513944](imgs\1570541513944.png)

封装axios

![1570542920896](imgs\1570542920896.png)

![1570542391146](imgs\1570542391146.png)

![1570542483129](imgs\1570542483129.png)



## axios使用封装接口

```js
request({
    url: '/p/getProblemSet/'+index
}).then(
    res =>{
        console.log(res);
    }
).catch(
    err =>{
        console.log(err)
    }
)
```

## Parameters与Data的区别:

实际上就是GET和POST区别;

我们知道GET请求的参数都是在URL上的，服务器并不会读取http body里面的数据,这样我们传递的就是Params里的请求的参数了；如果想让服务器读取http body里面的数据那就需要用POST请求了，POST请求的参数就存放在body；

以后**如果是GET请求最好是在parameters里面设置，而POST请求就在Body Data里面设置**.



## axios接收图片

```js
getVerifyImage() {
    request({
        url: "/verify/image",
        method: "get",
        responseType: "arraybuffer"
    })
        .then(response => {
        //将从后台获取的图片流进行转换
        return (
            "data:image/png;base64," +
            btoa(
                new Uint8Array(response.data).reduce(
                    (data, byte) => data + String.fromCharCode(byte),
                    ""
                )
            )
        );
    })
        .then(res => {
        console.log(res);
        this.verifiedImg = res;
    })
        .catch(err => {
        console.log(err);
    });
}
```

## axios跨域

需要在默认实例添加

```js
withCredentials: true
```

```js
const instance = axios.create({
    baseURL: process.env.VUE_APP_BASE_API,  
    timeout: 5000, // 请求的超时时间
    // `withCredentials` 表示跨域请求时是否需要使用凭证，允许携带cookie
    withCredentials: true, // default false
})
```

https://segmentfault.com/a/1190000011811117

## axios上传文件

[axios官网](http://www.axios-js.com/)