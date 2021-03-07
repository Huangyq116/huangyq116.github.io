---
title: Postman常用设置全局变量的js片段
permalink: 
date: 2019-07-30 16:16:00
categories:
  - 3.Testing
  - 接口
tags:
  - 接口
---

#### postman知识总结

[API自动化利器](http://www.bayescafe.com/tools/use-postman-to-test-api-automatically.html)



#### 1.获取环境变量内容

```js
var ostype = pm.environment.get("ostype");
```



#### 2.设置全局变量内容

```js
postman.setEnvironmentVariable("ts",Math.floor(new Date().getTime()/1000));
```



#### 3.auth签名

```js
var auth = CryptoJS.SHA1(pm.environment.get("device_secret"),{asString: true});
postman.setEnvironmentVariable("auth", auth);
```



#### 4.随机标识

```js
const randomInt = (min, max) => Math.floor(Math.random() * (max - min + 1)) + min;  // 随机整数
const getRandomValue = list => list[randomInt(0, list.length - 1)];  // 随机选项
const chars = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'g', 'k', 'l', 'm', 'n', 'o', '1'];
let identifier = '';
for (let i = 0; i < 5; i++) {
  identifier += getRandomValue(chars);
}
pm.environment.set("identifier", identifier);
```



#### 5.schema校验

```js
let json;
try {
  json = JSON.parse(responseBody);
} catch(err) {
  tests['服务端没返回合法的JSON格式，请检查相关服务、网络或反向代理设置（以下跳过其他断言）'] = false;
  tests[`[INFO] 返回：${responseBody}`] = true;
  console.error(err);
}
if (json) {
  const result = tv4.validateResult(json, schema);
  console.log(result);
  tests['JSON Schema格式正确 ' + result.error ] = result.valid;
  } else {
    console.error(result.error);
    console.error(responseBody);
}
```

