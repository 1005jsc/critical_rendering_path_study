# 웹브라우저가 렌더링하는 과정 알기


<br/>

## 이걸 이해하면?...

- 웹브라우저가 화면을 어떻게 그리는지 알 수 있다 
- 웹사이트를 더 빠르게 만들 수 있는 성능 최적화의 가장 기초적인 개념을 이해 할 수 있다 

## 목차 

1. 전체적인 흐름 이해
2. 네이버를 검색할때 브라우저에서 일어나는 일
3. Construction(DOM, CSSOM, Render Tree)
4. Operation(Layout, Paint, Composite)
5. Critical Rendering Path 지식을 기반으로한 성능최적화 방법 

<br/>
<br/>

## 1. 전체적인 흐름 이해

일단 웹브라우저가 어떻게 웹사이트 화면을 그리냐...를 영어로
'Critical Rendering Path'라고 한다 
이 Critical Rendering Path의 흐름은 아래의 그림과 같다. 



<br/>
<br/>

<div align="center"> <img src="/assets/1.svg" width="700px"  alt="그림 1. Critical Rendering Path의 흐름 도식화 "></div>

<br/>
<br/>
    <div align="center"> <span>그림 1. Critical Rendering Path의 흐름 도식화 </span></div>

<br/>
<br/>






설명보다 그림을 보는게 쉽고 더 빨리 익숙해질것 같아서 구체적인 설명에 앞서 Critical Rendering Path의 그림부터 미리 보여주었다.
<br/>
<br/>
간략하게 설명해보자면...
<br/>
<br/>

Critical Rendering Path는 크게 Construction과 Operation 이렇게 두가지로 나뉜다.
<br/>
<br/>
Construction에서는 DOM(DOM Tree)와  CSSOM(CSSOM Tree)를 합하여 Render Tree를 생성한다. 그리고 Operation에서 Construction에서 생성된 Render Tree를 가지고 요소들(Nodes)들을 어느위치에 어느 크기로 그릴 지 정하고(Layout), 레이어 그룹을 설정하고(Paint), 마지막으로 레이어들을 올바른 순서로 포개어 드디어 화면에 보여주게된다(Composite).  
<br/>
어쩌다 보니 이 글 전체의 내용을 요약해버렸다 이제부터는 위의 내용의 각 단계별로 차례차례 설명하고자 한다.  
<br/>



## 2. 네이버를 검색할 때 브라우저에서 일어나는 일 




<br/>
<br/>

<div align="center"> <img src="/assets/2.svg" width="700px"  alt="그림 2. 2.의 설명범위 "></div>

<br/>
<br/>
    <div align="center"> <span>그림 2. 2.의 설명범위 </span></div>

<br/>
<br/>



|1.|'www.naver.com'이라고 주소창에 침|
|:--:|:--:|
|2.|웹 브라우저가 DNS한테 naver의 IP주소를 물어봄|
|3.|브라우저는 DNS한테 받은 IP주소로 네이버 서버에 가서 데이터를 요청(http Request)|
|4.|네이버 서버는 응답으로 네이버 홈페이지 데이터를 브라우저에게 전송해줌(http Response) |


<br/>
<br/>


## 3. Construction(DOM, CSSOM, Render Tree)


<br/>
<br/>

<div align="center"> <img src="/assets/3.svg" width="700px"  alt="그림 3. 3. Construction의 설명범위"></div>

<br/>
<br/>
    <div align="center"> <span>그림 3. 3. Construction의 설명범위 </span></div>

<br/>

<br/>
<br/>

1. 받은 HTML을 W3C 웹 표준화기구의 명세에 따라 HTML을 해석한다. 이걸 **parcing**이라고 부른다. <br/><br/>이때 HTML의 각 element들을 Node로 변환한다.  그리고 변환된 Node들로 DOM Tree를 만든다 이 부분을 **scripting**이라고 한다. <br/><br/>

> DOM: hierarchical collection of Nodes<br/><br/>'노드들의 계층구조'라는 뜻이다.<br/><br/>
돔은 Node들의 '가족관계도'라고 이해하는게 제일 나을 듯<br/><br/>
공식정의는 'HTML이나 XML문서를 실체로 나타내는 API'라고 

<br/>

> Tip<br/>
    돔은 꼭 자바스크립트로만 제어할 수 있는게 아니다(python의 'beautiful soup'처럼 다른언어로도 돔을 제어할 수 있다.) 어떤 언더든 라이브러리만 갖춰저 있으면 되는데 그 이유는 돔이 API를 갖고 있기 때문이다. 





<br/>
<br/>

<div align="center"> <img src="/assets/4.svg" width="700px"  alt="그림 4. 'Dom Tree는 Node들의 가족관계도다' 라고 이해하면 된다  "></div>

<br/>
<br/>
    <div align="center"> <span>그림 4. 'Dom Tree는 Node들의 가족관계도다' 라고 이해하면 된다  </span></div>

<br/>
<br/>




2. .css를 만나면 html 파싱하던걸을 멈추고 css를 파싱하여 **CSSOM**을 만든다.
<br/>

3. DOM 과 CSSOM을 합쳐 **Render Tree**를 만든다
> css에 ```display: none;``` 이 있다면... <br/>
dom 에서는 해당 Node가 존재해도 css에서는 ```display: none;```이라고 했으니 Render Tree에서 아예 빼버리게 된다. 해당 Node는 Render Tree에서는 찾아볼 수 없게 된다. <br/>
Render Tree에는 사용자에게 궁극적으로 보여지는 노드들만 있다. 
<br/>
<br/>

지금 까지 (1), (2), (3) 을 묶어 **Construction**이라고 부른다. Construction과정의 최종적인 결과물은 Render Tree인데 이 Render Tree를 바탕으로 브라우저가 최종적으로 화면에 웹사이트를 띄우는 것에 대해서는 다음 장 **Operation**에서 설명하겠다.


<br/>
<br/>


## 4. Operation

<br/>
<br/>

<div align="center"> <img src="/assets/5.svg" width="700px"  alt="그림 5. 4. Operation의 설명범위"></div>

<br/>
<br/>
    <div align="center"> <span>그림 5. 4. Operation의 설명범위 </span></div>

<br/>

<br/>

operation: construction에서 얻어진 Render Tree를 바탕으로 브라우저가 실제 그림을 그린다. 

<br/>

4. ### layout(reflow)
    
<br/>
    Render Tree의 노드들을 정확하게 어떤 크기, 어떤 위치에 그려야 될지를 계산함.
<br/>

> 노드의 수가 많아지면 레이아웃이 느려진다.
  css나 노드들이 추가되거나 애니메이션이 거창하면 레이아웃이 완료되는 시간이 느려진다. 이를 **jank**라고 한다. 우리 눈은 애니매이션이 16ms 당 한장보다 느려지면 불쾌감을 느끼기 시작하는데 jank로 인해 20ms 30ms느려져 버리면 그만큼 유저들은 웹사이트를 보면서 불쾌함을 느끼게 되는 것이다.   
<br/>

5. ### paint(repaint)

<br/>

    그릴 노드들을 어느 레이어에 올릴지 구분한다

<br/>

6. ### Composite 
<br/>

    레이어들을 z-index에 맞춰 쌓고 브라우저에 띄운다. 
    드이더 브라우저에 웹사이트가 보이게 됨 

    >주의: Critical Path Rendering을 공부하면서 정말 애매했던 곳이 이 Composite부분이였다. 왜냐하면 어떤곳에서는 composite부분을 설명하고 또 어떤 곳에서는 composite부분을 설명 안하고 있기 때문이다. 일단 프론트엔드를 공부하는데 가장 기준이 되는 MDN에서는 설명을 **안하고 있다**. Composite부분이 정말로 존재하는지는 확실히 얘기 못하겠다. 그래도 일단 여기에는 설명을 해놓겠다. 혹시 저보다 자세히 아는 성님 계시면 훈수 부탁드립니다.  



<br/>
지금 까지 (4), (5), (6) 을 묶어 **Operation**이라고 부른다. 


<br/>
<br/>

## 5. Critical Rendering Path 지식을 이용한 성능개선하는 방법  




<br/>
<br/>




이제 Critical Rendering Path가 어떤 과정으로 동작하는 지는 알지만 진짜 중요한 것은 실전에 써먹는 법을 아는 것이다. 
Critical Rendering Path를 잘 이해하면 지금보다 더 성능좋은 웹사이트를 만들 수 있다. 
### 성능개선하는 법
일을 처음부터 다시 시키는 코드를 작성하지 않는다. 
이게 무슨 말이냐 

<br/>
<br/>

<div align="center"> <img src="/assets/6.svg" width="700px"  alt="그림 6. 4. Operation의 설명범위"></div>

<br/>
<br/>
    <div align="center"> <span>그림 6. Critical Rendering Path의 이해로 웹의 성능개선하는 법 </span></div>

<br/>

<br/>
<br/>

앞서 설명한 대로 순서는 (1) -> (6)으로 브라우저가 일을 한다. 
일을 처음부터 다시 시키는 코드는 브라우저에게 '너 (1)부터 다시 일해' 라고 말하는 코드를 말한다. 반면 일을 덜 시키는 코드는 최대한 (6)쪽에 가까운 코드를 말한다. '(5)~(6)만 일하면 돼'
예를 들어 div박스를 왼쪽으로 옮기는 애니매이션이 있다고 하자. 
left를 써서 박스를 움직이면, (4), (5), (6)과정을 모두 건드려야 된다. 하지만 translate을 쓰면 (6)과정 만으로 똑같은 결과를 얻을 수 있다. 내가 쓴 코드중에 left가 있었는데 translate로 바꿨다? 그럼 내 웹은 그만큼 더 빨라진, 더 성능개선이 된 웹이 된 것이다. 이런 css property들의 성능들은'css trigger'라는 사이트에서 확인 할 수 있다. 