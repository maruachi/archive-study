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
