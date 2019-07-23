---
title: Python Class - 사용법
date: "2019-07-17T08:46:37.121Z"
template: "post"
draft: false
slug: "/posts/python/class2"
category: "Python Class"
tags:
  - "Python3"
  - "Wecode"
  - "Class"
description: "Python Class - How to use"

---

## 클래스 사용법
[파이썬 공식문서](https://docs.python.org/ko/3/tutorial/classes.html)

전 시간에 클래스에 대해 알아봤는데, 이번엔 어떻게 써야 할지에 대해 자세히 알아보고자 한다.  

클래스 객체는 어트리뷰트 참조(클래스 내부의 변수나 함수에 접근)와 인스턴스(실체화)를 만든다

메소드에 `__`의 사용은 2가지로 나뉜다.  
1) init과 같은 특별한 메소드를 사용해야 할 때(메소드의 종류나 사용법이 궁금하다면 이 [페이지](https://corikachu.github.io/articles/python/python-magic-method)를 참고하거나, 구글링을 하면 더 많은 정보가 나와있다.)  
2) 맹글링을 위한 메소드  
* 맹글링이란 컴파일러나 인터프리터가 변수나 함수명을 일정한 규칙에 의해 변형시키는 것을 의

```
#Attribute
classname.variable          #변수일 경우 호출 방법
classname.function(input)   #메서드(함수)일 경우 호출 방법

#Instance
#x = classname()            #instance 생성
```

```
class Flight:

    def __init__(self):
        print('init')
        super().__init__()

    def __new__(cls):
        print('new')
        return super().__new__(cls)

    def number(self):
        return 'SN060'

from airtravel import Flight
f = Flight()
new
init
```

```
calss Dog:                    # CamelCase로 첫글자 대문자로 클래스 선언
    
    kind   = 'canine'           # 변수선언(공유됨)
    tricks = []

    def __init__(self, name): # 인스턴스에 따라 값이 변함
        self.name = name

    def add_trick(self, trick):
        self.tricks.append(trick)

>>> d = Dog('Fido')   # d 인스턴스 생성
>>> e = Dog('Buddy')  # e 인스턴스 생성
>>> d.kind            # 변수 kind 호출
'canine'
>>> e.kind            # 변수 kind 호출 == d와 같은 값
'canine'
>>> d.name            # 메소드 호출 == 인풋 데이터를 name에 넣음
'Fido'
>>> e.name            # 메소드 호출 => 인풋 데이터를 name에 넣음
'Buddy'
>>> d.add_trick('roll over')
>>> e.add_trick('play dead')
>>> d.tricks
['roll over', 'play dead'] 
```
1) 클래스를 선언  
2) Attribute 작성  
3) instance 생성(d, e)  
4) instance.Attribute로 호출 

>> 주의사항: 위 add_trick과 같이 list를 사용하게 되면 리스트의 값은 공유되기 때문에  초기화자(__init__)에 빈 list를 추가해두어야 한다. 

```
class Dog:
    ~ 중략~
    def __init__(self, name):
        self.name  = name
        self.tricks = []
    ~ 중략 ~
```

### 주의사항

1) 데이터 Attribute(이하 '속성')은 같은 이름의 메서드를 덮어쓰기 떄문에 의도하지 않게 충돌이 될 수 있다. 메서드에는 동사를, 속성에는 명사를 쓰는 것이 좋다.  
2) 파이썬코드는 숨기는 기능이 없기 때문에 클라이언트가 참조될 수도 있다.  
3) 메서드는 일반 함수들과 마찬가지로 전역 이름을 참조할 수 있기 때문에 헷갈리지 않게 조심하자.

```
class Flight:

    def __init__(self, number):
        if not number[:2].isalpha():
            raise ValueError("첫 두글자가 알파벳이 아닙니다.")
        if not number[:2].isupper():
            raise ValueError("첫 두글자가 대문자가 아닙니다.")
        if not number[2:].isdigit():
            raise ValueError("세번째 글자 이상이 양의 숫자가 아닙니다.")
        self.__number = number

    def number(self):
        return self.__number
```
`_name`의 경우 외부에서 호출이 가능하지만, 이 함수는 내부에서만 사용하기 위해 정의해 둔 것이라고 생각하면 쉬울 듯 하다. 파이썬을 공부한 사람들이라면, `_`가 붙은 메서드를 호출한다면 권유의 문법이라고 불린다고 한다. 위 예제에서는 외부에서 접근을 못하게 하기 위해 `__`을 사용했다.  
만약 `__name`을 호출하게 될 경우 에러메시지가 나온다.


참고 사이트:  
[파이썬 공식문서](https://docs.python.org/ko/3/tutorial/classes.html)  
[위키독스-파이썬기초](https://wikidocs.net/16071)
[곰가드의 라이브러리](https://gomguard.tistory.com/125)  

