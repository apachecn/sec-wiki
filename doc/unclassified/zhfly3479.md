# 简介

> 原文：[https://www.zhihuifly.com/t/topic/3479](https://www.zhihuifly.com/t/topic/3479)

### 绕过前端服务器安全控制

在这个网络环境下，前端服务器负责安全控制，只有被允许的请求才能转发给后端服务器，而后端服务器无条件的相信前端服务器转发过来的全部请求，并对每一个请求都进行响应。

在这种情况下可以利用HTTP请求走私，将无法访问的请求走私给后端服务器以获得响应。