# Why Go language?
- Go lang is basically used for backend side like java
- Go lang is designed as a language for mircoservices comparing to java because it doesn't require a huge machine (jvm) to consume a lot of memory and cpu.
- Go lang: syntax simple, nice concurrency, very fast
- [why go rddit question](https://www.reddit.com/r/golang/comments/11c9wv1/why_go/?rdt=45890)

# MySQL vs PostgreSQL 차이
- MySQL는 무난하게 웹서비스에서 사용하기 용이한 느낌이며 PostgreSQL은 대규모 데이터 서비스에 적합하다.
- [f-lab MySQL vs PostgreSQL 차이](https://f-lab.kr/insight/mysql-vs-postgresql?gad_source=1&gclid=Cj0KCQjwrp-3BhDgARIsAEWJ6Swmudqd9Tc7-UEkrDWxc927kTPxLRg7xZzZCCGRQb3Lb7mttN-IPuoaAms3EALw_wcB)

# Bulk inserting 어떻게 해야 잘했다고 할까?
- SQL 문법에 따라 bulk insert 성능을 개선할 수 있다.
- [Bulk Inserting - MySQL 다량의 데이터 넣기](https://dev.dwer.kr/2020/04/mysql-bulk-inserting.html)
- [[Oracle] 대량 데이터 인서트 하기 (Bulk Data Insert)](https://itsuit.tistory.com/116)

# 옵저버 패턴은 무엇일까?
- 어떤 객체의 상태가 변할 때 그와 연관된 객체 들에게 알림을 보내는 디자인 패턴
  - Observable: 옵저버를 관측하는 객체 `notifyObservers()`로 옵저버에게 상태 변경을 인폼한다.
  - Observer: 상태 변경에 대한 기능 처리를 담당하는 옵저버 객체 일반적으로 `observer`로 처리한다.
- java9에서 `Observable` 인터페이스가 제외 됐다.
  - [java official doc link about this](https://bugs.openjdk.org/browse/JDK-8154801)
  - 서비스 제공보다는 자체 구현이 낫다는 판단
  - Observable이 클래스로 정의되어 있어서 다른 클래스 상속이 불가능
  - 동기화 처리가 안 되어 있어서 Thread-safe 문제가 존재
- [옵저버 패턴이란?](https://pjh3749.tistory.com/266)

# State 패턴은 무엇일까?
- State에 따라 행위와 상태 변화를 정의하는 디자인 패턴
- State interface는 `speak()`와 `wait()`의 기본 구성을 가진다.
  - `speak()`: 상태에 대한 기능 정의
  - `wait()`: 상태 변화를 담당
- Context 클래스는 State를 가지고 `speak()`와 `wait()`을 감싸는 구조를 가진다.
- 이를 보면, 전략패턴과 유사해보이지만 상태 메소드에 context 자신을 넘기는 부분이 다르다.
  - 객체 관계를 보면 전략 패턴에서 각 전략들은 서로와 컨텍스트를 모르지만, 상태 팬터에서 각 상태들은 서로하고 컨텍스트를 모두 안다.
- [유튜브 영상 자료](https://www.youtube.com/watch?v=278vXJkgXoY)

# 오픈 소스 기여하는 방법
- issue나 스스로 찾은 불편함을 개선하여 기여한다.
- 난이도 낮은 오픈소스 기여 방법
- CI/CD, Dockerfile 코드 개선
  - 테스트 코드 추가 (프로젝트에 coveralls 툴이 있다면, 테스트 코드가 없는 methods를 찾기 수월)
  - 초보자용 issue 키워드 참조
    - `good-first-issue`, `first-timer-only`, `low-haning-fruit` 
- [open source 기여 입문 팁](https://velog.io/@skynet/%EC%98%A4%ED%94%88%EC%86%8C%EC%8A%A4-%EA%B8%B0%EC%97%AC-%EC%9E%85%EB%AC%B8)
- [open source 겨여 방법](https://velog.io/@youngjun_10/Github-%EC%98%A4%ED%94%88%EC%86%8C%EC%8A%A4-%EA%B8%B0%EC%97%AC%ED%95%98%EA%B8%B0)

# Adapter 패턴은 무엇일까?
- 인터페이스가 호환되지 않을 때 인터페이스를 연결해주는 디자인 패턴
- Adapter, Adaptee, Target interface가 존재한다.
- 클라이언트에서 Target Interface로 Adaptee의 인터페이스를 통일 시키고 싶을 때 Adapter를 중간에 두어 Target Interface인 척 사용할 수 있다.
- 구현은 Adapter는 Target Interface를 상속 받아서 Adapter 내부에서 Adaptee의 기능을 Target Interface 기준으로 재설계 해주면 된다.

# BFS, DFS 알고리즘을 웹 서비스 실무에서 사용하는 경우가 있을까?
- 소셜 네트워크 서비스에서 그래프 탐색, 카테고리 트리 /  메뉴 구조 / 문서 구조 등의 트리 구조 탐색, 네비게이션 경로 탐색, 의존성 작업 스케줄링, 크롤러
- [chat GPT: BFS, DFS 알고리즘을 웹 서비스 실무에서 사용하는 경우가 있을까?](https://chatgpt.com/share/66f000ca-05ec-800f-ac00-c522e5675432)

# 템플릿 패턴 vs 템플릿 콜백 패턴 vs 전략 패턴
- 세 가지 패턴은 기본적으로 논리 흐름을 담당하는 Context(or Template)와 특정한 기능을 정의한 Strategy로 나뉜다.
- 이런 공통점이 있지만 3가지의 구분은 전략을 객체 지향에서 Context와 Strategy를 어떤 단위로 구현할 것인가로 갈린다.
- 템플릿 패턴
  - 객체 자체가 Context를 의미하며 특정 메소드가 Strategy가 된다.
  - Strategy는 전략에 따라 변경되어야 하기 때문에 추상 클래스로 선언되며 상속을 통해서 전략을 정의할 수 있다.
- 텀플릿 콜백 패턴
  - 하나의 특정 메소드가 Context가 되며 넘겨주는 콜백 함수인 매개변수가 전략에 해당한다.
  - 템플릿 패턴과의 차이는 Context가 메소드 level로 내려온다는 것이다.
- 전략 패턴
  - 전략 패턴은 템플릿 패턴 처럼 하나의 객체가 Context를 대표한다.
  - 전략을 인터페이스나 상속으로 구현할 수 있으며 Context 객체는 전략을 합성을 통해 가지고 있다.
  - 의존성 주입을 통해서 어떤 전략을 사용할지 결졍해줄 수 있다.
 
# 왜 자바에서는 추상클래스는 다중상속을 허용하지않고 인터페이스는 다중상속을 허용하나요?
- 상속의 모호성으로 추상클래스는 다중 상속을 허용하지 않는다.
- 상속의 모호성은 필드의 이름 중복을 통해서 나온다.
- 인터페이스에서 메소드의 중복은 클라이언트 측에서 사용하는 것과 오버라이드 재정의 시에도 문제가 되지 않는다.
- [OKKY 질문](https://okky.kr/questions/1515115?topic=questions&page=2)
 
# node.js vs spring
- http 요청에 대해서 node.js는 single thread로 처리하고 spring은 별도의 thread로 만들어서 처리한다. 이는 응답성에 큰 차이를 발생시킨다.
- 대신에 node.js는 가볍다는 장점이 있다. 이는 k8s와 결합하여 나온 MSA 아키텍처에서 node.js가 spring에 비해선 유리하다.
  - 트래픽이 몰릴 경우에는 node.js 서버만 늘려주면 된다.
  - 아무것도 하지 않는 Java Spring은 400MB 가량의 Memory를 사용하는데 반해, Node.js는 25MB 정도의 Memory 만을 사용
- 이런 인프라의 개선 뿐만 아니라 테스트 프레임워크(Jest, Mocha), 타입 제공(Typescript), DI/IoC 기반 개발(nest.js) 도입으로 백엔드 영역이 좋아지는 상황
- 하지만, 아직 java spring의 안정성을 대체하기엔 이르다. 안정화 작업이 필요!
- [Node.js VS Java Spring](https://medium.com/naverfinancial/node-js-vs-java-spring-c4699565918e)

# Command 패턴은 무엇일까?
- 특정 객체의 기능 수행을 Command와 Invoker 위임하여 실행할 수 있도록하는 디자인 패턴
- Command의 실행은 Invoker가 담당한다.
- 실 사용 예시
  - 작업을 큐에 넣고 실행해야 할 때
  - Undo/Redo 기능 구현
  - 작업 실행의 추상화 필요
  - 트랜잭션 기반 시스템에서 여러 작업을 하나로 묶을 때
  - UI 작업의 캡슐화
- [Chat GPT 답변](https://chatgpt.com/share/66f1e2aa-9590-800f-bea1-a13ed184f8ac)

# javascript export, import, 함수 선언
1. export와 import
- js 파일은 하나의 모듈이다.
- 모듈을 다른 모듈에서 참조가 가능한데, 참조하는 방식이 export, import를 통해서 이루어진다.
- default export와 named export가 존재하고 모듈 당 딱 한 개의 default export만 존재해야 한다.
- import 시에 default export는 { }로 감싸는 거 없이 참조 가능하며, named export된 요소들은 { } 내에 선언하여 가져올 수 있다

```js
//person.js
const person = { name: 'Max'};

export default person

//utility.js
export const clean = () => { ... };
export const baseData = 10;

//app.js
import person from './person.js'
import prs from './person.js'

import {baseData} from './utility.js'
import {clean} from './utility.js'
```

2. 함수 선언

```js
function add(a, b){
  return a+b;
};

var add = function (a, b){
  return a+b;
};

var add = (a, b) =>{
  return a+b;
};
```

# javascript 특징을 보여주는 문법이 무엇이 있을까?
- null vs undefined
  - null은 값이 비어 있다는 것을 의도한 것이도 undefined는 의도와 무관한 것이다.
- in loop vs of loop
  - in loop: 객체의 열거 가능한 속성을 순회할 때 사용
  - 반복 가능한 객체의 값을 순회할 때 사용
- async와 await 그리고 Promise 객체
  - async 함수는 항상 Promise 객체를 반환하고 await 시에 Promise가 해결될 때까지 기다린다. 
- Optional chaining
  - obj?. 방식으로 ?와 함께 chaining하는 문법으로 체이닝 시에 객체의 속성이 존재하지 않으면 undefined을 반환한다.
- Spread syntax
  - `...` 배열이나 객체의 값을 펼칠 때 사용한다.
- 세미콜론 자동 삽입 기능
  - 세미콜론을 붙이지 않아도 javascript는 잘 동작한다. 하지만 이로 인해 예기치 못한 문제가 발생할 수 있으므로 세미콜론은 꼭 붙이는 게좋다. 

# webpack, vite와 같은 모듈 번들러는 왜 사용할까?
- 모듈별로 나눠져 있는 html, css, js 파일을 하나의 파일로 합쳐주는 역할을 수행한다.
- 웹 서비스 시에 모듈별로 파일을 나워서 서비스하게 되면 네트워크 요청이 많아져 성능 저하를 가져다 주기 때문에 모듈 번들러로 하나의 파일로 합친다.
- webpack은 일반적으로 build, vite는 dev-server를 위해서 사용한다. 그 이유는 vite가 요청에 필요한 모듈만 번들링을 시도하기 때문에 반응이 빠르기 때문이다.
  - [회사 vue 프로젝트 vite 적용하기](https://eddie-sunny.tistory.com/107)

# 팩토리 메서드 패턴 vs 추상 팩토리 패턴
- 팩토리 메서드 패턴: 하위 객체의 생성을 인터페이스 팩토리 구현체에게 위임하는 것
- 추상 팩토리 패턴: 서로 다른 객체에 대한 생성을 팩토리 객체의 구현체에게 위임하는 것
- [[디자인 패턴] 팩토리 메서드 패턴 vs 추상 팩토리 패턴](https://velog.io/@quden04/%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4-%ED%8C%A9%ED%86%A0%EB%A6%AC-%EB%A9%94%EC%84%9C%EB%93%9C-%ED%8C%A8%ED%84%B4-vs-%EC%B6%94%EC%83%81-%ED%8C%A9%ED%86%A0%EB%A6%AC-%ED%8C%A8%ED%84%B4)

# vue.js vue-router 개념
- 라우터(router): 네트워크간에 데이터를 전송하는 장치
- 라우팅(routing): 최적의 경로를 찾는 프로세스
- 라우트(route): 서로 다른 네트워크 간 데이터를 전송하고 전송한 데이터를 받는 경로
- vue-router는 client-side routing의 개념을 가지고 있다. Server-side routing은 url 별로 page를 mapping 시키는데, vue.js router는 브라우저 내에서 page 맴핑을 구현할 수 있게 해준다.
- [stack overflow 답변](https://stackoverflow.com/questions/10190215/what-is-client-side-routing-and-how-is-it-used)

# vue.js axios repaonse.data 내의 List (spring) 값을 반응형 객체에 할당하기
```js
export default setup(){
  views = ref(Array())
  onMounted(async ()=>{
    const response = await getSomethingFromServer(); // getSomethingFromServer: Rest API
    const views.value = response.data.views
  })
  return {
    views
  }
}
```
- getSomethingFromServer() 함수는 api를 wrap하고 있다.
- onMounted에서 api 호출이 이루어지는 이유는 setup 함수 내에서 promise로 객체를 생성할 수 없기 때문이다. (why?)
- response.data 내 List 값을 Array로 변환해서 제공하기 때문에 ref(Array()) 값으로 반응형 객체를 정의한다.
- Array()로 선안하고 Array() 내부 변경점이 발생해도 vue는 반응형 상태를 내부 깊숙이 추적하으로 변경된 상태를 반여할 수 있다.
- [[VUE] 배열에 값 추가 또는 삭제하기(push, splice)](https://ssjeong.tistory.com/entry/VUE-%EB%B0%B0%EC%97%B4%EC%97%90-%EA%B0%92-%EC%B6%94%EA%B0%80-%EB%98%90%EB%8A%94-%EC%82%AD%EC%A0%9C%ED%95%98%EA%B8%B0push-splice)

# vue.js reactive vs ref
- 객체, 배열 그리고 Map이나 Set과 같은 컬렉션 유형에만 작동함. string, number 또는 boolean 과 같은 기본 유형에는 사용할 수 없다.
- string, number 또는 boolean 과 같은 기본 유형에도 사용 가능하다.
- [reactive vs ref 성능차이](https://changjoo-park.github.io/write/guide/vue/performance-ref-reactive.html#_10%E1%84%86%E1%85%A1%E1%86%AB%E1%84%80%E1%85%A2-%E1%84%89%E1%85%A2-%E1%84%83%E1%85%A6%E1%84%8B%E1%85%B5%E1%84%90%E1%85%A5-%E1%84%86%E1%85%A1%E1%86%AB%E1%84%83%E1%85%B3%E1%86%AF%E1%84%80%E1%85%B5)

# javascript concat vs push
- concat은 배열 연결
- push는 요소 추가
- push 시에 Spread syntax(...)를 사용하면 concat과 동일한 기능을 구현 가능하다.

# javascript [] vs new Array()
- 둘 다 배열 타입과 관련한 차이가 없다
- []가 선호도가 높은데 간결성과 성능 우수성 그리고 문법 안정성 때문이다.
- [new Array() vs []](https://stackoverflow.com/questions/1800594/in-javascript-why-is-preferred-over-new-array)
- [성능 비교](https://stackoverflow.com/questions/7375120/why-is-arr-faster-than-arr-new-array)

# vue.js vuetify에서 container 내의 요소를 배치하는 방법
- v-container는 layout을 잡을 때 많이 사용한다.
  - v-row와 v-col로 내부 레이아웃 배치를 한다.
- v-responsive는 미디어 콘텐츠가 다양한 화면 크기에 맞춰 반응형으로 조정

# vuetify card vs panel
- Panel: 여러 UI 요소를 그룹화하고 논리적인 구분을 제공하는 컨테이너로 사용되며, 비교적 단순한 디자인을 가집니다.
- Card: 정보를 독립적으로 표시하고 좀 더 시각적으로 강조된 스타일을 가지며, 개별적인 정보 블록을 표시하는 데 적합합니다.
- [chat-gpt panel vs card](https://chatgpt.com/share/66f7efbe-aa20-800f-b11e-d00a44cf622e)

# vue.js MVVM pattern
- MVVM: Model, View, ViewModel
- Model: 데이터와 비지니스 로직을 처리
- View: 사용자에게 보여지는 화면을 처리
- ViewModel: View를 표현하기 위해서 만들어진 View만을 위한 Model -> View와 1대1 Mapping 관계에 있다.
- Vue는 ViewModel 집중적인 프레임워크다. directives와 DOM Listeners을 통해 ViewModel을 구현하며 Model과 View를 연결해주는 역할을 한다.
- ViewModel은 Command 패턴으로 ViewModel에 대한 처리를 추상화여 일괄처리

# MVVM vs MVC

![image](https://github.com/user-attachments/assets/6820076b-792c-48e2-897b-31d8f64ae766)

# Python Garbage Collector 동작 방식

- 기본적으로 레퍼런스 카운팅하여 갈비지 컬렉션을 수행
- 레퍼런스 카운팅이 불가능한 순환 참조 또한 감지하여 갈비지 컬렉션을 수행한다.
- generation(세대), threshold(임계값)로 가비지 컬렉션의 실행 기준으로 삼는다.
  - 0세대, 1세대, 2세대로 나뉘며 0세대가 young 2세대로 갈수록 old이다. 0세대를 더 자주 갈비지 컬렉션 하도록 되어 있다.
  - [Python GC가 작동하는 원리](https://www.winterjung.dev/python-gc/)

# user mode vs kernel mode
- user mode: application 코드는 보통 유저모드에서 실행되며, 유저모드에서 접근할 수 있는 자원은 한정적이다.
- kernel mode: kernel mode에서는 모든 자원에 접근 명령 할 수 있다.
- user mode와 kernel mode는 system call을 통해서 소통한다.

# 질문 리스트
- Visitor 패턴은 무엇일까?
- Iterator 패턴은 무엇일까?
  - [이터레이터 패턴](https://velog.io/@cham/Design-Pattern-%EC%9D%B4%ED%84%B0%EB%A0%88%EC%9D%B4%ED%84%B0-%ED%8C%A8%ED%84%B4-iterator-pattern)
- 데이터베이스 데이블 Migration 방법이 무엇이 있을까?
- Session clustering
  - [세션 클러스터링이란?](https://velog.io/@mirrorkyh/%EC%84%B8%EC%85%98-%ED%81%B4%EB%9F%AC%EC%8A%A4%ED%84%B0%EB%A7%81%EC%9D%B4%EB%9E%80)
- 요구사항 명세서 작성 방법
  - [요구사항 명세서 (Requirements Specification)](https://mklab-co.medium.com/%EC%9E%91%EC%84%B1%EB%B2%95-%EC%9A%94%EA%B5%AC%EC%82%AC%ED%95%AD-%EB%AA%85%EC%84%B8%EC%84%9C-requirements-specification-ad3533d6d5b8)
- 이벤트 큐 vs 메세지 큐
  - [[Architecture] 메시지 브로커와 이벤트 브로커 - 메시지와 이벤트 그리고 RabbitMQ 와 Kafka](https://velog.io/@beberiche/Architecture-%EB%A9%94%EC%8B%9C%EC%A7%80-%EB%B8%8C%EB%A1%9C%EC%BB%A4%EC%99%80-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%B8%8C%EB%A1%9C%EC%BB%A4-%EB%A9%94%EC%8B%9C%EC%A7%80%EC%99%80-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EA%B7%B8%EB%A6%AC%EA%B3%A0-RabbitMQ-%EC%99%80-Kafka)
- 연구와 개발의 차이
  - [개발과 연구의 차이](https://www.ibric.org/bric/community/popular-sori.do?mode=view&articleNo=9569214&title=%EA%B0%9C%EB%B0%9C%EA%B3%BC+%EC%97%B0%EA%B5%AC%EC%9D%98+%EC%B0%A8%EC%9D%B4#!/list)
  - [개발자와 연구자 - 파트타임 개발자가 살아남는 법](https://lifidea.tistory.com/entry/%EA%B0%9C%EB%B0%9C%EC%9E%90%EC%99%80-%EC%97%B0%EA%B5%AC%EC%9E%90-%ED%8C%8C%ED%8A%B8%ED%83%80%EC%9E%84-%EA%B0%9C%EB%B0%9C%EC%9E%90%EA%B0%80-%EC%82%B4%EC%95%84%EB%82%A8%EB%8A%94-%EB%B2%95)
- # vue.js에서 frontend api 관리 방법
  - [[번역]Vue 프로젝트를 위한 API Factories 설계](https://velog.io/@taewo/%EB%B2%88%EC%97%ADVue-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%EB%A5%BC-%EC%9C%84%ED%95%9C-API-Factories-%EC%84%A4%EA%B3%84)
  - [[Vue의 현실] API Interface 만들기](https://velog.io/@uriyang_/Vue%EC%9D%98-%ED%98%84%EC%8B%A4-API-inteface-2)
