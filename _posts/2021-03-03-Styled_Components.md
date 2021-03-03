---
layout: post
title: Styled_Components!
color: rgb(239, 192, 80)
tags: [websolute,team,react,JSX]
---

#### #1 셋업

- __`<App.js>`__

```javascript
import React,{Component,Fragment} from "react"; 
import "./App.css";
class App extends Component{
  render(){
    return (
      <Fragment>
        <button className="button button--success">Hello</button>;
        <button className="button button--danger">Hello</button>;        
      </Fragment>
    );
  }
}
export default App;
```

- class명 2개 사용 안좋음


- __`<App.css>`__

```javascript
.button{
    border-radius:50px;
    padding:5px;
    min-width:120px;
    color:white;
    font-weight:600;
    -webkit-appearance:none;
    cursor:pointer;
}
.button:active,
.button:focus{
    outline:none;
}
.button--success{
    background-color:#2ecc71;
} 
.button--danger{
    background-color:#e74c3c;
    
}
```


#### #3 injectGlobal and Extend


- __목표: margin을 바꾸기__


__v4부터 injectGlobal이  createGlobalStyle로 바껴  createGlobalStyle사용함__


- `import styled,{ createGlobalStylel} from "styled-components";` 추가


```javascript
createGlobalStyle`
(CSS 스타일링 코드)
`
```


- 버튼을 앵커로 사용하고 싶다면?
    - Anchor -Link component를 이용

```javascript
const Anchor=styled(Box.withComponent("a"))`
text-decoration:none;
`;
//anchor component를 사용하고 Box의 component연장 
```

-`<class App>`
` <Anchor href="http://google.com">Go to google</Anchor> ` 추가


#### #4 Animations



- `import styled, { createGlobalStyle,keyframes} from "styled-components";` 추가

- rotation 정의

```javascript
const rotation =keyframes`
  from{
    transform: rotate(0deg);
  }to{
    transform: rotate(360deg);
  }
`;
```

- Button style에 추가

```javascript
  ${props=>{
    if(props.danger){
      return css`animation: 2s ${rotation} linear infinite`;
    }
  }};
```

- props.danger인 경우, rotation을 하게 함


#### #5 Extra Attributes and Mixins


- Attributes
```javascript
const Input=styled.input.attrs({
  required:true  
})`
  border-radius:5px;
`;
```
- styled에 attrs를 추가시켜 required라는 attributes 생성
웹페이지에 Introspection 확인해보면 Input 태그에 required있는 것 확인할 수 있음 



- Mixin (자주 사용하는 스타일인 CSS 그룹)
```javascript
const awesomeCard=css`
  background-color:white;
  border-radius:10px;
  padding:20px;
`;
```


- 사용하고 싶은 곳의 스타일에 `  ${awesomeCard};` 추가


#### #6 Theming


__Theme은 모든 element에 적용됨__

-__`<theme.js>`__
```javascript
const theme={
    mainColor: "#3498db",
    dangerColor:"#e743c",
    successColor:"#2ecc71"
}

export default theme;
```


__모든 컴포넌트에 이 theme을 적용하는 방법은?__
- `import styled, { createGlobalStyle,ThemeProvider} from "styled-components";`
- `import theme from "./theme";`추가


```javascript
class App extends Component{
  render(){
    return (
      <ThemeProvider theme={theme}>
        <Container>
          <Form />
        </Container>  
      </ThemeProvider>
    );
  }
}
//<ThemeProvider>로 감싸줌
```

- `background-color: ${props=>props.theme.successColor};`
    - theme의 successColor를 backgroundColor로 사용 

#### #7 Nesting

```javascript
${Card}{
    background-color:blue;
  }
```
