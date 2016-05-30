Lidb API (Application Programming Interface)
---------------------------------------------------------------------
```txt
Started at August 12th 2015
Version 3.0.1 - December 26th 2015

Authored by 9r3i
https://github.com/9r3i
```


i. Introduction
---

Alhamdulillah, all praises only to Allah SWT, who created the universe. I cannot finish this class without Him.

Lidb is alternative database and custom portable database. Lidb stands for Luthfie improvement database.

Yes, this is the improvement of my earlier database classes, the same way as the custom portable database.

But this one is much more improved than Ldb, encrypted in 9r3i\deema and 9r3i\gr.
Faster dan powerful, that is the basic of Lidb.

This one has professional license, and only allowed to use for some application or software that has special permission from 9r3i himself. https://github.com/9r3i/Lidb/blob/master/license.txt


ii. Requirement
---
Require php version >= 5.4, created at 5.6.8

Tested at 5.5 and 5.4, also at 5.3 by creating function array_combine

Require file Lidb.dmo (9r3i\deema), Lidb.gr (9r3i\gr)


iii. Functions
---
Lidb is mostly using function cmd

example:
```php
$lidb = new Lidb();
$lidb->cmd('[table_name]/[1st command]/[2nd commands]/[3rd commands]',$data=array());
```
explaination:
```txt
+ [table_name] = the name of table, detected by using pattern /^[a-z0-9_]+$/i
                 + create -> to create database, require [1st command] "database"
                 + drop -> to drop database, require [1st command] "database"
                 + user -> to alter users database, require [1st command] "database"
                   -> added at version 1.1.0
+ [1st command] = + drop -> to drop the selected table
                  + alter -> to alternate the selected table
                  + create -> to create a new table
                  + insert -> to insert an new entry into selected table
                  + update -> to update entries in selected table
                  + delete -> to delte entries in selected table
                  + select -> to select entries in selected table
                  + search -> to search entry in selected table
                    -> added at version 1.1
                  + preg -> to search entry preg_match in selected table
                    -> added at version 1.2.0
                  + database -> if the [table_name] set as create, drop or user
+ [2nd commands] = additional command for specific order, using url query
+ [3rd commands] = temporarily is only available if [1st command] is select, using url query
+ $data = array of data entry, used if [1st command] is alter, insert or update
```


iv. Storage
---
Lidb database storage has specific pattern:
```php
~ ^[a-zA-Z0-9_]+$\.lidb
```
Saved at directory where the class file is, or in system directory for 9r3i\deema and 9r3i\gr.


v. Scheme
---
Here is the scheme of Lidb in php array:
```php
$scheme = array(
  'db_user'=>array(
    'username'=>'password',
  ),
  'db_option'=>array(
    'created_time'=>'',
  ),
  'db_content'=>array(
    'table_name'=>array(
      'table_option'=>array(
        'auto'=>0,
        'column'=>array(
          'column_name'=>'default_value',
          'column_name'=>'default_value',
        ),
      ),
      'table_content'=>array(
        'primary_keys'=>array(
          'column_name'=>'value',
        )
      )
    ),
  )
);
```

And here is the scheme of Lidb in json:
```javascript
var scheme = {
  "db_user": {
    "username": "password"
  },
  "db_option": {
    "created_time": ""
  },
  "db_content": {
    "table_name": {
      "table_option": {
        "auto": 0,
        "column": {
          "column_name": "default_value"
        }
      },
      "table_content": {
        "primary_keys": {
          "column_name": "value"
        }
      }
    }
  }
}
```


vi. Methods
---
Here they are, the methods of Lidb:
```php
Lidb::__construct -> build Lidb class
Lidb::connect     -> connect to Lidb server
Lidb::cmd         -> command string
Lidb::test        -> test connection to Lidb host
Lidb::close       -> close the connection
Lidb::destroy     -> unset the active class
Lidb::Lidb_Lo9    -> create Lo9 encoding string
Lidb::json        -> create json output pretty print
```
Mostly Lidb is using cmd command string to execute its query.


vii. Variables
---
Here they are, the variables of Lidb:
```php
  public object $info;
  public string $database;
  public string $error;
  public array $errors;
  public string $status;
  public string $host;
  public string $host_version;
  public array $default_values;
```


viii. Command String (CMD)
---
Here is Lidb CMD descriptions:

DATABASE commands
```txt
[create]--+               +-->[db_name]
          |               |
          +-->[database]--+-->[db_user]
          |               |
[drop]----+               +-->[db_pass]
```
example:
```php
Lidb::cmd -> create/database/db_name=test_db&db_user=test_user&db_pass=mypass
Lidb::cmd -> drop/database
```

USER commands
```txt
[user]-->[database]--+-->[add]----------------+           +-->[username]
                     |                        |           |
                     +-->[edit]-->[username]--+-->[data]--+
                     |                                    |
                     |                                    +-->[password]
                     +-->[remove]-->[username]
```
example:
```php
Lidb::cmd -> user/database/add
          -> [data][username]=>"luthfie" and [data][password]=>"mypass1"
Lidb::cmd -> user/database/edit/luthfie
          -> [data][username]=>"luthfie" and [data][password]=>"mypass2"
Lidb::cmd -> user/database/remove/luthfie
```

TABLE commands
```txt
[table_name]--+-->[create]-------------------+-->[data]-->[column]-->[default_value]
              |                              |
              +-->[drop]                     |
              |                              |
              +-->[alter]--+-->[column]------+
              |            |                 |
              |            +-->[add_column]--+
              |            |
              |            +-->[drop_column]---->[data]-->[0]-->[column_name]
              |
              +-->[insert]------------+-->[data]-->[column]-->[value]
              |                       |
              +-->[update]-->[index]--+
              |
              +-->[delete]-->[index]
              |
              +-->[select]-->[index]--+-->[start=0&limit=10&sort=ASC&order=[key]&output=[key]]
              |                       |
              +-->[search]-->[index]--+
              |                       |
              +-->[preg]-->[pattern]--+
```
example:
```php
Lidb::cmd -> posts_table/create
          -> [data][id]=>"LIDB_AID"
          -> [data][title]=>""
          -> [data][content]=>""
          -> [data][status]=>"publish"
          -> [data][time]=>"LIDB_TIME"
          -> [data][date]=>"LIDB_DATE"
Lidb::cmd -> post_table/drop
Lidb::cmd -> post_table/alter/column
          -> [data][title]=>"default title"
Lidb::cmd -> post_table/alter/add_column
          -> [data][author]=>"Abu Ayyub"
Lidb::cmd -> post_table/alter/drop_column
          -> [data][0]=>"author"
Lidb::cmd -> posts_table/insert
          -> [data][title]=>"test judul"
          -> [data][content]=>"yang takkan pernah terlupakan selamanya"
Lidb::cmd -> posts_table/update/id=1
          -> [data][title]=>"test title"
          -> [data][content]=>"dan itupun takkan mungkin terjadi kembali"
Lidb::cmd -> posts_table/delete/id=1
Lidb::cmd -> posts_table/select
Lidb::cmd -> posts_table/select/status=publish/start=0&limit=10&sort=ASC&order=id&output=id,title,content
Lidb::cmd -> posts_table/select/id=1/output=id,title,content
Lidb::cmd -> posts_table/search/content=kembali
Lidb::cmd -> posts_table/preg/content=|[a-z]+|i
```


ix. Connecting
---
Lidb has usual connection like the other database.
Using function connect

example:
```php
$lidb_host = 'localhost';
$lidb_name = 'testDB';
$lidb_user = 'root';
$lidb_pass = 'mypass';
$lidb = new Lidb(34,$lidb_host);
/* if using host, it is better to test the connection */
if($lidb->test()){
  $lidb->connect($lidb_name,$lidb_user,$lidb_pass);
}
```



And to close the connection is:
```php
$lidb->close();
```
Or:
```php
$lidb->destroy();
```
The different between close and destroy is close method is only disconnecting the connected database, while the destroy is unsetting the whole defined object in global variable.


---
Copyright (c) 2012-2016, 9r3i


