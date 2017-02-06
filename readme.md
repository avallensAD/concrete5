#Concrete5 for Azure 
[![Deploy to Azure](http://azuredeploy.net/deploybutton.png)](https://azuredeploy.net/)


This repository contains a version of Concrete5 that can run on the Azure app service with both ClearDB and MySQL in-app. If you wish to use [MySQL in-app](https://azure.microsoft.com/en-us/blog/mysql-in-app-preview-app-service/), it is recommended to use Environment variables. 

Add this code to application\config\database.php file in order to use environment variables and support **MySQL in-app**:

```
<?php

//Get Connection string

$connectstr_dbhost = '';
$connectstr_dbname = '';
$connectstr_dbusername = '';
$connectstr_dbpassword = '';
foreach ($_SERVER as $key => $value) {
    if (strpos($key, "MYSQLCONNSTR_") !== 0) {
        continue;
    }
    
    $connectstr_dbhost = preg_replace("/^.*Data Source=(.+?);.*$/", "\\1", $value);
    $connectstr_dbname = preg_replace("/^.*Database=(.+?);.*$/", "\\1", $value);
    $connectstr_dbusername = preg_replace("/^.*User Id=(.+?);.*$/", "\\1", $value);
    $connectstr_dbpassword = preg_replace("/^.*Password=(.+?)$/", "\\1", $value);
}


return [
    'default-connection' => 'concrete',
    'connections' => [
        'concrete' => [
            'driver' => 'c5_pdo_mysql',
            'server' => $connectstr_dbhost,
            'database' => $connectstr_dbname,
            'username' => $connectstr_dbusername,
            'password' => $connectstr_dbpassword,
            'charset' => 'utf8',
        ],
    ],
];
```
