### Laravel 学习笔记

#### 约定成俗
1. 被@include() 的文件应该用下划线开头，比如_header.blade.php
2. 控制器命名应该是复数形式+Controller
3. 模型命名应该是单数形式
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

#### 模版语法
1. @extends('layouts.default') 继承/resources/views/layouts/default.blade.php
2. @yield('content','default value') 预定义可替代的区块
> 配合@section('@yield的第一个参数名')
>> @section需要配合@stop结束当前@section区块的解析
3. @section('@yield的第一个参数名') 填充@yield('参数')区块的内容
4. @stop结束@section的区块的解析
5. {{ route('somename') }} 用于代替传统的 Url ，批量改动 Url 时，只需要改动路由 web.php 文件即可
> 需配合 Route::get('/path','SomesController@path')->name('somename'); 使用
6. @include('folders.filenamewithout.blade.php') 引入文件，无需写.blade.php

#### 路由
1. Route::get('/path','SomesController@path')->name('somename'); 指定访问 localhost/path 跳转到 SomesController 下的 path 方法，并为此路由定义了一个 somename 的名称