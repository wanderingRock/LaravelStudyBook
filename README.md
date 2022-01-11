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
進入容器 (php-fpm)
docker exec -it d42d bash  
winpty docker exec -it d42d bash

先建好資料庫再產生資料   
php artisan migrate

資料庫直接重整並生成種子
php artisan migrate:fresh --seed
```
