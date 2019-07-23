---
title: 파이썬 기본 문법 정리
date: "2019-07-23T08:46:37.121Z"
template: "post"
draft: false
slug: "/posts/python/foundation2"
category: "python"
tags:
  - "Python3"
  - "Wecode"
  - "Test"
description: "foundation"

------

#### 1) 변수란?  
보통 프로그래밍 분야에서 변수는 어떤 값을 저장하는 공간(기억해야 하는 값을 대신 기억하기 위한 이름표)을 의미하며, 파이썬에서 사용하는 변수는 객체를 가리키는 것이라고도 말할 수 있다.(객체란 자료형(int, string, list 등)과 같은 것을 의미한다.)  
어떠한 데이터를 메모리상에 위치하는 주소를 이용해서 변수라는 곳에 저장해둔 후 변수에 저장된 메모리상의 주소에 가서 실제 값을 읽어서 사용한다.`(저장 및 호출)` 또한, 변수에 저장된 값을 원하는 만큼 곱하거나 반복, 혹은 값을 수정 할 수도 있다.  
사용예시: 핸드폰 연락처 목록, 기억해두어야 할 특정 숫자(주식가격 등)

```
>>> a = [1, 2, 3]
>>> a
[1, 2, 3]

>>>  a * 3  # 변수의 반복
[1, 2, 3, 1, 2, 3, 1, 2, 3]

>>> b = 3 
>>> b * 2   # 변수의 곱셈
6
````
위 코드를 해석하자면 `a`라는 이름(변수)에 `[1,2,3]`이라는 값을 가리키게하라는 것이며, 프로그래밍 언어에서는 binding(가르키는것)을 의미한다.  
위와 같은 코드를 작성하면 `a`는 `[1,2,3]` 값을 가지는 리스트 자료형이 자동으로 메모리에 생성되고 변수 a는 `[1, 2, 3]`리스트가 저장된 메모리의 주소를 가리키게 된다.(데이터를 메모리 상에 위치하는 주소를 변수라는 곳에 저장)
* 메모리: 컴퓨터가 프로그램에서 사용하는 데이터를 기억하는 공간 => `id(a)`로 객체의 주소값을 확인할 수 있다. 

```
>>> id(a)
4516974408
```

`b = a`를 실행할 경우 같은 주소를 가지고 있다.  

#### 2) 변수를 만드는 방법  
먼저 선언된 변수명에 다른 값을 바인딩하게 되면 해당 변수의 값은 변경된다.  

```
>>> a = 1
>>> a
1
>>> a = 2
>>> a
2
```  
```
>>> a,b = ('python', 'life') 
(a,b) = 'python', 'life' #a,b변수에 각각의 값을 대입
>>> a = b = 'python'  #a와 b에 같은 값을 대입
```  

두 변수의 값을 서로의 값으로 변경하는 방법
```
>>> a = 3
>>> b = 5
>>> a, b = b, a
>>> a
5
>>> b
3
```

#### 3) 변수명 사용시 고려사항    
변수를 선언할 때 변수명은 잘 정해야 한다.   
어떠한 값을 사용하기 위해서 변수에 저장했는데, 이름과 값이 매칭이 안될 경우 사용하는데 어려움이 있고, 다른 개발자가 봤을때도 해당 변수가 어떤 값을 저장해두었는지 예상할 수 있도록 `가시성`에도 신경써야 한다. 영어 또는 언더스코어로 시작을 해야 하며, 숫자로 시작할 경우 아래와 같이 오류메세지가 발생한다.   
```
>>> 1naver = 60
SyntaxError: Invalid syntax
```
한글도 변수명으로 사용할 수 있지만 일반적으로 한글보단 영어와 언더스코어를 조합해서 변수명을 만드는 것이 좋다.