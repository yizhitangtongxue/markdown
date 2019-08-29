### Heroku 学习笔记

#### heroku login 
 登录 Heroku
---
#### heroku keys:add
 添加 SSH Key 到 Heroku 上
---
#### heroku create
 创建一个 Heroku App
---
#### echo web: vendor/bin/heroku-php-apache2 public/ >Procfile
 配置 Procfile文件
---
#### heroku buildpacks:set heroku/php
 声明 buildpack 说明程序为PHP写的
---
#### heroku rename your-app-name
 对应用改名