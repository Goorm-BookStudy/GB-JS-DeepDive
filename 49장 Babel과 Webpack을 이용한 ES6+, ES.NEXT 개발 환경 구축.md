# 49장 Babel과 Webpack을 이용한 ES6+, ES.NEXT 개발 환경 구축
- 크롬, 사파리, 파이어폭스 등 대부분의 브라우저에서 ES6 사양을 지원한다.
- 하지만 IE 11의 경우 11%를 지원한다.
- 이처럼 브라우저마다 지원율이 제각각이다.

<br/>

## Babel
- 다음 예제는 ES6의 화살표 함수와 ES7 지수 연산자를 사용하고 있다.

```
[1,2,3].map(n => n **n);

// 브라우저에서 지원하지 않을 경우 Babel을 사용하면
'use strict';

[1,2,3].map(function(n){
	return Math.pow(n, n);
});
```

- 구형 브라우저에서도 동작하는 소스코드로 변환(트랜스파일링)할 수 있다.

<br/>

### Babel 설치

```
npm init -y

npm install --save-dev@babel/core @babel/cli
```

- babel을 사용하기 위해 @babel/preset-env 설치해야 한다. (함께 사용되어야 하는 babel 플러그인을 모아둔 것을 바벨 프리셋이라고 부른다.
- @babel/preset-env는 필요한 플로그인들을 프로젝트 지원 환경에 맞춰 동적으로 결정해 준다.
- 프로젝트 루트 폴더에 babel.config.json 설정 파일을 생성하고 다음과 같이 작성하면 @babel/preset-env를 사용하겠다는 의미다.

```
{
	"presets": ["@babel/preset-env"]
}
```

<br/>

### 트랜스파일링
- 매번 Babel CLI를 입력할 순 없으므로 package.json을 활용하여 scripts에 명령어를 등록하여 사용한다.

```
"scripts": {
  "build": "babel src/js -w -d dist/js"
}
```

<br/>

### Babel 플러그인 설치
- Babel 홈페이지에서 public/private 클래스 필드를 지원하는 @babel/plugin-proposal-class-properties 설치

```
npm install --save-dev @babel/plugin-proposal-class-properties
```

```
{
	"presets": ["@babel/preset-env"],
    "plugins": ["@babel/plugin-proposal-class-properties"]
}
```

<br/>

### 브라우저에서 모듈 로딩 테스트
- 브라우저는 node.js와 달리 CommonJS 방식의 require 함수를 지원하지 않으므로 트랜스파일링된 결과를 그대로 브라우저에서 실행하면 에러가 발생한다. 
- Webpack을 통해 문제 해결이 가능하다.

<br/>

## Webpack
- 의존 관계에 있는 JS, CSS, 이미지 등의 리소스들을 하나의 파일로 번들링하는 모듈 번들러다.
- JS 파일을 하나로 번들링하므로 HTML에서 script 태그로 여러 개의 JS 파일을 로드해야하는 번거로움도 사라진다.

<br/>

### Webpack 설치

```
npm install --save-dev webpack webpack-cli
```

<br/>

### babel-polyfill 설치
- Promise, Object.assign, Array.from 등은 트랜스파일링이 되지 못하고 그대로 남아서 이를 위해 babel-polyfill을 설치한다.

```
npm install @babel/polyfill
```

- 개발 환경에서만 사용하는 것이 아닌 실제 운영 환경에서도 사용해야하기 때문에 --save-dev 옵션을 지정하지 않는다.
- 바벨만 사용하는 경우 진입점 파일의 선두에 ```import @babel/polyfill";``` 을 작성한다.
- Webpack을 사용하는 경우 entry 배열에 폴리필을 추가하여 사용한다. 

```
entry: ['@babel/polyfill', './src/js/main.js'],
```

- 이후 명령어로 실행하면, 번들링된 파일을 확인해보면 폴리필이 추가된 것을 확인할 수 있다.
