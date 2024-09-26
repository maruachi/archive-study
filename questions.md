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
- 추상 팩토리 패턴: 서로 다른 객체에 대한 생성을 추상 팩토리 객체의 구현체에게 위임하는 것
- [[디자인 패턴] 팩토리 메서드 패턴 vs 추상 팩토리 패턴](https://velog.io/@quden04/%EB%94%94%EC%9E%90%EC%9D%B8-%ED%8C%A8%ED%84%B4-%ED%8C%A9%ED%86%A0%EB%A6%AC-%EB%A9%94%EC%84%9C%EB%93%9C-%ED%8C%A8%ED%84%B4-vs-%EC%B6%94%EC%83%81-%ED%8C%A9%ED%86%A0%EB%A6%AC-%ED%8C%A8%ED%84%B4)

# 질문 리스트
- Visitor 패턴은 무엇일까?
- Iterator 패턴은 무엇일까?
  - [이터레이터 패턴](https://velog.io/@cham/Design-Pattern-%EC%9D%B4%ED%84%B0%EB%A0%88%EC%9D%B4%ED%84%B0-%ED%8C%A8%ED%84%B4-iterator-pattern)
- vue.js 빌드 방식은 어떻게 진행될까?
- vue.js에서 frontend api 관리를 실무에서 가장 많이 쓰는 방법은 무엇일까?
- javascript async와 await
- 이벤트 큐 vs 메세지 큐
  - [[Architecture] 메시지 브로커와 이벤트 브로커 - 메시지와 이벤트 그리고 RabbitMQ 와 Kafka](https://velog.io/@beberiche/Architecture-%EB%A9%94%EC%8B%9C%EC%A7%80-%EB%B8%8C%EB%A1%9C%EC%BB%A4%EC%99%80-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EB%B8%8C%EB%A1%9C%EC%BB%A4-%EB%A9%94%EC%8B%9C%EC%A7%80%EC%99%80-%EC%9D%B4%EB%B2%A4%ED%8A%B8-%EA%B7%B8%EB%A6%AC%EA%B3%A0-RabbitMQ-%EC%99%80-Kafka)
