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

### 对应用改名
heroku rename your-app-name