---
layout: post
title: 6.2 Building the Navigation
color: rgb(239, 192, 80)
tags: [websolute,team,react,JSX]
---

#### 6.2 Building the Navigation

__목표 : 네비게이션 (버튼) 만들기__


- __`<App.js>` 할 일__  
    - `import Navigation from "./components/Navigation";` 추가
-function App 안에 ` <Navigation />` 추가 

- __`<Navigation.js>`__

```javascript
import React from "react";

function Navigation(){
    return <div>
        <a href="/">Home</a>
        <a href="/about">About</a>
        
    </div>
}
// href는 HTML로 페이지를 새로고침시킴
// Home 과 About을 눌러보면 페이지가 새로고침됨
// 리액트가 죽고 새 페이지가 새로고침

export default Navigation;
```

- __link를 사용해야함__
    - 페이지 새로고침 대신 About과 Home페이지 들어가짐 
    - __Link를 사용할 경우, Link는 라우터 안에 있어야함__


```javascript
import React from "react";
import {Link} from "react-router-dom";

function Navigation(){
    return <div>
        <Link to="/">Home</Link>
        <Link to="/about">About</Link>
        
    </div>
}

export default Navigation;
```
