---
layout: post
title: React Hooks (Use State)
color: rgb(239, 192, 80)
tags: [websolute,team,react,JSX,Hooks,UseState]
---

### React Hooks
Hooks 는 리액트 v16.8 에 새로 도입된 기능으로서, 함수형 컴포넌트에서도 상태 관리를 할 수 있는 useState, 그리고 렌더링 직후 작업을 설정하는 useEffect 등의 기능 등을 제공하여 기존의 함수형 컴포넌트에서 할 수 없었던 다양한 작업을 할 수 있게 해줌


### 1.0 Introduction to useState


- class component로 작성한 코드
```javascript
class Appugly extends React.COmponent{
  state={
    item:1
  }
  render(){
    const{item}=this.state;
    return(
      <div className="App">
        <h1>Hello CodeSandbox {item}</h1>
        <h2>Start editing to see some magic happen!</h2>
        <button onClick={incrementItem}>Increment</button>
        <button onClick={decrementItem}>Decrement</button>
      </div>
    );
  }
  incrementItem=()=>{
    this.setState(state=>{
        return{
          item:state.item+1
        }
    })
  }
  decrementItem=()=>{
    this.setState(state=>{
        return{
          item:state.item-1
        }
    })
  }
}
```


- Hooks를 이용해 작성한 코드
```javascript
const App=()=>{
  const [item, setItem]=useState(1);
  //useState:초기에 state를 InitialState를 세팅할 수 있는 옵션을 제공
  const incrementItem=()=> setItem(item+1);
  const decrementItem=()=> setItem(item-1);
  return(
    <div className="App">
      <h1>Hello CodeSandbox {item}</h1>
      <h2>Start editing to see some magic happen!</h2>
      <button onClick={incrementItem}>Increment</button>
      <button onClick={decrementItem}>Decrement</button>
    </div>
  );
}
```

- __Hooks를 이용해 훨씬 간결하게 코드 사용 가능!!__
- class component는 this,setState등을 써야하지만 Hooks는 필요 없음 


### 1.1 useInput


__useInput:기본적으로 input을 업데이트__


```javascript
const useInput=(initialValue)=>{
  const [value,setValue]=useState(initialValue);
  const onChange=event=>{
    console.log(event.target);
  };
  return {value,onChange};
};
const App=()=>{
  const name=useInput("Mr.");
  return(
    <div className="App">
      <h1>Hello</h1>
      <input placeholder="Name" value={name.value} onchange={name.onChange}/>
      
    </div>
  );
}
```


` value={name.value} onchange={name.onChange}`
-> `{...name}`으로  name 안에 있는 모든 것들을 unpack 가능!!


### 1.2 useInput part Two


__유효성을 검증하는 기능 포함__

```javascript

const useInput=(initialValue,validator)=>{
  const [value,setValue]=useState(initialValue);
  const onChange=event=>{
    const {
      target:{value}
    }=event;
    let willUpadate=true;
    if(typeof validator==="function"){
      willUpadate=validator(value);
    }
    if(willUpadate){
      setValue(value);
    }

  };
  return {value,onChange};
};
```


- const App에 ` const maxlen=(value)=>!value.includes("@");` 추가 
    - "@"일 경우 적용안됨


### 1.3 useTabs 


```javascript
const content=[
  {
    tab:"Section 1",
    content:"I'm the content of the Section 1"
  },
  {
    tab:"Section 2",
    content:"I'm the content of the Section 2"
  }
];

const useTabs=(initialTab,allTabs)=>{ //initialTab:index, allTabs:array
  const [currentIndex,SetCurrentIndex]=useState(initialTab);
  //setCurrentIndex : state를 update시켜줌
  if(!allTabs ?? !Array.isArray(allTabs)){
    return ; //array가 아니면 실행 x
  }
  return{ 
    currentItem:allTabs[currentIndex],//선택한 인덱스의 배열 값
    changeItem:SetCurrentIndex  //map의 idex를 통해 변화하는 인덱스로 업데이트 하기 위한 변수
  };
}

const App=()=>{
  const {currentItem,changeItem}=useTabs(1,content);
  return(
    <div className="App">
      {content.map((section,index)=>(<button onClick={()=>changeItem(index)}>{section.tab}</button>
      ))} 
      <div> {currentItem.content} </div>
     
    </div>
  );
  //버튼을 클릭하면 changeItem이 실행되고 index값이 바뀜 
  //그건 SetCurrentIndex의 item을 바꿔줌 
  //이는 state를 바꾸고 이로 인해 currentItem의 currentIndex도 바꿈 
}

```



 
