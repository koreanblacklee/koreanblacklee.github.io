---
title: Django 소셜로그인(google, kakao) 구현
date: "2019-07-13T19:46:37.121Z"
template: "post"
draft: false
slug: "/posts/djangsociallogin/"
category: "Wecode 첫번째 프로젝트(2) Django Social Login"
tags:
  - "Python"
  - "Django"
  - "Wecode"

description: "Django + React로 웹사이트를 제작할 때 별도의 라이브러리를 사용하지 않고 구글 및 카카오 소셜로그인을 구현"
---

Wecode에서 진행하는 첫번재 프로젝트에서 소셜로그인과 회원가입을 구현해봤다.   
자체 회원가입 및 로그인을 해봤기 때문에 비슷하다고 생각해서 금방 끝날 줄 알았는데 오산이였다. 프론트엔드에서 google과 kakao토큰을 전달받은 후 사용자 정보를 받는 것을 해봤고, 그와 관련된 포스팅을 남긴다.
(별도의 라이브러리를 이용하지 않아서 코드가 길고 복잡할 수 있으며, 블로그를 보기 전에 충분히 혼자서 고민해보고 어떤 로직으로 돌아가는지 봐줬으면 한다.)
****
##목차
####1. 소셜로그인이란?
####2. 소셜로그인 공통
####3. 구글 소셜 회원가입 및 로그인 구현
####4. 카카오 소셜 회원가입 및 로그인 구현

## 1. 소셜로그인이란? 
유저가 별도의 회원가입 없이 유저가 이용하고 있는 소셜 웹사이트의 로그인정보를 사용하여 웹사이트에 로그인계정을 얻게되어 회원으로 접근할 수 있도록 하는 방법.  
즉, 특별한 회원가입의 절차를 거치지 않고 기존에 가입된 소셜 웹사이트의 아이디와 패스워드로 이용하고 싶은 다른 사이트에 로그인 할 수 있는 기능

####장점
1) 많은 계정과 비밀번호를 기억하지 않아도 되는 편리함을 제공함   
2) 해외 소셜을 이용하면 외국인을 대상으로 하는 서비스를 제공하기도 쉬워짐
3) 네이버는 네이버 계정으로 가입된 여러 서비스가 나타남
4) 서버에 회원의 개인정보를 최소한으로 저장할 수 있음

####단점

1) 소셜계정을 해킹당했거나 소셜계정을 탈퇴한 경우 문제가 생길 수 있음   
2) 하나의 소셜을 가입한 뒤 가입한 사실을 모르고 추가로 가입할 수도 있음

단점도 있지만 여러가지 장점이 있어서 요즘 대부분의 웹사이트나 앱에서 소셜로그인을 통한 로그인을 많이 사용하고 있다. 
![Donec eu libero sit amet quam egestas semper. Aenean ultricies mi vitae est. Mauris placerat eleifend leo. Quisque sit amet est et sapien ullamcorper pharetra. Vestibulum erat wisi, condimentum sed, commodo vitae, ornare sit amet, wisi.](/media/social_login.png)
####로직순서(백엔드 기준)
구글, 카카오 등 기본적인 로직순서는 동일하여 한번에 정리함. 

1)프론트엔드에서 유저의 소셜로그인에 필요한 토큰을 전달받음   
2)받은 토큰을 각각의 소셜 api에 보내서 회원의 정보를 요청    
3)소셜 플랫폼에서 받은 자료를 파이썬에서 활용하기 위해(?) JSON화하여 변수에 저장   
4)JSON화 한 데이터 중 회원의 고유값을 저장하고 있는 키의 값이 DB에 저장되어 있는지 확인

4-1)DB에 고유값이 저장되어 있다면, 이미 기존에 가입이 되어 있는 유저이므로, JsonResponse로 Access_token과 정상적으로 처리완료되었고, 프론트엔드에서 필요로 하는 회원의 정보를 프론트엔드에게 전달   
4-2)DB에 고유값이 저장되어 있지 않다면, JSON화한 데이터에서 필요한 자료를 데이터베이스에 저장함과 동시에 프론트엔드에게 JsonResponse로 정상적으로 처리완료되었고, Access Token과 회원의 정보를 전달

####2. 소셜로그인 공통내용
1)models.py
SocialPlatform이라는 새로운 class 생성 후 platform이라는 메소드를 생성(어떤 소셜플랫폼을 이용하여 가입했는지 확인하기 위해)   
기존유저 클래스에 social메소드를 ForeignKey로 메소드 추가
각 소셜플랫폼의 회원 고유의 정보를 저장항 social\_login_id 메소드 추가
```
class SocialPlatform(models.Model):
    platform = models.CharField(max_length=20, default=0)

    class Meta:
        db_table = "social_platform"

class User(models.Model):
    ~중략~
    social          = models.ForeignKey(SocialPlatform, on_delete=models.CASCADE, max_length=20, blank=True, default=1)
    social_login_id = models.CharField(max_length=50, blank=True)
    ~중략~
```
2)urls.py
구글과 카카오의 소셜로그인 엔드포인트 작성
```
from .views import UserView, LoginView, GoogleLoginView, KakaoLoginView
urlpatterns = [
    path('/login/google',GoogleLoginView.as_view()),
    path('/login/kakao',KakaoLoginView.as_view())
]
```
####3. 구글 회원가입 및 로그인 구현
[구글](https://developers.google.com/identity/sign-in/web/backend-auth) 페이지를 확인해보니 이메일이 없을수도 있고, 회원의 고유키값(sub)이 따로 있기 때문에 해당 값을 social_login_id에 저장함(각 줄의 코드 뒤에 주석을 달아두었으니 필요하다면 복사하여 메모장이나 에디터에서 확인하면 쉽게 볼 수 있음)

````
class GoogleLoginView(View):
    # 소셜로그인을 하면 User테이블에 아이디와 패스워드를 담아두고
    
    def get(self,request): # id_token만 해서 헤더로 받기
        token    = request.headers["Authorization"] # 프론트엔드에서 HTTP로 들어온 헤더에서 id_token(Authorization)을 변수에 저장
        url      = 'https://oauth2.googleapis.com/tokeninfo?id_token=' # 토큰을 이용해서 회원의 정보를 확인하기 위한 gogle api주소
        response = requests.get(url+token) #구글에 id_token을 보내 디코딩 요청
        user     = response.json() # 유저의 정보를 json화해서 변수에 저장

        if User.objects.filter(social_login_id = user['sub']).exists(): #기존에 가입했었는지 확인
            user_info           = User.objects.get(social_login_id=user['sub']) # 가입된 데이터를 변수에 저장
            encoded_jwt         = jwt.encode({'id': user["sub"]}, wef_key, algorithm='HS256') # jwt토큰 발행
            none_member_type    = 1

            return JsonResponse({ # 프론트엔드에게 access token과 필요한 데이터 전달
                'access_token'  : encoded_jwt.decode('UTF-8'),
                'user_name'     : user['name'],
                'user_type'     : none_member_type,
                'user_pk'       : user_info.id
            }, status = 200)            
        else:
            new_user_info = User( # 처음으로 소셜로그인을 했을 경우 회원으 정보를 저장(email이 없을 수도 있다 하여, 있으면 저장하고, 없으면 None으로 표기)
                social_login_id = user['sub'],
                name            = user['name'],
                social          = SocialPlatform.objects.get(platform ="google"),
                email           = user.get('email', None)
            )
            new_user_info.save() # DB에 저장
            encoded_jwt         = jwt.encode({'id': new_user_info.id}, wef_key, algorithm='HS256') # jwt토큰 발행
        
            return JsonResponse({ # DB에 저장된 회원의 정보를 access token과 같이 프론트엔드에게 전달
            'access_token'      : encoded_jwt.decode('UTF-8'),
            'user_name'         : new_user_info.name,
            'user_type'         : none_member_type,
            'user_pk'           : new_user_info.id,
            }, status = 200)
````

####4. 카카오 회원가입 및 로그인 구현
기본적으로 구글 소셜로그인과 비슷하기 때문에 추가 설명은 생략하겟음.
```
class KakaoLoginView(View): #카카오 로그인

    def get(self, request):
        access_token = request.headers["Authorization"]
        headers      =({'Authorization' : f"Bearer {access_token}"})
        url          = "https://kapi.kakao.com/v1/user/me" # Authorization(프론트에서 받은 토큰)을 이용해서 회원의 정보를 확인하기 위한 카카오 API 주소
        response     = requests.request("POST", url, headers=headers) # API를 요청하여 회원의 정보를 response에 저장
        user         = response.json()

        if User.objects.filter(social_login_id = user['id']).exists(): #기존에 소셜로그인을 했었는지 확인
            user_info          = User.objects.get(social_login_id=user['id'])
            encoded_jwt        = jwt.encode({'id': user_info.id}, wef_key, algorithm='HS256') # jwt토큰 발행

            return JsonResponse({ #jwt토큰, 이름, 타입 프론트엔드에 전달
                'access_token' : encoded_jwt.decode('UTF-8'),
                'user_name'    : user_info.name,
                'user_pk'      : user_info.id
            }, status = 200)            
        else:
            new_user_info = User(
                social_login_id = user['id'],
                name            = user['properties']['nickname'],
                social          = SocialPlatform.objects.get(platform ="kakao"),
                email           = user['properties'].get('email', None)
            )
            new_user_info.save()
            encoded_jwt         = jwt.encode({'id': new_user_info.id}, wef_key, algorithm='HS256') # jwt토큰 발행
            none_member_type    = 1
            return JsonResponse({
                'access_token' : encoded_jwt.decode('UTF-8'),
                'user_name'    : new_user_info.name,
                'user_pk'      : new_user_info.id,
                }, status = 200)
```
