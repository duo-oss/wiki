# historyApiFallback

这个设置用于自动将404响应跳转到首页入口文件 index.html。

webpack 配置如下：

```js
module.exports = {
  //...
  devServer: {
    historyApiFallback: true
  }
};
```

部署到服务器上时 nginx 的配置如下：

```js
server {
    listen  80;
    server_name  172.20.47.16;

    location / {
        root /usr/share/nginx/html/console;
        try_files $uri $uri/ /index.html;  # 加上这一行即可
    }

    access_log /var/log/nginx/console_access.log main;
    error_log /var/log/nginx/console_error.log;
}
```
