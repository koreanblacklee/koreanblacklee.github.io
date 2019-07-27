---
title: Python Mocking Test(mock, patch)
date: "2019-07-26T012:46:37.121Z"
template: "post"
draft: false
slug: "/posts/python/Django/test2"
category: "python"
tags:
  - "Python3"
  - "Wecode"
  - "mocking test"
description: "Python Mocking Test"

------

일전에 블로깅 했던 Python Unittest에서 얘기한 Mock 테스트에 대해 더 알아보고자 한다.

### Why Unit Testing is Important?
모듈이나 애플리케이션 안에 있는 개별적인 코드들의 가장 작은 단위에서 코드들이 생각한대로 기능하는지 테스트하는 것을 Unittest라고 하며, 유닛테스트는,  
* 각각의 코드들이 어떤 동작을 하는지 생각하는데 도움을 준다.
* 버그를 찾고, 버그를 고쳤을 때 문제가 없는지를 확인한다.

>> 코드가 효율적인지, 각각의 코드들이 문제가 없는지, 버그를 수정한 후 문제강 없는지 등을 확인하기 위한 테스트

### What is Mock?

실제 객체를 만들기엔 비용과 시간이 많이 들거나 의존성이 걸처져 있어 제대로 구현하기 어려울 경우, 가짜 객체를 만들어 사용한다고 보면 된다.(Mock외에도 )

### When should use Mock?  
유닛테스트를 하다 보면 테스트를 하기 위해 실제로 실행할 수 없는 코드들이 있다.   
예를 들어, SMS 문자, 결제 API, 플랫폼 소셜 로그인 등이 있을 수 있다.  
즉, 외부 API를 실제로 호출하게 되면 결제가 되고, 문자가 보내지는 등 테스트를 위한 추가 시간과 비용이 발생할 수 있기 때문에 위와 같은 코드를 테스트 하기 위해서 `Mock`을 이용한 테스트를 시행한다.   
Stackoverflow나 구글링을 해보면 많은 설명과 사례들이 있으니 시간내서 보는 것도 좋을 듯 하다.

[stackoverflow: What is the purpose of mock objects?](https://stackoverflow.com/questions/3622455/what-is-the-purpose-of-mock-objects)  
(좀 오래되었지만 답변에 mock테스트 관련해서 잘 나온 듯 해서 퍼왔다(물론 크롬번역을 통해서 봤지만,,,)

>> **테스트 환경을 구축하기 어려운 경우, 특정경우나 의존적인 경우, 테스트 시간이 오래 걸리는 경우 등의 이유로 사용된다.**

### What is Patch?

런타임 시 속성을 동적으로 대체하는 것을 말한다.  
예를 들어, 카카오톡에 소셜로그인을 하기 위해 카카오톡 API로 access_token을 보내서 사용자 정보를 받기 위한 requests에 대한 데이터를 테스트를 위해 고정으로 반환할 값을 정해주어야 할 경우 아래와 같이 작성하면 된다.

`@patch("account.views.requests")`  

### How to Mocking Test?

그럼 어떻게 테스트를 해야 하는지 아래 코드를 통해 확인해보자
1. 장고환경  
2. 홈페이지 관리자의 카카오톡 소셜로그인을 하는 코드를 테스트하기 위해 작성되었습니다.

#### 로직 순서
1. views.py
* 소셜로그인 코드 작성(최초 request는 프론트엔드에서 카카오톡 `access token`을 전달 받음)
* 카카오톡 토큰을 카카오톡 API로 보내 사용자의정보를 `requests` 요청 (추후 이 부분을 mocking할 것임)
* 카카오톡에서 받은 정보 중 `nickname`이 DB에 동일한 관리자가 있는지 확인(특수한 경우로 보통 카카오 ID을 기준으로 함)[구글,카카오 소셜로그인](https://koreanblacklee.github.io/posts/djangsociallogin/) 페이지 확인
* `nickname`이 같은 관리자가 존재하고, 해당 관리자가 카카오톡으로 회원가입 했는지 확인(카카오톡 에서 받은 정보 중 `id`가 고유값이기 떄문에 해당 값을 이용해서 파악)
* 기존에 가입했다면 바로 로그인, 가입하지 않았다면 Employee Table에 저장(**본래 관리자 중에 동일한 이름이 없을 경우 예외처리를 해주어야 하지만, 해당 부분은 제외했습니다.**)

2. tests.py
* 테스트를 위해 `EmployeeTest`클래스를 만들고 `setUp`(EB에 가짜 사용자를 create함), `tearDown`(하나의 테스트함수가 실행된 후 해당 테스트의 데이터 삭제하는 함수로 setUp함수와 함께 사용된다고 알면 된다.)를 만든다.
* 테스트 class안에 `test_employee_kakao_account`작성 및 `@patch("account.views.requests")` 데코레이터 작성
* `MockedResponse`mock test에 사용할 class 작성(카카오톡에서 얻을 정보 중 활용할 정보들을 작성)
* `MagicMock(return_value = MockedResponse)`을 requests.post 함수에 저장
* 나머지는 기존 테스트 코드와 동일한 코드이기 때문에별도의 설명 생략함.

**각 클래스와 함수별로 기능을 작성해두었고, 나머지는 코드를 보고 이해하면서 공부를 해보길 바란다.**
```
#views.py
import json
import bcrypt
import jwt
import requests

from settings       import SECRET_KEY, EXP_TIME
from account.models import Employee
feom django.views   import View
from django.http    import JsonResponse


class EmployeeSocialLoginView(View): #카카오톡 소셜로그인을 위한 클래스

    @employee_login_required # employee가 로그인 상태인지 확인하기 위한 데코레이터
    def post(self, request): # frontend에 post로 카카오톡 소셜로그인 토큰을 request 받음

        #토큰을 이용해서 카카오톡에 사용자 정보 확인 요청(requests가 핵심)
        access_token = request.headers["Authorization"]
        headers      = ({'Authorization' : f"Bearer {access_token}"})
        url          = "https://kapi.kakao.com/v1/user/me"
        response     = requests.post(url, headers=headers, timeout=3)
        employee     = response.json()
        exp_time = EXP_TIME

        # 관리자가(employee) 기존에 카카오톡 계정이 DB에 저장되어 있는지 확인
        if Employee.objects.filter(kakao_id = employee["id"]).exists():
            exp_time = EXP_TIME
            siren_secret = SECRET_KEY

            employee_data = Employee.objects.get(kakao_id = employee["id"])
            encoded_jwt = jwt.encode({"employee_id":employee_data.id, 'exp':exp_time}, siren_secret, algorithm="HS256")

            login_check = Login(
                employee = Employee.objects.get(employee_code=employee_data.employee_code)
            )
            login_check.save()

            return JsonResponse(
                {
                    'access_token' : encoded_jwt.decode('UTF-8'),
                    'name'         : employee_data.name,
                    'message'      : 'SUCCESS'
                }, status = 200
            )

        #저장되어 있지 않다면 DB에 저장
        else:
            employee_info = Employee.objects.get(name=employee['properties']['nickname'])
            employee_info.kakao_id = employee["id"]
            employee_info.save()

            return JsonResponse(
                {
                    "message": "SUCCESS"
                }, status = 200
            )
```
```
#tests.py
import json
from account.models import Employee
from django.test    import TestCase, Client
from unittest.mock  import patch, MagicMock

#setUp, tearDown을 통한 사전설정 
class EmployeeTest(TestCase):
    def setUp(self):
        bytes_pw = bytes('1234', 'utf-8')
        hashed_pw = bcrypt.hashpw(bytes_pw, bcrypt.gensalt())
        Employee.objects.create(
            name          = '아이유',
            user_id       = '100011',
            password      = hashed_pw.decode('UTF-8'),
            phone_number  = '010-1234-1111',
           )

    def tearDown(self):
        Employee.objects.filter(name='아이유').delete()


    #patch, MagicMock을 이용한 테스트
    @patch("account.views.requests")
    def test_employee_kakao_account(self, mocked_requests):
        c = Client()

        class MockedResponse:
            def json(self):
                return {
                    "id" : "12345",
                    "properties" : {
                        "nickname" : "아이유"
                    }
                }

        mocked_requests.post = MagicMock(return_value = MockedResponse())

        test = {
            'employee_code':'1000111',
            'password':'1234',
            'nickname' : '아이유'
        }

        response = c.post("/account/employee/kakao", json.dumps(test), **{"HTTP_AUTHORIZATION":"1234","content_type" : "application/json"})

        self.assertEqual(response.status_code, 200)
        self.assertEqual(
            response.json(),
                {
                    'message' : 'SUCCESS'
                }
        )

```
- 참고  
[Mock](https://blog.leop0ld.org/posts/about-mocking/) : 오브젝트들을 동적으로 대체하고 사용결과를 확인하기 위한 다양한 기능들을 제공, 의존성이 잇는 것들을 실제로 실행시키지 않고 호출여부, 인터페이스를 확인할 수 있다.   
[mock_requests](https://gist.github.com/evansde77/45467f5a7af84d2a2d34f3fcb357449c) 원본