---
layout: post
title: 5.0 Deploying to Github Pages
color: rgb(239, 192, 80)
tags: [websolute,team,react,JSX]
---

#### 5.0 Deploying to Github Pages


__gh-pages 설치__
- `npm i gh-pages`
- 웹사이트를 guthub의 github page 도메인에 나타나게 해줌

__`<package.json>`에서 home page 추가__
- `"homepage":"https://suminRoh.github.io/movie_app_2021/"}`


__scripts에 deploy 추가__
- `"deploy":"gh-pages "`



__명령어 만들기__
- deploy :  build 폴더를 upload
- predeploy: npm run build


__`<index.html>` 에서 title 바꾸기__
-`<title>Movie App</title>` 


__https://suminroh.github.io/movie_app_2021/ 링크로 내가 만든 Movie app 사이트 접속 가능 !!__