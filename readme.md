## SBAdmin使用文档
* 前言
* 依赖环境
* 下载
* 配置
* 安装
* 开始
* 开发用户登录

### 前言
本系统使用Laravel([文档](https://laravel.com/docs/)/[中文文档](https://laravel-china.org/docs/home))，以及Bootstrap([文档](http://getbootstrap.com/)/[中文文档](http://v3.bootcss.com/))开发。

### 依赖环境
* [nginx](https://nginx.org/en/)或[Apache](https://httpd.apache.org/).
  * OSX自带apache，亦可用brew安装。
  * Windows用户推荐使用[WAMP](http://www.wampserver.com/en/)，请先自行安装[VC++可再发行组件包](http://support.microsoft.com/kb/2019667)（认准 **Microsoft Visual C++ Redistributable Packages**字样）。亦可使用[XAMPP](https://www.apachefriends.org/zh_cn/index.html)。请注意PHP版本。
  * Linux用户可以自行使用`apt-get`/`yum`等安装。
  * 也可以不要服务器，`laravel`自带一个用php实现的简单服务器[server.php](https://coding.net/u/skys215/p/sbadmin/git/blob/master/server.php)
* PHP5.5+, [MacOS下非brew安装方法](http://php-osx.liip.ch/)
* [composer](https://getcomposer.org/),[composer国内镜像](http://pkg.phpcomposer.com/)
* [npm](https://nodejs.org/)(nodejs) [npm国内镜像](http://npm.taobao.org/)

### 下载
下载代码：`git clone https://git.coding.net/skys215/sbadmin.git`
### 配置
1. 配置配虚拟主机。<br />
   需要将根路径指向`public/`。默认文件需为`index.php`。<br />
   可根据需要隐藏`index.php`项目内已经包含[.htaccess](https://coding.net/u/skys215/p/sbadmin/git/blob/master/public/.htaccess)隐藏index.php。<br />
   Apache：在配置文件中添加`<VirtualHost *:80>`及其相关配置。需要开启`mod_rewrite`模块。设置`AllowOverride all`。<br />
   Nginx：在配置文件中添加`server{...}`及其相关配置。
2. 修改hosts文件<br />
   Windows: `C:\Windows\system32\drivers\etc\hosts`<br />
   Linux && Mac: `/etc/hosts`<br />
   添加`127.0.0.1 sbadmin.dev`，请根据自身情况修改值。

### 安装
* 进入项目目录：`cd sbadmin`
* 给特定文件夹赋予权限：`sudo chmod -R 777 bootstrap/ storage/`
* 安装composer包：`composer install`，可带`-vvv`参数查看进度。建议修改为国内源，本项目中[已设置](https://coding.net/u/skys215/p/sbadmin/git/blob/5.3/composer.json#L51)。
* 创建`.env`文件：`cp .env.example .env`。按照具体情况修改文件内的值。
* 生成密钥：`php artisan key:generate`
* 目前写后台的前端时，没有使用任何工具，不做前端开发可以忽略一下操作。
* 安装npm包：`npm install`。已安装cnpm的用户可用`cnpm install`.
* 安装bower包：`bower install`。

### 开始
访问http://sbadmin.dev/ 应该能访问到的了。
接下来是基本的开发工作。如，用户登录。

### 开发用户登录
1. 确定用户所需属性
* 后台登录用户是否和原系统共享用户体系？
* 如果答案为否，请自行修改`database/migration/`下[User表的Migration](https://coding.net/u/skys215/p/sbadmin/git/blob/master/database/migrations/2014_10_12_000000_create_users_table.php)。
* 如果答案为是，那么请删除里面默认的两个migration文件。

2. 修改模型层文件
   默认为`app/User.php`,若修改了命名空间（例如`App\Models`），请务必到`config/auth.php`中修改`model`属性所指类名
3. 通常后台用户是没有找回密码的功能的，此时可以删除[password_resets](https://coding.net/u/skys215/p/sbadmin/git/blob/master/database/migrations/2014_10_12_100000_create_password_resets_table.php)的migration文件。
4. 若不需要使用“记住密码”的功能，则在`app/User.php`user的模型层文件中，添加以下内容
```
    public function getRememberToken(){
        return null;
    }

    public function setRememberToken( $value ){
        //
    }

    public function getRememberTokenName(){
        return null;
    }
```
5. 若在**5.3**版本下要使用Laravel自带的用户登录与注册页面，请先运行`php artisan make:auth`命令。（[文档](https://laravel.com/docs/5.3/authentication#introduction)）
