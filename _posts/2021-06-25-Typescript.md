---
layout: post
title:  Typescript
color: rgb(239, 192, 80)
tags: [websolute,team,typescript]
---

### Typescript
- javascrpit의 superset ( comepile 시, javascript로 compile 됨 )
- 예측 가능한 언어이며 읽기 쉬운 코드로 javascript를 더 잘 사용할 수 있게 해줌 


#### Setting Typescript Up
- tsc: ts파일인 코드를 컴파일해서 .js와 .js.map을 만들어줌 
```javascript
 "scripts": {
    "start":"node index.js",
    "prestart": "tsc"

  }
```
-> `yarn start`를 실행 시, prestart의 tsc가 먼저 실행되고 start의 node index.js가 만들어지고 실행하게 됨 


#### Interface
```typescript
interface Human{
    name:string;
    age:number;
    gender:string;
}
//interface는 js에서 작동안함 

const person={
    name:"nicolas",
    age:22,
    gender:"male"
    
}
const sayHi=(person:Human):string=>{
    return (`Hello ${person.name}, you are ${person.age} you are a ${person.gender}!`);
}
```
-> interface를 통해 object의 type을 정의해주고 간편하게 함수에서 인자 하나로 이용 가능 


#### Class
- js에서 interface를 사용하고 싶을 경우, class 사용 

```typescript 

class Human {
    public name : string;
    public age:number;
    public gender:string;
    constructor(name:string,age:number,gender?:string){ //class가 시작할때마다 호출됨 
        this.name=name;
        this.age=age;
        this.gender=gender;
    }

}

const lynn=new Human("Lynn",18,"female");
```
- public 변수들은 컴파일시 ,js 코드 상에서는 나타나지 않음 (js에서 지원 X)
- 변수 private 시, class외부에서는 이용할 수 없어 속성을 보호할 수 있게 해줌 