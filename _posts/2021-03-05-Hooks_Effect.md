---
layout: post
title: React Hooks (Use Effect)
color: rgb(239, 192, 80)
tags: [websolute,team,react,JSX,Hooks,UseEffect]
---

### 2.0 Introduction to useEffect 

- __useEffect__
    - componentDidMount,ComponentWillunMount, componentDidUpdate역할을 실행 
    - 2개의 인자를 받는데 첫번째는 function으로써의 effect(componentDidMount),
    두번째는 deps 만약 deps가 있다면 effect는 (deps)리스트에 있는 값일때만 값이 변하도록(실행) 활성화(componentWillUpdate)
    - component가 mount되었을 때만 실행시키고 싶다면 빈 dependency전달 ([])


```javascript
const App=()=>{
  const sayHello=()=>{
    console.log("Hello");
  }
  const [number,setNumber]=useState(0);
  const [aNumber,setAnumber]=useState(0);
  useEffect(sayHello,[])
  return(
    <div className="App">
      <div>Hi</div>
      <button onClick={()=>setNumber(number+1)}>{number}</button>
      <button onClick={()=>setAnumber(aNumber+1)}>{aNumber}</button>
    </div>
  );
}
```



### 2.1 useTitle
```javascript
const useTitle=(initialTitle)=>{
  const [title,setTitle]=useState(initialTitle);
  const updateTitle=()=>{
    const htmlTitle=document.querySelector("title") //html tag의 title을 얻어야함
    htmlTitle.innerText=title;
  };
  useEffect(updateTitle,[title]);//componentDidMount이거나 title이 업데이트되면, updateTitle함수 호출
  return setTitle;
};

const App=()=>{
  const titleUpdater=useTitle("Loading..."); //setTitle과 동일 
  setTimeout(()=>titleUpdater("Home"),5000); //5초 뒤 title은 Home으로 바뀜
  return(
    <div className="App">
      <div>Hi</div>
  
    </div>
  );

}

```


### 2.2 useClick


- react에 있는 모든 component는 reference element를 가지고 있음


```javascript
const useClick=(onClick)=>{
  if(typeof onClick !=="function"){
    return;
  }
  const element=useRef(); //모든 것들은 reference가 있어서 작동하는 것 
  useEffect(()=>{
    if(element.current){
      element.current.addEventListener("click",onClick);
      //component가 mount되었을 때 실행 
    }
    //component가 unmount 일 때, 이벤트가 발생한 후 정리할 필요가 있음 
    return ()=>{
      if(element.current(){
        element.current.removeEventListener("click",onClick);
      }
    };//componentWillunmount일때 return의 function 호출 
  },[]);//no dependency
  return element;
};
```


### 2.3 useConfirm & usePreventLeave 


- __useConfirm__ 
    - 사용자가 버튼을 클릭하는 작업을 하면(이벤트를 실행하기 전에) 메세지를 보여줌



```javascript
const useConfirm=(message="",onConfirm,onCancel)=>{
  if(!onConfirm || typeof onConfirm !=="function"){
    return;
  }
  if(onCancel && typeof onCancel !=="function"){
    return;
  }
  const confirmAction=()=>{
      if(window.confirm(message)){
        onConfirm();
        //ok를 누르면 실행 
      }
      else{
        onCancel();
        //취소를 누르면 실행
      }
  }
  return confirmAction;
};

const App=()=>{
  const deleteWorld=()=>console.log("Deleting the world")
  const abort=()=>console.log("Aborted")
  const confirmDelete=useConfirm("Are you sure?",deleteWorld,abort)
  return(
    <div className="App">
      <button onClick={confirmDelete}>Delete the world</button>
    </div>
  );
}
```
- 버튼을 누르면 confirmDelete가 실행 
- confirmDelete는 useConfirm실행해서 return confirmAction
- confirlAction 실행 



- __usePreventLeave__
    - window창을 닫을 때 아직 저장하지 않았냐고 물어봄


```javascript
const usePreventLeave=()=>{
  const listener=(event)=>{
    event.preventDefault();//표준에 따라 기본동작 방지
    event.returnValue=""; //chrome에서는 returnvalue설정이 필요함 
  }
  const enablePrevent=()=>window.addEventListener("beforeunload",listener);
  const disablePrevent=()=>window.removeEventListener("beforeunload",listener);
  return (enablePrevent,disablePrevent);
};

const App=()=>{
  const {enablePrevent,disablePrevent}=usePreventLeave();
  return(
    <div className="App">
      <button onClick={enablePrevent}>Protect</button>
      <button onClick={disablePrevent}>Unprotect</button>
    </div>
  );
}
```


### 2.4 useBeforeLeave

기본적으로 탭을 닫을 때, 실행되는 페이지


```javascript
const useBeforeLeave=(onBefore)=>{
  if(typeof onBefore !=="function"){
    return;
  }
  
  const handle=(event)=>{
    /*console.log로 event결과를 확인했을 때, 
    clientY는 마우스가 위로 벗어나면 음수. 아래로 벗어나면 양수 나옴*/

    const{clientY}=event;

    if(clientY<=0){onBefore();}//마우스가 위로 벗어난경우만 
  }; 
  //mouse가 위로 벗어나면 argument 함수가 실행돼서 "pls don't leave"가 console에 뜸

  useEffect(()=>{
    document.addEventListener("mouseleave",handle);
    return()=> document.removeEventListener("mouseleave",handle);
    //componentWillUnmount일 때 eventlistner 제거
  },[]);

};


const App=()=>{
  const begForLife=()=>console.log("pls don't leave");
  //mouse가 document를 벗어났을 때 실행시킬 함수 begForLife

  useBeforeLeave(begForLife);
  return(
    <div className="App">
      <h1>Hello</h1>
    </div>
  );
}
```


### 2.5 useFadeIn & useNetwork


- __useFadeIn__
    - 자동으로 서서히 나타나게 만듦


```javascript
const useFadeIn=(duration=1,delay=0)=>{
  
  const element=useRef();  

  useEffect(()=>{
    if(element.current){//.current는 우리가 원하는 DOM을 가리킴
      const{current}=element;
      current.style.transition=`opacity ${duration}s ease-in-out ${delay}s` ;//${} : 처리된 값을 문자열로 반환
      current.style.opacity=1;
    }
  },[])
  if(typeof duration !=="number" || typeof delay !=="number"){return;}
  
  return {ref:element,style:{opacity:0}}; //opacity:투명도
//component가 mount되면 Hello가 opacity,ease-in-out에 따라 서서히 나타남
};

const App=()=>{
  //주어진 reference를 fadeInH1,fadeInP에 줌
  const fadeInH1=useFadeIn(1,2); 
  const fadeInP=useFadeIn(5,10);
  return(
    <div className="App">
      <h1{...fadeInH1}>Hello</h1>
      <p{...fadeInP}>alala</p>
      //속성들이 참고돼서 reference,style이 들어옴 
    </div>
  );
};
```


- __useNetwork__
    - navigator가 online 또는 offline이 되는 것을 막아줌 


```javascript
const useNetwork =onChange=>{//network 상태가 바뀔때마다 function을 부름
  const [status,setStatus]=useState(navigator.onLine);//navigator.online:웹사이트가 온라인인지 아닌지 T/F나타냄
  
  const handleChange=()=>{
    if(typeof onChange=="function"){
      onChange(navigator.onLine);//onChange: 이벤트가 변경됐을 때 작동되는 함수 
    }
    setStatus(navigator.onLine);
  };

  useEffect(()=>{
    console.log("when");
    window.addEventListener("online",handleChange);
    window.addEventListener("offline",handleChange);
    return()=>{
      window.removeEventListener("online",handleChange);
      window.removeEventListener("offline",handleChange);
    };
  },[]);
  return status;
};

const App=()=>{
  const handleNetworkChange=(on)=>{ 
    console.log(on?"we just went online":"we are offline");
  }
  const onLine=useNetwork(handleNetworkChange);
  return(
    <div className="App">
      <h1>{onLine ? "Online":"Offline"}</h1>
    </div>
  );
};
```


### 2.6 useScroll & useFullscreen


- __useScroll__
  - 유저가 스크롤해서 무언갈 지나쳤을 때 나타나게 함 



```javascript
const useScroll=()=>{
  const[state,setState]=useState({
    x:0,
    y:0
  });
  const onScroll=()=>{
    console.log(window.scrollY);
    setState({y :window.scrollY},{x :window.scrollX});//측정한 window의 scroll으로 set 
  };
  useEffect(()=>{
    window.addEventListener("scroll",onScroll);
    return()=>{
      window.removeEventListener("scroll",onScroll);
    };
  },[]);
  return state;
};

const App=()=>{
  const {y}=useScroll();
  return(
    <div className="App" style={{height:"1000vh"}}>
      <h1 style={{position:"fixed", color:y>100?"red":"blue"}}>Hi</h1>
    </div>
  );
};
```


- __useFullscreen__
  - 이미지를 fullscreen으로 만드는 버튼


```javascript
const useFullscreen=(callback)=>{
  const element=useRef();
  const triggerFull=()=>{
    if(element.current){
      if(element.current.requestFullscreen){
        element.current.requestFullscreen();//fullscreen으로 만들어줌 
      } else if(element.current.mozRequestFullscreen){
        element.current.mozRequestFullscreen();
      } else if(element.current.webkitRequestFullscreen){
        element.current.webkitRequestFullscreen();
      } else if(element.current.msRequestFullscreen){
        element.current.msRequestFullscreen();
      }
      if(callback && typeof onFulls==="function"){
        callback(true); //onFulls함수에 인자로 true를 줘서 we are full 출력
      }
    }
  };
  const exitFull=()=>{
    document.exitFullscreen();
    
    if(document.current){
      if(document.current.requestFullscreen){
        element.current.requestFullscreen();//fullscreen으로 만들어줌 
      } else if(element.current.mozRequestFullscreen){
        element.current.mozRequestFullscreen();
      } else if(element.current.webkitRequestFullscreen){
        element.current.webkitRequestFullscreen();
      } else if(element.current.msRequestFullscreen){
        element.current.msRequestFullscreen();
      }
      if(callback && typeof onFulls==="function"){
        callback(false); //onFulls함수에 false를 줘서 we are small 출력
      }
    }
  };
  //
  return {element,triggerFull,exitFull}; 
  //내부함수 자체가 return값이라면 내부함수 외부에서 사용 가능 
};

const App=()=>{
  const onFulls=(isFull)=>{
    console.log(isFull?"We are full":"We are small");
  }
  const {element,triggerFull,exitFull}=useFullscreen(onFulls); 
  //내부함수를 사용하기 위해 define
 
  return(
    <div className="App" >
      <div ref={element}>
      <img ref={element} src="data:image/png;base64"/>
      </div>
      <button onClick={triggerFull} >Make fullscreen</button>
      <button onClick={exitFull}>Exit fullscreen</button>
    </div>
  );
};
```


### 2.7 useNotification

- __useNotification__
  - 알람이 실행되는 function


```javascript
const useNotification=(title,options)=>{
  if(!("Notification"in window)){
    return;
  }
  const fireNotif=()=>{
    if(Notification.permission !== "granted"){ //granted:사용자가 알림받길 허가
      Notification.requestPermission().then(permission=>{
        if(permission ==="granted"){
          new Notification(title,options);
        } else{
          return;
        }
      });
    } else{
      new Notification(title,options);
    }
  };
  return fireNotif;
};


const App=()=>{
  const triggertNotif=useNotification("Can I steal your kimchi?",
  {body:"I love kimchi"}
  );
  return(
    <div className="App" >
      <button onClick={triggertNotif}>Hello</button>
    </div>
  );
};
```



### 2.8 useAxios


- __Axios__
  - axios는 HTTP 클라이언트 라이브러리로써, 비동기 방식으로 HTTP 데이터 요청을 실행



```javascript
const useAxios=(opts,axiosInstance=defaultAxios)=>{//opts:configuration
    /*axiosinstance 요청 ,
    axios는 약간의 customization(맞춤화)과 configuration(구성)허용
    만약 얻지 못했다면 import한 axios 전달*/ 
    const[trigger,setTrigger]=useState(0);

    const[state,setState]=useState({
        loading:true,
        error:null,
        data:null
    });
    useEffect(()=>{
        axiosInstance(opts)
            .then(data=>{
                setState({
                    ...state,
                    loading:false,
                    data
                });
            })
            .catch(error=>{
                setState({...state,loading:false,error});    
            });
    },[trigger]);
  
    if(!opts.url){return;}//url이 필요하기 때문

    const refetch=()=>{//trigger일때도 useEffect실행시키기 위해 만든 함수
        setState({
            ...state,
            loading:true
        });
        setTrigger(Date.now());//Date.now():숫자를 줌
    };
    
    return {...state,refetch};
};


const App=()=>{
  const {loading,data,error,refetch}=useAxios({url:"http://localhost:3004/www.naver.com"});
  return(
    <div className="App" >
      <h1>{data&&data.status}</h1>
      <h2>{loading&&"Loading"}</h2>
      <button onClick={refetch}>Refetch</button>
    </div>
  );
};
```






