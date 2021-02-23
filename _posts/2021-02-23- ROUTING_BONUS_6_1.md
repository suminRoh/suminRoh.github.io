---
layout: post
title: 6.1 Building the Router
color: rgb(239, 192, 80)
tags: [websolute,team,react,JSX]
---



#### 6.1 Building the Router


라우터 : url을 확인하고 우리가 라우터에게 무엇을 명령했느냐에 따라 알맞은 component를 불러옴 
- `import { HashRouter,Route} from "react-router-dom";` 추가


```javascript
import { HashRouter,Route} from "react-router-dom";
import Home from "./routes/Home";
import About from "./routes/About";

function App(){
  return <HashRouter>
    <Route path="/" component={Home}/>
    <Route path="/about" component={About}/> 
  </HashRouter>;
}
```
- HashRouter 먼저 만들고 Route 넣어줌 
-  Route에는 스크린을 넣어주는 것 , 원하는만큼 path를 만들 수 있음
- 기본 url 들어오면 Home component 보여줌  
- path about으로 들어오면 About component를 보여줌 
    - url에 /about을 붙여주면 About component 볼 수 있음 


- __문제 :  url을 들어가면 Home만 보이지만  /about으로 들어가면 About 과  Home component가 모두 보임__ 
    - 2개의 component가 동시에 rendering 
    - 리액트 라우더는 기본적으로 url을 가져와서 라우터들과 비교하며 매치되는 라우터들을 모두 가져옴 
        - /about이 가져온 url 이고 "/" 또한 하나의 라우터로 생각해서 매치되고 "/about" 또한 매치됨 

- __해결방법 exact={true}를 태그 안에 넣어줌__ 
    - path와 정확히 일치할 때만 rendering 해줌 


  