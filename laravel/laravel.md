### Laravel 学习笔记

#### Env方法
1. getenv('APP_ENV')获得.env配置文件中的APP_ENV对应的值

#### Artisan指令
1. php -artisan:generate --show 生成APP_KEY 并显示APP_KEY
2. php artisan key:generate 生成 APP_KEY
3. php artisan make:controller	生成控制器
4. php artisan make:model 生成模型
5. php artisan make:policy 生成授权策略
6. php artisan make:seeder	生成 Seeder 文件
7. php artisan migrate	执行迁移
8. php artisan migrate:rollback 回滚迁移
9. php artisan migrate:refresh	重置数据库
10. php artisan db:seed	填充数据库
11. php artisan tinker 进入 tinker 环境
12. php artisan route:list 查看路由列表
13. php artisan list 查看所有可用的Artisan命令
14. php artisan help xxx 查看以上各个命令的帮助