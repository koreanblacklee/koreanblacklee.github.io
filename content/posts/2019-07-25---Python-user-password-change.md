---
title: Django를 활용한 유저 패스워드 변경(백엔드)
date: "2019-07-25T12:46:37.121Z"
template: "post"
draft: false
slug: "/posts/python/Django/userpwchange"
category: "python"
tags:
  - "Python3"
  - "Wecode"
  - "Django"
description: "Djangp / ChangePassword"

------

오늘은 라이브러리를 json과 bcrypt를 제외한 다른 라이브러리를 사용하지 않고 유저의 패스워드 변경관련해서 포스팅 해보려 한다.

*****

일단 유저의 회원가입과 로그인에 대해 자세히 알아보자.

기본적으로 유저가 회원가입을 시도하면, 기존에 가입된 아이디나 휴대폰번호, 이메일이 있는지 확인하고, 동일한 정보가 없다면 DB에 회원의 정보를 저장(패스워드는 Bcrypt나 단방향 암호화를 한 후에 저장한다.)하고 회원가입이 되었다는 것을 프론트엔드에게 알려준다. 그 후, 유저가 로그인을 시도하면 DB에 저장된 아이디와 패스워드가 일치하는지 확인한 후 일치한다면 `access_token`과 로그인성공 메세지를 프론트엔드에게 전달해준다.  
(해당 과정은 아래 블로그에 자세히 설명되어 있으니 한번 읽고 와주길 바란다.)
[pyJWT를 이용한 Python Django Login Decorator](https://koreanblacklee.github.io/posts/decorator/)

유저의 패스워드 변경은 위 `access_token`과 파이썬 데코레이터를 이용해서 해당 유저가 가입된 유저인지 확인 한 후 기존 패스워드와 변경될 패스워드를 받아서 진행한다.
유저가 패스워드 변경을 시도하면
1. jwt토큰과 유저의 원래 패스워드, 변경하고 싶은 패스워드를 받고
2. 유저의 이메일이 기존에 가입되어 있는 유저인지(데코레이터가 확인),
3. 유저의 기존 패스워드가 동일한지 체크해서
4. 맞다면 유저의 패스워드를 변경
5. 기존 패스워드가 아닐 경우 > `INVALID_PASSWORD`

회원가입과 반대로 진행하면 된다고 생각하면 쉬울 듯 하다.

그럼 이제 파이썬 코드를 보자.  
(패스워드를 암호화하기 위해 `bcrypt`를 이용했고, `access_token`은 pyJWT를 이용했다.)  

```
@login_decorator # 유저가 로그인이 되었는지 확인하기 위한 데코레이터
def post(self, request):
        user_info = json.loads(request.body) # 프론트엔드에서 바디로 보내준 정보를 변수에 저장
        current_pw = user_info["current_password"] # 기존의 패스워드를 변수에 저장
        new_pw = user_info["new_password"] #변경될 패스워드를 변수에 저장

        if bcrypt.checkpw(current_pw.encode("UTF-8"), request.user["password"].encode("UTF-8")): # 유저의 패스워드가 맞는지 확인
            bytes_pw = bytes(new_pw, 'utf-8') 
            new_hashed_pw = bcrypt.hashpw(bytes_pw, bcrypt.gensalt()) #패스워드를 `bcrypt`를 이용해 단방향 암호화 진행

            request.user.password = new_hashed_pw
            request.user.save() # DB에 유저의 변경된 패스워드를 저장

            return JsonResponse(
                {
                    "message" : "SUCCESS"
                }, status = 200) # 프론트엔드에게 잘 되었다고 얘기해줌

        else:
            return JsonResponse(
                {
                    "message" : "INVALID_PASSWORD"
                }, status = 400) # 만약 패스워드가 기존에 저장된 값과 다르다면 메세지와 함께 status 400을 프론트엔드에게 전달해주자.

```
각각의 주석을 달아두었으니 코드가 이해가 안된다면 한번씩 읽어보길 바란다.

여기서 중요하게 봐야 할 점은,   
1) 해당 함수가 실행되기 전 데코레이터가 실행되며, 데코레이터에서 회원의 정보와 `access_token`등을 받아서 `post`함수가 실행된다.
2) 로그인 확인하기 위한 데코레이터가 있기 때문에 `access_token`이 decode가 되는지, 이미 로그인한 회원인지 등은 여기서 별도로 확인안해도 된다.(`request.user.password`가 그 예이다.)    

현재 개발을 배우고 있는 나는 데코레이터를 오랫만에 사용하다 보니 위 과장을 정확히 인지를 못하고 있었다. 이번 기회로 머리에 정확히 박혀 있으면 좋으련만,, 내 기억력은 나조차 못믿어서,, 잘 기억하기 위해 매일 블로그를 한번 씩 보면서 되새겨야 겠다.

로그인, 회원가입을 계속 하다보니 로직을 짜는건 쉬웠는데 데코레이터를 생각못해서 시간을 많이 허비한 점이 아쉽긴 하지만 다시 한번 배웠다고 생각하고 다음으로 넘어가야 겠다.   

구글링을 하면 프레임워크밖에 안나오는데, 프레임워크말고 기본적으로 어떻게 돌아가는지 알고 넘어가자.