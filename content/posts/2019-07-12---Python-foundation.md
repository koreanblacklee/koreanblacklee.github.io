---
title: Python Foundation1
date: "2019-07-12T08:46:37.121Z"
template: "post"
draft: false
slug: "/posts/python/foundation1"
category: "Python Foundation1"
tags:
  - "Python3"
  - "Wecode"
  - "Foundation"
description: "Python 기초 문법 정리"
---

파이썬 홈페이지에선 파이썬 코딩 표준에 관한 문서를 정의해두었고, 기초를 익힌 후 참조하면 코딩스타일을 익히는데 많은 도움이 될 듯 하다   
[파이썬 공식홈페이지 코딩스타일 정의](https://www.python.org/dev/peps/pep-0008)

#### 들여쓰기(Indentation)
파이썬은 코딩블럭을 표시하기 위해 들여쓰기(Indentation)을 사용하며, 문장을 예로 들어보면 if, for, def 등 끝에 콜론(:)을 사용하면 내부 코딩블럭들은 동일한 들여쓰기를 사용해야 한다.  
일반적으로 공백(스페이스) 4개를 사용하며, 모두 같은 들여쓰기를 쓰지 않고 하나만 5개의 공백을 사용하면 IndentationError가 발생한다.

#### 표준 라이브러리
파이썬은 표준라이브러리들을 제공하고 있는데, 불러다 사용하기 위해선 import문을 사용한다.  


#### Comment
코딩 외의 설명이나 기타 내용을 작성한 곳에서 사용하는데, 사용위치는 상관없지만, 보통 #사인 뒤에 하나의 공백을 두는 것을 권장한다.

#### 기본 데이터 타입
`int`           = 정수형    
`float`         = 소수점을 포함한 실수   
`bool`          = 참, 거짓을 표현    
`None`          = Null과 같은 표현     
`ComplexNumber` = a+bj(파이썬에선 j를 사용함)
                a의 값을 얻기위해선 .real, b의값을 얻으려면 .imag  

#### 포맷팅 연산자(Formatting Operator) 
`%s`	       문자열 (파이썬 객체를 str()을 사용하여 변환)    
`%r`	       문자열 (파이썬 객체를 repr()을 사용하여 변환)    
`%c`	       문자(char)    
`%d` 또는 %i	정수 (int)    
`%f` 또는 %F	부동소수 (float) (%f 소문자 / %F 대문자)    
`%e` 또는 %E	지수형 부동소수 (소문자 / 대문자)    
`%g` 또는 %G	일반형: 값에 따라 %e 혹은 %f 사용 (소문자 / 대문자)    
`%o` 또는 %O	8진수 (소문자 / 대문자)    
`%x` 또는 %X	16진수 (소문자 / 대문자)    
`%%`	       % 퍼센트 리터럴    

#### Str Metod(문자열 메서드)
`str.join()` 여러개의 문자열을 하나로 합쳐 줌    
`str.split()` 특정 문자열을 괄호안의 기준으로 분리하여 리스트로 리턴    
`str.format()` 문자열에 포함된 '{}'들의 값을 format()의 값으로 대입    

#### if, while, for
`if` 주로 참과 거짓, 혹은 해당 값인지 확인하기 위해 사용됨    
`while` 조건문이 거짓이 될때까지 계속 실행됨
`for` 리스트, Tuple, 문자열 등의 인덱스를 하나하나 확인할 때 주로 사용됨( ex) for i in range(a):)   
`range` 반복문과 연동되어 많이 사용됨

#### List
Mutable 데이타 타입으로 여러 요소들을 갖는 집합체며, 추가, 수정, 삭제가 가능함   
Indexing이란 a.[n]과 같이 리스트안의 특정 요소만을 선택하기 위해 사용됨 

#### List Method
`slice` x = [처음 인덱스:마지막인덱스]와 같이 부분집합의 범위를 지정하여 필요한 데이터만 추출할 수 있음, 만약 처음 인덱스나 마지막 인덱스 중 한가지의 값이 공란으로도 사용가능함    
`append` a.append()로 표현되며, 괄호안의 데이터를 a의 마지막 값을 추가 할 수 있음
`del` del a[0]과 같이 사용되며, 리스트 중 0번째 데이터를 삭제할 수 있음   
a[1] = 11과 같이 값을 수정할 수도 있음   

#### List 기타
`c = a + b` a와 b가 리스트라는 가정 하에 2개의 리스트를 하나의 리스트로 병합할 수 있음
`x = a * 3` x에 a리스트를 3번 반복한 값을 저장함    
`a = list.index('')` a변수에 list의 값 중 index괄호 안에 있는 값을 검색해서 저장함 
`a = list.count('')` a변수에 list의 값 중 count괄호 안에 있는 값이 몇번 나오는지 저장함
`list = [for n in range(5)]` 리스트에 for문을 넣어 실행시킬수도 있음

#### Tuple
리스트와 유사하나 새로운값을 추가, 갱신, 삭제할 수 없다 => immutable 데이터 타입    
`tu = (345,)` 만약 튜플의 요소가 하나일 경우 콤마를 사용하면 tuple값으로 저장됨

#### Tuple Method
튜플의 값을 변경할 순 없지만, 튜플의 값을 다른 변수에 저장하여 사용할 수 있다.
`a = tu[1]` 튜플값 중 인덱스가 1인 값을 a변수에 저장함
`s = tu[1:]`  튜플 값 중 인덱스가 1인 값부터 마지막 값까지 s변수에 저장
`c = a + b` a와 b가 모두 튜플일 때, c변수에 두개의 데이터를 저장
`c = a * 3` 리스트와 마찬가지로 a튜플의 값을 3번 반복하여 c에 저장
`a, b = ('lee', 'sj')` a튜플에 'lee', b튜플에 'sj'를 각각 저장 (lee, sj는 제 이름을 예시로 한것임)

#### Dictionary(dict)
{키(Key):값(Value)} 쌍을 요소로 갖는 데이터 타입이며, 키를 이용해서 값을 빠르게 찾을 수 있는 hashtable 구조의 데이터이다. 또한, "dict"클래스로 구현되어 있으며, 키는 수정할 수 없는 immutable타입이어야 함 => 튜플은 키로 쓸 수 있는 반면 리스트는 사용할 수 없음

####입력, 추가, 수정, 삭제, 읽기 등
`c = a['b']` a의 딕셔너리 중 b의 키를 가진 값을 c변수에 저장함
`a['b'] = c` c를 a딕셔너리 중 b의 키를 가진 값으로 수정함
```
a = [("A",1),(B,2)]
c = dict(a)
```
or
```
a = (b=1,c=2)
c = dict(a)
```
a의 값을 딕셔너리화하여 c에 저장함(만약 기존 데이터가 있다면, 각 키의 값들은 수정됨)

`a["b"] = "c"` a딕셔너리 중 b의 키가 없다면 c를 값으로 추가함
`del a["b"]` a딕셔너리 중 b의 키를 삭제함(값도 같이 삭제됨)

####dict Method
`dict.keys()` => "dict"에 저장된 키값들을 리턴  
`dict.values()` => "dict"에 저장된 값들을 리턴  
`dict.items()` => "dict"의 키와 값을 dict_items 객체로 리턴함(dict_items를 리스트로 변환하기 위해서는 list()를 사용하면 됨 ex) itemlist = list(items)   
`dict.get("key")` => 키의 값을 리턴하는건 동일하지만, 키가 없을 경우 None을 리턴하므로 더 유용하게 사용할 수 있다.(`dict["key"]`를 하게 될 경우 에러메세지가 출력됨)  
`if "key_name" in dict:` => dict에 key_name이 있는지 확인  
`dict.clear()` => dict의 키와 값을 모두 삭제

`dict.update({key_name1: value1, key_name2:value2})` => dict에서 key_name1,key_name2의 값을 각각 수정할 때 사용됨

