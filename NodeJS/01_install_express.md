## 🛠 Install & Run Express

> 선수환경 : npm, Visual Studio

### Node.js 란?
- 이벤트 기반
- 비동기(Asynchronous)
  > 결과가 나올 때까지 기다리지 않고 계속 진행. 결과가 나오면 이벤트가 발생함
  
### NPM (Node Package Manager) 이란?
- 패키지를 쉽게 사용할 수 있도록 해주는 프로그램
- ``` package.json ``` : 현재 작업중인 패키지의 메타 정보
- 사용법
```sh
$ npm init
$ npm install(uninstall) [package name]
```
  - --save : package.json 에 dependency 저장 
  - --no-save : save 하지 않음
  - --save-dev : package.jsson 에 dev-dependency 저장
  - -g : global 로 install (command 에 추가) 


### Express 란?
- Node.js 를 위하 빠르고 개방적인 간결한 web framework
- 미들웨어들로 연결됨 (req, res, next)
- view 를 위한 템플릿 엔진은 다양하게 이용 가능

<br/>

### Let's Install Express 🔥

설치를 원하는 디렉토리에 들어가서 다음 명령어를 입력한다.

```
npm install -g express-generator
```

설치 완료 후 원하는 프로젝트 명을 넣어 다음 명령어를 입력한다.

```
express [project name] -v pug
```
> 나는 html 대신 pug 를 사용하는 것이 편해 명령어 뒤에 ``` -v pug ``` 를 입력했다.   

> ``` --git ``` 명령어를 뒤에 붙이면 .gitignore 파일을 자동으로 생성해준다.   
``` -c sass ``` 명령어는 css 는 sass 를 사용하게 한다.

```
cd [project name]
npm install
npm start
```
방금 생성시킨 프로젝트에 들어가서 ``` npm install ``` 한 후 ``` npm start ``` 명령어로 실행시켜 보았다.

<img src="./screenshots/0-server.png" width="600">


``` http://localhost:3000 ``` 주소에서 위 화면이 뜬다면 성공이다❗️

<br/>

데이터가 바뀔 때마다 다시 서버를 껐다 켜기 귀찮으므로 ``` nodemon ``` 을 설치해준다.

```
npm install nodemon
nodemon
```

개발 환경이 편리해졌다 👏 개발할 준비 완료❗️


<br/>

## Route 란?

특정 url/method 가 왔을 때 처리하는 handler 를 등록함

```js
var express = require('express');
var router = express.Router();

/* GET home page. */
router.get('/', function(req, res, next) {
  res.render('index', { title: 'Express' });
});

module.exports = router;

```
> 기본으로 setting 된 코드 (```routes/index.js```)

- req : 미들웨어 함수에 대한 **HTTP 요청** 인수
- res : 미들웨어 함수에 대한 **HTTP 응답** 인수 
- next : 미들웨어 함수에 대한 **callback** 인수
