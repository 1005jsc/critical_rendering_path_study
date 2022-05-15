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


## 1. 전체적인 흐름 이해

일단 웹브라우저가 어떻게 웹사이트 화면을 그리냐...를 영어로
'Critical Rendering Path'라고 한다 
이 Critical Rendering Path의 흐름은 아래의 그림과 같다. 









그림 1. Critical Rendering Path의 흐름 도식화 

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





    그림2. '네이버'를 검색할때   


1. 네이버를 검색함
2. 










