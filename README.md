# 프리미엄 서비스 (평가 / 통계 / 환경설정 )

## 서버 정보
```
CentOS : 6.9 (Final)
Apache : Apache/2.2.15 (Unix)
PHP : 5.3.3
MySQL : 5.1.73

NodeJS : 10.15.0
NPM : 6.5.0
```

## 프로젝트 설치
```
npm install
```

## 프로젝트 환경 설정
```
:: File .env.development
 # 프로젝트 실행 전, 운영서버의 API를 사용하고 싶다면 true
 # Local PC의 API를 사용하고 싶다면 false
 
 VUE_APP_USE_SERVE_PHP=true    # 운영서버의  PHP 사용, default true
 VUE_APP_USE_SERVE_JAVA=true   # 운영서버의 JAVA 사용, default true
 
 # 아래 포트정보는 로컬 API 사용시만 사용됨
 VUE_APP_PPORT=8080	# 로컬 php API 아파치 포트
 VUE_APP_JPORT=81		# 로컬 java 프리미엄 API 스프링부트 포트
```

### 프로젝트 실행
```
npm run serve
```
 - 기본적으로 https 로 동작하도록 세팅되어 있으며, local.scrapmaster.co.kr 을 로컬호스트로 지정하여 사용한다.
 - ["https README"](https://dev-git.scrapmaster.co.kr/daejeonmember/premium_eval-vue/-/blob/master/ssl/README.md "value") 
 - ["https README(RAW)"](https://dev-git.scrapmaster.co.kr/daejeonmember/premium_eval-vue/-/raw/master/ssl/README.md "value") 

### 배포본 빌드
```
npm run build
 - dist 폴더에 배포본 생성됨
 - phpapi와 함께 배포하면 됨
  : premium_eval-phpapi/src 내 evalPhp 폴더를 같이 배포
 - 아파치 설정변경(우분투 기준 아래 명령)
  : rewrite모듈 활성화 & 관련 설정 추가(.htaccess 사용하도록)
  $ a2enmod rewrite
  $ sed -i 's/AllowOverride None/AllowOverride All/g' /etc/apache2/apache2.conf
```

### 프로젝트 배포 (jenkins 사용 - php 통합)
```
%NODE_PATH% : 로컬(또는 빌드 환경)의 node.exe 절대경로
%VUEJS_WORKSPACE% : 로컬(또는 빌드 환경)의 vue.js workspace 절대경로

:: Build
  set "_node=%NODE_PATH%\node.exe" && cd "%VUEJS_WORKSPACE%" && bundle.cmd

:: Post-build Actions (Exec command)
  cd /home/tomcat/apache-tomcat-9.0.34/webapps_eval/ROOT
  cdt=$(date +"%Y%m%d_%H%M%S")
  mkdir -p ../b/$cdt && mv api css img js favicon.ico index.html ../b/$cdt
  cd dist && mv api css img js favicon.ico index.html ../
  cd .. && rm -r ./dist

Jenkins 프로젝트의 상기 두 곳 수정 확인 후 'Build with Parameters' 실행
```


### 폴더 구조 규칙
```
.
├─ README.md
├─ .env
├─ .env.development
├─ .gitignore
├─ .project
├─ babel.config.js
├─ babel.cmd
├─ fetch.ftp
├─ package_org.json
├─ package-lock.json
├─ package.json
├─ vue.config.js
├─ WinSCP.com
├─ WinSCP.exe
└─ public
│  ├─ favicon.ico
│  └─ index.html
└─ src
   ├─ App.vue
   ├─ common.js 
   ├─ main.js
   ├─ routes.js         라우터
   ├─ store.js          상태 관리
   ├─ components        컴포넌트
   │  ├─ common
   │  └─ ...
   ├─ views             라우터 페이지
   │  ├─ MainView.vue
   │  └─ ...
   └─ assets            기타 자원
       ├─ images        이미지
       ├─ js            
       └─ styles        css
```
