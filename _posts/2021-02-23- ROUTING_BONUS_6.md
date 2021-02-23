---
layout: post
title: 6.0 Getting Ready for the Router
color: rgb(239, 192, 80)
tags: [websolute,team,react,JSX]
---


#### 6.0 Getting Ready for the Router


- react-router dom : 네비게이션을 만들어주는 패키지
    - `npm install react-router-dom`


-components 폴더에 `<Movie.css>`,`<Movie.js>` 넣기


- routes 폴더에 `<About.js>`,`<Home.css>`,`<Home.js>` 넣기 


- `<App.js>` 코드 `<Home.js>`로 옮기기 


`<App.js>`
```javascript
import React from 'react';

function App(){
  return <span>something</span>;
}

export default App; 
```
- something이라는 문자만 나옴  
- react-router dom을 통해 Home.js와 About,js에 접근하는 방법 알아야함 