## RestAPI With MySQL 

> 선수환경 : MySQL

<img src="./screenshots/2-mysql.png" width="400">

### Install MySQL

```sh
$ npm i mysql
```

### Connect to MySQL

```js
const mysql = require('mysql')
```
상단에 ```mysql``` library 를 불러와준다.

```js
const connection = mysql.createConnection({
  host: 'localhost',
  user: [username],
  password: [pw],
  database: [db name]
})
```
연결할 준비 끝❗️

### 전체 user 리스트 불러오기

```js
router.get('/', function(req, res, next) {
  var queryString = "SELECT * FROM user"

  connection.query(queryString, (err,rows,fields) => {
    res.json(rows)
  })

});
```

<img src="./screenshots/2-mysql2.png" width="300"/>

### 원하는 user 리스트 불러오기

```js
router.get('/:id', function(req,res,next) {
  var queryString = "SELECT * FROM user WHERE id = ?"
  var userId = req.params.id

  connection.query(queryString, [userId], (err,rows,fields) => {
    if(err){
      console.log(err)
      res.sendStatus(500)
      return
    }
    res.json(rows)
  })
})
```

<img src="./screenshots/2-mysql3.png" width="300">

