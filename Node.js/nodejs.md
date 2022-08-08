# Node.js

### Create Account

- 유저 스키마에서 username, email은 unique 설정을 주면 콜렉션 내에서 중복이 없음을 보장한다

```json
{
	name: String,
	username: { type: String, unique: true },
	email: { type: String, unique: true },
	password: { type: String },
	location: { type: String},
}
```

- 패스워드의 보안을 위해 해시화해서 저장할 필요가 있음
- bcrypt 패키지를 참조해서 패스워드를 해시화
- mongodb 에서 find 할 경우 $or 연산자를 이용해서 둘중 하나만 만족하는 경우를 찾아낼 수 있다. (유저네임 혹은 이메일의 중복 체크)
  [$or](https://docs.mongodb.com/manual/reference/operator/query/or/)
- express-session 미들웨어를 사용하면 각각의 브라우저에 sid를 부여한다, 또한 메모리에 브라우저에 부여한 sid를 저장한다
- 브라우저는 부여된 sid를 쿠키에 저장하고 서버와의 데이터 송신시에 매번 전송한다
- 세션 정보는 json 오브젝트이며, 기본적으로 서버상의 메모리에 저장된다
- 세션 정보에는 코딩을 통해 원하는 값을 입력할수 있으며 이는 각 브라우저를 식별해 정보를 저장하는것이 된다
- pug와 express의 기본 정보 공유 변수는 res.locals 이며 pug에서 사용할때는 #{변수명} 으로 바로 호출 가능

```json
# in express
res.locals.var = test

# in pug
#{test}
```

- 세션은 메모리베이스 저장소이기 때문에 계속 기억해두려면 데이터베이스와 연결해서 세션을 저장할 필요가 있음
- connect-mongo 가 몽고DB와 연동되는 패키지
- resave: false
- saveUninitialized: false (세션이 실제로 변경될때 저장하도록 함)
  [express-session 패키지의 resave, saveUninitialized 옵션](https://fierycoding.tistory.com/36)
- JWT방식 개발
  [Passport.js + JWT 로 유저 인증 하기 - chanyeong](https://chanyeong.com/blog/post/28)

[node.js cookie & signedCookie](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=pxkey&logNo=221220232811)
