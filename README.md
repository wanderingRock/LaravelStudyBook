# LaravelStudyBook

相關指令
--
1.如有現有專案或是框架,一開始要先composer install
```
composer install
```

2.資料庫的產生   

因為是使用laradock,所以要進入容器下php指令    
```
進入容器 (php-fpm) 放編放或是容器名稱
docker exec -it d42d bash  
winpty docker exec -it d42d bash

先建好資料庫再產生資料   
php artisan migrate

資料庫直接重整並生成種子
php artisan migrate:fresh --seed

資料庫產生虛擬資料 > 對user產生200筆虛擬資料
php artisan tinker
factory(App\User::class, 200)->create(); 
```



### Laravel 相關說明(實做&紀錄)



### Laravel 目錄結構
https://justcode.ikeepstudying.com/2021/07/php%E6%A1%86%E6%9E%B6%EF%BC%9Alaravel-%E9%A1%B9%E7%9B%AE%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84%E4%BB%8B%E7%BB%8D-laravel%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84-laravel-%E5%90%84%E6%96%87%E4%BB%B6/
