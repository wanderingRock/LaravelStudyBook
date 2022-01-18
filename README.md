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



### Laravel DataTable(Server Side)
參考: https://www.itsolutionstuff.com/post/laravel-datatables-server-side-processing-exampleexample.html

1.Install Yajra Datatable
```
composer require yajra/laravel-datatables-oracle
```

2.將下方程式碼加入config/app.php
```
.....
'providers' => [
	....
	Yajra\DataTables\DataTablesServiceProvider::class,
]
'aliases' => [
	....
	'DataTables' => Yajra\DataTables\Facades\DataTables::class,
]
.....
```

3.tinker產生假資料
```
php artisan tinker

factory(App\User::class, 200)->create();
```
4.路由新增
routes/web.php
```
Route::get('users', ['uses'=>'UserController@index', 'as'=>'users.index']);
```

5.新增 Controller
app/Http/Controllers/UserController.php
```
<?php
       
namespace App\Http\Controllers;
       
use App\User;
use Illuminate\Http\Request;
use DataTables;
       
class UserController extends Controller
{
    /**
     * Display a listing of the resource.
     *
     * @return \Illuminate\Http\Response
     */
    public function index(Request $request)
    {
        if ($request->ajax()) {
            $data = User::select('*');
            return Datatables::of($data)
                    ->addIndexColumn()
                    ->addColumn('action', function($row){
     
                           $btn = '<a href="javascript:void(0)" class="edit btn btn-primary btn-sm">View</a>';
       
                            return $btn;
                    })
                    ->rawColumns(['action'])
                    ->make(true);
        }
        
        return view('users');
    }
}
```
6.建立view
resources/views/users.blade.php
```
<!DOCTYPE html>
<html>
<head>
    <title>Laravel Datatables Server Side Data Processing Example - ItSolutionStuff.com</title>
    <meta name="csrf-token" content="{{ csrf_token() }}">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/twitter-bootstrap/4.1.3/css/bootstrap.min.css" />
    <link href="https://cdn.datatables.net/1.10.16/css/jquery.dataTables.min.css" rel="stylesheet">
    <link href="https://cdn.datatables.net/1.10.19/css/dataTables.bootstrap4.min.css" rel="stylesheet">
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.js"></script>  
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery-validate/1.19.0/jquery.validate.js"></script>
    <script src="https://cdn.datatables.net/1.10.16/js/jquery.dataTables.min.js"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/js/bootstrap.min.js"></script>
    <script src="https://cdn.datatables.net/1.10.19/js/dataTables.bootstrap4.min.js"></script>
</head>
<body>
    
<div class="container">
    <h1>Laravel Datatables Server Side Data Processing Example <br/> ItSolutionStuff.com</h1>
    <table class="table table-bordered data-table">
        <thead>
            <tr>
                <th>No</th>
                <th>Name</th>
                <th>Email</th>
                <th width="100px">Action</th>
            </tr>
        </thead>
        <tbody>
        </tbody>
    </table>
</div>
   
</body>
   
<script type="text/javascript">
  $(function () {
    
    var table = $('.data-table').DataTable({
        processing: true,
        serverSide: true,
        ajax: "{{ route('users.index') }}",
        columns: [
            {data: 'id', name: 'id'},
            {data: 'name', name: 'name'},
            {data: 'email', name: 'email'},
            {data: 'action', name: 'action', orderable: false, searchable: false},
        ]
    });
    
  });
</script>
</html>
```
建立完後即可進頁面查看基本的serverside
### Laravel 目錄結構
https://justcode.ikeepstudying.com/2021/07/php%E6%A1%86%E6%9E%B6%EF%BC%9Alaravel-%E9%A1%B9%E7%9B%AE%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84%E4%BB%8B%E7%BB%8D-laravel%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84-laravel-%E5%90%84%E6%96%87%E4%BB%B6/
