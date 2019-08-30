## Heroku 学习笔记

### 登录 Heroku
heroku login 

### 添加 SSH Key 到 Heroku 上
heroku keys:add

### 创建一个 Heroku App
heroku create

### 配置 Procfile文件
echo web: vendor/bin/heroku-php-apache2 public/ >Procfile

### 声明 buildpack 说明程序为PHP写的
heroku buildpacks:set heroku/php

### 将 Laravel 的 APP_KEY 加入到 heroku 中
heroku config:set APP_KEY=your-key-here
> 需配合 php artisan:generate --show 指令使用

### 对应用改名
heroku rename your-app-name

### 查看 heroku 站点地址
heroku domains

### 输出生产环境上的日志
heroku logs

### 在浏览器打开 heroku 站点
heroku open