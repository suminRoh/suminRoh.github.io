---
layout: post
title: 6.3 Sharing Props Between Routes
color: rgb(239, 192, 80)
tags: [websolute,team,react,JSX]
---

#### 6.3 Sharing Props Between Routes


__route props__
- __`<About.js>`__ 
```javascript
function About(props){
    console.log(props);
}
```
- About console에서 4개의 props를 볼 수 있음 (history, location, match, staticContext)
   - 아직 about으로 전송되지 않은 react-router에 의해서 넣어진 props들


__Route에 있는 모든 router들은 props를 가짐__


- __`<Movie.js>`__
```javascript
function Movie({year,title,summary,poster,genres}){
    return (
        <Link to={{
            pathname:"/movie-detail",
            state:{
                year,
                title,
                summary,
                poster,
                genres
            }
        }}>
            <div className="movie">
                <img src={poster} alt={title} title={title}/>
                <div className="movie_data">
                    <h3 className="movie_title">{title}</h3>
                    <h5 className="movie_year">{year}</h5>
                    <ul className="genres">
                        {genres.map((genre,index)=>(
                            <li key={index} className="genres_genre">{genre}</li> 
                        ))}
                    </ul>
                    <p className="movie_summary">{summary.slice(0,140)}...</p>
                </div>
            </div>
        </Link>
        );
} 
```

- Movie들을 클릭하면 Link to를 이용해서 "/movie-detail"로 state에 적은 props들을 전송할 수 있음 


- __`<App.js>`__
    - `import About from "./routes/Detail";` 추가
    - function App에 `<Route path="/movie-detail" component={Detail}/>  ` 추가


- __`<Detail.js>` 생성__
```javascript
import React from 'react';

function Detail(props){
    console.log(props);
    return <span>
        Hello
    </span>;
}

export default Detail;
```
__console - location -state를 보면 보내려고 했던 props들이 movie-detail 스크린에 주어진 것을 확인할 수 있음 !!!__
