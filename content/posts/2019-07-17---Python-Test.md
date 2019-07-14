---
title: Python & Django Test 
date: "2019-07-18T08:46:37.121Z"
template: "post"
draft: false
slug: "/posts/python/test"
category: "Python & Django Test"
tags:
  - "Python3"
  - "Wecode"
  - "Test"
description: "Python & Django Test"

------

시스템을 구현하거나 코딩을 했다면 잘 작동하는지, 원하는대로 개발되었는지 등을 알아보기 위해 테스트를 꼭 해야 한다고 한다. 하지만 한국사회는 빠르게 뭔가를 해야 된다는 고정관념이 있기 때문에 테스트를 소홀히 생각하고 있는 듯 하며, 나 역시도 테스트의 중요성을 아직까지는 크게 못 느끼고 있었다. 이번 포스팅은 테스트가 무엇인지, 어떤 방법이 있는지, 파이썬과 장고로는 어떻게 해야 하는지 등을 알아보고 제일 중요한(?)유닛테스트에 대해 자세히 포스팅해보려 한다.

###테스트란?
소프트웨어 테스트는 시스템 또는 애플리케이션이 주어진 조건하에 기대되는(개발목적에 맞게) 결과를 제공하는 가를 평가하는 것이다. 주어진 조건이란 정상적인/비정상적인 입력 모두를 의미하며, 각각의 경우에 기대되는 결과를 보여야 한다. 

사전적 의미로는 stakeholders(이해관계자)들에게 제품이나 서비스의 품질정보를 제공하는 것을 의미하며, 이는 제품이 사용자의 요구와 일치하는지, 개발자의 설계대로 동작하는 지를 동시에 측정하는 평가활동이라고 보면 된다. debugging과 같이 생각할 수 있지만, 디버깅은 발견된 결함을 수정하기 위한 활동이고, 테스팅은 의도에 맞지 않게 시스템이 돌아가는지 등 숨어있는 결함을 발견하는 활동이라고 구별된다.(디버깅은 결함을 분석하는 테스팅의 활동 중 일부)

###테스트의 종류
[테스트의 종류](https://eehoeskrap.tistory.com/14)는 여러가지가 있지만 시스템을 테스트할 때 크게 3가지 방법으로 나눌 수 있다.

1. UI Testing / End-To-End Testing(테스트 비중: 10%)
2. Integration Testing(테스트 비중: 20%)
3. Unit Testing(테스트 비중: 70%)  

> 총 테스트를 각각의 요소로 나눈다면 위와 같이 나누는게 효율적이다.  
-----

###백엔드로서의 테스트
API, Funtions 등에 Mock을 대입하여 기대하는 값을 리턴하는지 확인   
난이도나 공수가 낮아 


###1. UI Testing / End-To-End Testing  
프로젝트에서 하나의 Featuer(기능)을 구현한 후 웹사이트를 사람이 하는 테스트같이 프로그램이 마우스 클릭이나 키보드 입력 등을 UI를 통해서 테스트 하는 방법을 말한다. (Selenium 등)
>> E2E testing: 사용자의 입장에서 사람이 직접 할 경우 많은 공수(노동의 수치)가 발생하며, 사람이 직접 눈으로 확인할 수 밖에 없는 것들을 테스트한다.

###2. Integration Testing(통합 테스팅)
개별 기능들 혹은 모듈들을 결합했을 때 인터페이스 및 통합 구성 요소 또는 시스템간의 상호 작용에서 결함을 노출하기 위해 수행된다.  
1) 구성 요소 테스트: 인터페이스의 결함과 통합 구성 요소 간의 상호작용을 노출시키기 위해 수행 된 테스트
2) 시스템 통합 테스트: 시스템과 패키지의 통합 테스트, 인터페이스 테스트 등  
(블랙박스 테스트, 화이트박스 테스트, 그레이 박스 테스트 등이 사용된다고 한다.)   

###3. Unit Testing(단위 테스트)
* 테스트할 수 있는 가장 작은 단위에서의 테스트를 의미하며(함수), 테스트를 위한 코드를 짜야 하기 때문에 귀찮고, 많이 소외시 되고 있다.  
* 위에서 말했던것과 같이 정상적인지/비정상적인지 등을 최소한의 단위에서 확인하는 것이며, 객체지향프로그램인 파이썬에서느 추상화한 클래스, 파생된 자식 클래스에 속하는 메소드 들을 하나하나 테스트 한다고 보면 된다.  
* (절차지향? 프로그래밍에선 프로그램, 함수, 절차 등을 테스트 한다고 하지만 여기선 다루지 않을 내용이라 알고만 있자.)
* 일반적으로 소프트웨어 개발자나 동료가 진행하며, 독립적인 테스터가 수핼할 떄도 있다.  

####유닛테스트의 장점
1. 코드의 변경/유지보수에 대한 자신감을 높힐 수 있다.  
좋은 유닛테스트를 만들고, 코드가 변경될 때 마다 실행될 경우 결함 을 즉시 잡을 수 있음  
2. 코드를 재사용하기 쉽게 할 수 있다. 
3. 유닛테스에서 발생된 버그를 수정하는 비용은 다른 테스팅에서 발견된 버그에 비해 상대적으로 상당히 적은 비용이 발생한다.  
4. 테스트가 실패하면 최신 변경사항만 디버깅하면 된다. 
5. 코드가 더 안정적이다.(추가 설명은 필요 없을 듯 하다.)

####유닛테스트 팁
* 언어에 맞는 도구나 프레임워크 찾기
* 시스템의 동작에 영향을 미치는 테스트에 중점을 두자.  
* 그 날의 코딩을 시작하기 전과 끝내기 전에 풀 테스트 슈트를 돌리자.  
* 모듈 혹은 하나의 함수가 모두 독립적인 수행결과를 출력할 수 있도록 작성. 
* 코딩한 후 저장할 때마다 자동으로 돌려야 한다.  
* 개발 중에 다른 일을 해야 한다면, 일부로 잘못된 테스트를 작성하는 것도 좋음.
* 모두가 공유하는 저장소에 코드를 넣기 전에 자동으로 모든 테스트를 수행하도록 하는 훅을 구현하는 것이 좋음
* 테스트가 빠르게 돌 수 있도록 만들기 위해 노력해야 함 => 테스팅을 하는데 몇 밀리세컨드 이상의 시간이 걸린다면 개발속도가 느려지거나 테스트가 충분히 자주 수행 할 수 없다.(무거운 테스트는 따로 분리하여 별도의 테스트 슈트를 만들어 두고 자업을 걸어두자.)
* 테스트에 사용되는 함수는 직접 호출되지 않기 때문에 가능한 길고 서술적인 이름을 사용해야 한다.  
* 새로운 개발자들을 위한 안내서로 쓴다.(이미 구현된 코드에서 작업해야 할 경우 관련 테스트 코드를 돌려보고 읽어보는 것이 가장 좋은 시작점일 경우가 많다.(어디가 문제인지, 수정하기 어려운 부분이 어디인지, 막히는 곳이 어디있는지 등을 발견할 수 있음))

####파이썬 유닛테스트에서 사용되는 것들
[TestCase](https://docs.djangoproject.com/en/dev/topics/testing/overview/) : 유닛테스트 프레임 워크의 테스트 조직의 기본 단위
[Client](https://docs.djangoproject.com/en/dev/topics/testing/tools/)  
[Mock](https://blog.leop0ld.org/posts/about-mocking/) : 오브젝트들을 동적으로 대체하고 사용결과를 확인하기 위한 다양한 기능들을 제공, 의존성이 잇는 것들을 실제로 실행시키지 않고 호출여부, 인터페이스를 확인할 수 있다.

다음 포스팅은 TestCase와 Client를 이용해 직접 작성해보고 포스팅 해보겠다.  

참고 사이트  
[Software Testing Fundamentals](http://softwaretestingfundamentals.com/integration-testing/)

[파이썬 테스트 시작하기(유튜브)](https://www.youtube.com/watch?v=hAUjItE42cY)
[파이썬 테스트 시작하기(PPT)](https://www.slideshare.net/hosunglee948/python-52222334)  
위 두 자료는 2015년 파이콘에서 이호성 개발자님께서 발표에 썻던 자료와 발표를 한 영상을 담고 있고, 기초 부분을 이해할 때 참고하기 좋은 자료인 듯 보인다.