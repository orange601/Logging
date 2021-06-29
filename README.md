# Logging
Log에 대해 알아보자

## java.util.logging

7개의 레벨로 구분된 로깅을 제공한다.    
- SEVERE (highest value)   
- WARNING   
- INFO   
- CONFIG   
- FINE   
- FINER   
- FINEST (lowest value)   

선언
````java
 private static final Logger logger = Logger.getLogger(MyApplication.class.getName());
````

장점   
- 원하는 기능이 아주 기본적인 수준이라면 외부 라이브러리 사용 없이 로깅이 가능하다. 

단점   
- 자바 1.4의 시점에 이미 잘 만들어진 log4j가 존재하였고 널리 쓰이고 있었다. 
- 굳이 기존 log4j 사용자들이 jul (java.util.logging) 으로 갈아탈 이유가 없었다.   
- 다른 라이브러리와 비교했을 때 퍼포먼스 (속도) 가 느리다.   
- Predefine 된 레벨의 개념이 명확하지 않다.    
- Debug의 뜻으로 사용된 FINE이 무엇을 뜻하는지 처음 보면 알 수 있을까?   
- FINE vs FINER vs FINEST의 차이점을 쉽게 구분할 수 없다.   
- 나만의 custom 레벨을 만들면 메모리 누수가 일어난다.   
- 타 라이브러리에 비해 기능이 부족하다.   
- 유연하지 않다.   

## log4j

참고로 2020년 현시점에서 다음 버전인 log4j2가 나왔기 때문에 새로운 프로젝트라면 log4j2를 사용하는 게 좋다. log4j2는 계속 기능 추가와 유지보수가 되고 있는 반면 log4j는 보다 적은 유지보수만을 가지고 있고 시간이 지남에 따라 점점 줄어들 것이다. 하지만 이미 log4j가 적용되어 있다면 굳이 업그레이드를 할 필요는 없다. 

log4j는 3개의 컴포넌트들로 이루어져 있다.
- logger: 데이터를 기록하는 역할
- appender: 데이터를 어디에 기록할지 정하는 역할 (파일, 콘솔, jdbc, smtp, 등)
- layout: 데이터를 어떤 스타일로 기록할지 정하는 역할

장점
- thread safe 하다.
- 퍼포먼스가 최적화되어있다.
- 여러 종류의 appender를 지원한다.
- jul에 비해 명확한 기준의 레벨을 가지고 있다: ALL, TRACE, DEBUG, INFO, WARN, ERROR and FATAL

선언
````java
private static Logger logger = LogManager.getLogger(MyApplication.class);
````


## slf4j
slf4j와 다른 라이브러리의 가장 큰 차이점은 slf4j는 wrapper라는 것이다. slf4j를 사용하여 설정에 따라 다른 로깅 라이브러리를 사용할 수 있게 된다.
Application code -> log4j -> 기록이던 것이 Application code -> slf4j (wrapper) -> log4j (혹은 log4j, logback, 등) 와 같이 2 티어로 변경되면서 원하는 destination (implementation) 을 사용할 수 있게 해 준다. 때문에 jul을 쓰다가 log4j로 갈아타는 등 마이그레이션 프로세스가 간단해진다. 
그래서 대부분 프로젝트는 slf4j의 사용을 추천한다. 
slf4j의 인기가 높아지다 보니 slf4j 자체에도 로깅 implementation을 포함하기도 했다. 

Implementation은 logback이나 log4j2를 사용할 것
logback이 다른 라이브러리보다 조금 더 핫 하며 log4j2가 조금 더 많은 기능을 제공한다.
