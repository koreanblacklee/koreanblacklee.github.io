---
title: Database란?-3(관계형 데이터베이스의 제약조건 및 Key)
date: "2019-08-08T12:46:37.121Z"
template: "post"
draft: false
slug: "/posts/database/intergrity"
category: "Database"
tags:
  - "Database"
  - "Intergrity"
  - "Wecode"
description: "Database"

------

이번 시간에는 관계형 데이터베이스에서의 제약조건과 해당 Key의 종류를 알아보고자 한다.
데이터베이스의 기본적인 개념이 정리가 안되어 있다면, 아래 2개의 블로그를 읽고 온다면 좀 더 쉽게 이해할 수 있을 것이다.

[Database란?](https://koreanblacklee.github.io/posts/database)   
[Database란?-2(schema)](https://koreanblacklee.github.io/posts/database/schema)
------

## 관계형 데이터베이스의 제약조건

제약조건으로는 아래와 같은 두가지의 **무결성**(개체 무결성, 참조 무결성) 이 있고, 무결성을 보장하기 위해 **Key** 를 이용하고 있다.(이는 데이터베이스의 AICD중 일관성에 해당하는 부분이다.)

### Key란?
* 데이터베이스에서 조건에 만족하는 튜플(행)을 찾거나 순서대로 정렬할 때 다른 튜플들과 구별할 수 있는 유일한 기준이 되는 속성을 의미 함(데이터베이스에서 데이터들을 구별할 수 있는 기준)

### Key의 종류
1. Candidate Key(후보키)    

    * 튜플의 유일성을 식혈할 수 있는 모든 Key다. 즉, Primary Key말고 해당 행에서 고유한 값을 가질 수 있는 키들을 의미한다.(하나 혹은 그 이상의 컬럼이 될 수 있으며 고유값을 가지고 있기 때문에 Primary Key로 사용가능하여 후보키 라고 함)
2. Primary Key(기본키)  
    * 튜플의 유일성 확보를 위해 후보키 중에 선택된 Key이다.
        * 유일성: 하나의 키 값으로 하나의 튜플을 유일하게 식별할 수 있어야 하는 것
    * 테이블의 각 행들을 고유하게 식별할 수 있는 Key
    * Django에서는 테이블 별로 기본적으로 `id`라는 Primary Key를 자동으로 만들어 주며, 만약 자동으로 만들지 않고 아래와 같이 입력하면 다른 속성(Attribute) 중에 Primary Key를 지정할 수도 있다.
    * 고유한 값을 가지고 있고, null값을 가지고 있으면 안됨

```models.py
from django.db import models


class User(models.Model):
    user_email = models.CharField(max_length=100, primary_key=True)
    ...

```

3. Alternate Key(대체키)
    * 두개 이상의 후보키가 있는 경우 기본키를 제외한 나머지 후보키로 이른 바 보조키를 의미한다.
    * 후보키에서 기본키로 사용될 잠재적 역량을 가졌지만(고유값을 가진) 기본키로 선택되지 못한 키를 얘기한다.(고유값을 가지고 있기 때문에 언제든지 대체할 수 있기 때문에 대체키라고 한다고 한다.)

4. Foreign Key(외래키)
    * 관계형 데이터베이스에서 다른 테이블끼리의 연결을 위해 중요한 역할을 하는 key다.
    * 관계된 다른 테이블(=Entity, Table)간의 참조관계를 나타내기 위한 Key이며, 하나 혹은 하나 이상의 속성들의 집합이다.  

홈페이지에서 사용자가 있고, 댓글을 작성하는 기능이 있다면 댓글이 저장되는 테이블에서 사용자가 외래키로 등록이 되어 있어야 어떤 사용자가 댓글을 달았는지 참조 할 수 있다.

```
# users/models.py
from django.db import models


class User(models.Model):
    name = models.CharField(max_length=100)
    ...
```

```
# comment/models.py
from django.db import models
from users.models import User   #User디렉토리의 models.py파일에 있는 User를 사용하기 위해 import함


class Comment(models.Model):
    user = models.ForeignKey(User, on_delete=models.CASCADE)
    post = models.CharField(max_length=255)
    ...
```    

5. Super Key(슈퍼키)
    * 릴레이션 내에 있는 **속성들(Attribute)** 의 집합으로 구성된 최소성이 없는 Key를 의미한다.
    * 특정 튜플(행)을 식별할 수만 있다면 Super Key가 될 수 있다.
    * 최소성: 키를 구성하는 속성 하나를 제거하면 유일하게 식별할 수 없도록 꼭 필요한 최소의 속성을 구성되어야 한다는 것


## 데이터 무결성  
* 데이터의 정확성과 일관성을 유지하고 보증하는 것을 의미한다.

### 개체 무결성
* 기본키(primary_key)와 관련된 것으로 모든 테이블은 Primary_key를 가져야 하며, Null(빈 값)이 들어갈 수 없음


### 참조 무결성
* 외래키(ForeignKey)와 관련된 것으로 외래키는 참조 테이블의 키본키 값 또는 Null값을 가지며, 참조 테이블의 기본키 속성 개수와 도메인이 일치해야 함
    * 도메인: model을 설정할 때 각 속성이 어떤 타입인지, 최대 몇글자인지 등 데이터의 형식을 정의하는데, 도메인은 이러한 데이터 형식에 맞는 값들의 집합이라고 하면 될듯 하다.
* A라는 테이블이 B라는 테이블에 외래키로 참조되고 있을 때, B테이블에서의 데이터를 사용자의 실수로 삭제되거나 수정되는 것을 막아준다.




참고 사이트:

[관계형 데이터베이스의 제약조건(Key와 무결성)](https://gomcine.tistory.com/entry/Database-8-RDB%EC%9D%98-%EC%A0%9C%EC%95%BD%EC%A1%B0%EA%B1%B4-Key%EC%99%80-%EB%AC%B4%EA%B2%B0%EC%84%B1?category=733455)  
[릴레이션 키 개념& 종류](https://jhnyang.tistory.com/71?category=817647)  
