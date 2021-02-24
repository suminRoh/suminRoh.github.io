---
layout: post
title: 6.4 Redirecting
color: rgb(239, 192, 80)
tags: [websolute,team,react,JSX]
---

#### 6.4 Redirecting


- __`<Detail.js>`__
```javascript
function Detail({location}){
    console.log(location);
    return <span>
        Hello
    </span>;
}
```
- __문제__
    - location의 props들이 console에 나타나지만 poster를 눌러서 movie-detail에 들어간 경로가 아닌 url에 /movie-detail이라고 쳐서 들어간 경우, state가 undefined라고 뜸

- __해결 방법__

```javascript
import React from 'react';

class Detail extends React.Component{
    componentDidMount(){
        const{location,history}=this.props;
        if(location.state===undefined){
            history.push("/"); //redirect
        }
    }
    render(){
        const{location}=this.props;
        return <span>{location.state.title}</span>;
    }
}

export default Detail;
```

- location에서 state가 undefined일 경우, history의 push 경로를 "/"으로 변경시켜 Home으로 가게 만듦

-__문제 :  poster을 누르면 location.state.title이 뜨지만, 새로고침하면 error가 뜸__


- 이유: render() 이후, componentDidMount()이 실행되는데 새로고침을 하면  componentDidMount()에서 if문이 실행되기전이여서 render()함수가 실행될 때 location.state가  undefined된 상태여서location이 존재하지않기 때문


- __해결 방법__

 ```javascript
 if(location.state){
            return <span>{location.state.title}</span>;
        }
        else{
            return null;
        }
```