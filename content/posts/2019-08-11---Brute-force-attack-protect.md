---
title: Brute Force Attack Countermeasures (무차별 공격 대응방안)
date: "2019-08-11T12:46:37.121Z"
template: "post"
draft: false
slug: "/posts/database/bruteforce"
category: "bruteforce"
tags:
  - "Wecode"
  - "록그인공격 방어"
  - "Brute Force"
description: "Brute Force Countermeasures"

------

사용자가 웹사이트에서 다양한 서비스를 이용하기 위해서는 웹사이트에 회원가입을 해야 하며, 회원가입할 때 입력된 사용자의 정보는 모두 데이터베이스에 저장된다. 보통 다른 사이트에서 사용하는 아이디와 패스워드를 거의 동일하게 사용하므로 보안이 취약한 사이트에서 해커에게 사용자의 정보를 탈취당한다면, 다른 사이트에서의 사용자의 개인정보도 모두 누출이 된다고 봐도 과언이 아니다. 그러므로 개발자도 보안적인 부분을 알아야 하며, 이번에는 해커가 사용자의 개인정보를 알기 위해 무차별 로그인공격을 시도할 때 어떻게 방어해야 하는지 알아보고자 한다.  

---


## Brute Force Attack이란
사용자의 개인정보(주민등록번호, 비밀번호 등)는 내부적 유출과 외부적 유출을 막기 위해 복호화 할 수 없는 단방향 암호화를 진행 후 데이터베이스에 저장된다.  
* 내부적 유출: 내부 직원이나 개발자도 사용자의 비밀번호나 개인정보를 볼 수 없어야 한다.  
* 외부적 유출: DB가 해킹을 당해도 비밀번호가 그대로 노출되는 것을 막는다.  
* 단방향 암호화: 한번 암호화된 데이터를 통해 원본 메시지를 구할 수 없다는 것.  

해커는 데이터베이스에 저장된 사용자의 아이디와 암호를 알기 위해 문자, 숫자 및 기호의 가능한 모든 조합을 체계적으로 시도하여 비밀번호를 찾으려고 시도하는 것을 무차별 대입공격(Brute Force Attack)이라고 한다.  

>> 개발자는 무차별 대입공격을 막고, 사용자의 정보를 보호할 수 있도록 해커에 대한 방어 방법에 대해 알고 있어야 한다.  

## 방어 방법  
![데이터베이스 테이블 구조](/media/bruteforce-attack.png)

(이미지 참고사이트: https://m.mkexdev.net/426)  

사용자도 자신의 개인정보를 보호하기 위해 어려운 단어를 사용하거나 사이트간의 서로 다른 패스워드를 사용해야 하며, 이번엔 개발자가 방어할 수 있는 방법에 대해서 알아보자.   

#### 사용자가 암호를 최소 10자리 이상을 사용할 수 있도록 설정  
* 암호가 길고, 특수문자가 들어가 있다면 해당 암호를 알아내기까지는 몇 십년이 걸리게 된다. 아래 사이트에 접속해서 비밀번호의 안정성을 테스트해 볼 수 있다.  
[howsecureismypassword](https://howsecureismypassword.net/)

#### 캡차 활성화  

* 캡차란?(Completely Automated Public Turing test to tall Computers and Humans Apart)  
    * 사람과 로봇을 구분짓기 위한 일종의 테스트 같은 것으로, 악의적으로 사용되는 프로그램 봇(bot)을 막기 위해(해킹을 막기위해) 2000년대 사람과 컴퓨터를 구별하기 위해 만든 기술  

* 봇(bot)이란?  
    * 보안이 취약한 컴퓨터를 스스로 찾아 침입해, 보이지 않는 곳에서 조용히 자동하면서 컴퓨터 사용자도 모르게 시스템에게 명령을 내릴 수 있는 원거리 해킹 툴  
    * 2~3분 내에 수천개의 e메일을 만들거나, 같은 댓글을 계속 반복하는 등 반복적인 일을 빠르게 수행할 수 있음  

* 캡챠가 생긴 유래(이유)  
과거 1999년 인터넷상으로 과학 프로그램을 가진 대학을 뽑는 투표가 진행되었는데, 이 때 몇몇 대학이 자동으로 투표하는 봇을 만들었고, 이를 방지하기 위해 구겨진 텍스트이미지를 보여주고 이미지에 보이는 숫자를 똑같이 입력함으로써 로봇과 사람을 구분하는 캡차가 생김  

* 사용 용도  
    * 주로 계정 생성이나 게시물 등록에 많이 이용되며, 예를 들면, 광고성 게시물 방지, 아이디 자동생성 방지, 이메일 주소 보호, 온라인 선거, 계정 해킹 방지, 인공지능 개발 등이 있다.  

* 캡차의 종류  
    * 기계와 사람을 구분 가능한 모든 방식의 테스트를 지칭하기 때문에 아래에 있는 것 외에도 종류가 매우 다양하다(wikipedia)(텍스트 CAPTCHA, 오디오 CAPTCHA, 이미지 CAPTCHA, 슬라이드 CAPTCHA 등)  

* 캡차의 한계  
    * 캡차로 "봇"은 막을 수 있지만 사람은 막을 수 없었다. 많은 사람을 대동하거나 우회 홈페이지를 이용해 사람이 직접 해동하게 하면 CAPTCHAS는 통과시킬 수 밖에 없다. 또한 OCR프로그램이 발달하면서 텍스트 CAPTCHA는 기계들도 충분히 판독이 가능해졌고, 심지어 수학으로 된 CAPTCHA까지 예측해서 답을 내는 프로그램도 있다.(오디오: 음성인식, 안면인식: 사진분석 등) 반면, 특정 장애가 있는 사람들이나 어린이, 노인 등의 접근을 자체적으로 막는 악효과까지 나게 된다.  

* RECAPTCHA  
    * 알아보기 힘든 고문서나 오래된 종이책들을 텍스트화하기 위해 OCR프로그램을 사용하는데, 낙서나 얼룩, 헤짐등이 있으면 이러한 것들을 프로그램이 해석하지 못한다. 이런 단어들은 사람이 직접 해독해야 하지만 수요가 없는 책들을 해독하려면 엄청난 노동력과 인건비가 필요하여 reCAPTCHA가 나왔다. 간단히 설명하자면, 10명의 사람에게 OCR 프로그램이 해석할 수 없는 이미지를 전달하고, 10명의 사람이 똑같은 답을 입력한다면, 해당 문자로 간주하여 저장하는 형식이라고 한다.  


이번에 사용할 것은 같이 구글에서 제작한 reCAPTCHA v3이며 현재 기준에서(2019년 8월)가장 최근에 출시된 캡차다.(같이 협업을 하고 있는 위코드 1기 형주님과 윤하님이 이미 구현해 둔것을 파이썬 데코레이터를 활용하여 붙이는 역할만 진행했다.)  

* reCAPTCHA v3
    * 사용자 상호작용 없이 웹사이트에서 악성 트래픽을 감지하는 데 도움이 되는 최신 API
    * v1은 텍스트 CAPTCHA, v2는 구글맵을 사용한 이미지 CAPTCHA였다(자세한 설명은 하단의 reCAPTCHA v3 사이트 참고)

    * v3는 백그라운드에서 적응형 위험분석을 실행하여 의심스러운 트래픽을 알려주도록 하는 동시에 인간 사용자는 사이트에서 아무런 중단 없이 원할하게 사용하도록 할 수 있다. '액션'이라는 새로운 개념을 도입하여 사용과정에서 만나는 주요 단계를 정의하고 상황에 맞게 위험 분석을 실행하도록 하기 위해 사용할 수 있는 "태그"이다. 적응형 분석 엔진은 사용자에게 간섭을 하지 않으면서 다양한 활동을 살펴보고, 다양한 항목에 점수를 매김으로써 공격자의 패턴을 더욱 정확히 식별할 수 있다. 이를 통해 어떤 페이지에서 얼마나 의심스러운 트래픽이 발생했는지 파악할 수 있다.

    * 점수를 매기는 방법
        * 첫째, 보안 절차에서 사용자를 통과시키는 시점이나 추가적인 검증을 수행할 필요가 있는 시점을 경정하는 임계값을 설정할 수 있다.
        * 둘째, 액세스할 수 없는 고유의 신호(예: 사용자 프로필 또는 트랜잭션 기록)와 점수를 결합할 수 있다.
        * 셋째, 머신러닝 모델이 악용 시도에 대응하도록 훈련하기 위한 신호 중 하나로써 reCAPTCHA 점수를 사용할 수 있다.

* 위에서도 언급했듯이, reCAPTCHA v3는 사용자에게 전혀 불편을 끼치지 않고 사이트를 보호하는데 도움이 되고, 위험한 상황에서 대처 방법을 결정하는 데 있어 더 많은 권한을 부여한다.
* **reCAPTCHA v3를 이용하기 위해서는 구글 API를 이용해야 한다. 파이썬에선 데코레이터를 이용해서 활용할 수 있다.**

### 계정 잠금
* 구글 reCAPTCHA v3를 이용해서 봇에 대한 공격을 막았다면, 사람이 직접 브루트포스를 공격하는 것을 막기 위해 계정잠금과 IP차단 방법을 사용할 수 있다.  
* 로그인 실패가 일정 횟수 이상이 될 경우 로그인 시도를 더이상 할 수 없도록 계정을 잠그는 것이다. 이렇게 한다면, 해커가 수많은 로그인 시도를 할 수 없거나 힘들게 되어 공격 성공율이 엄청 떨어지게 된다.  
* 일정 시간이 지나면 계정잠금을 해제하거나 사용자의 추가인증(모바일, 이메일 등)을 통해 패스워드를 초기화 하면서 계정잠금을 해제하는 등의 방법을 사용할 수 있다.  

```logs/models.py  

from django.db import models
from django.utils import timezone

from user.models import User

class LoginLog(models.Model):
    user_email = models.ForeignKey(User, on_delete=models.SET_NULL)
    login_result = models.BooleanField()
    created_at = models.DateTimeField(auto_now_add=True)
    ...


    @classmethod
    def email_block_check(cls, user):

        logined_false_count = list(
            LoginLog.objects.filter(user_email=user)
            .order_by("-created_time")
            .values_list("login_result", flat=True)[:5]
        )

        if len(logined_false_count) >= 5:

            User.objects.filter(user_email=user.user_email).update(
                blocked_email=True,
                blocked_time=timezone.now()
            )
```  

### IP차단
* 하나의 아이디로 일정횟수 이상의 로그인을 시도했을 때 해당 계정이 잠기는 것에 반해, 동일한 IP로 다른 계정에 로그인을 계속 실패한 경우 해당 IP를 차단하여 더 이상 로그인을 시도할 수 없도록 할 수 있다. 예를 들어, 로그인 실패횟수가 5회 이상일 경우 해당 계정을 잠그고, 로그인실패 10회 이상일 경우 IP를 잠그는 방법을 사용할 수 있다.

```logs/models.py

    @classmethod
    def block_ip(cls, user_ip):
        # 같은 ip로 연속 10회 실패했을 경우 해당 ip 잠김
        if user_ip:

            block_ip = BlackListIP.objects.filter(ip_address=user_ip)

            if len(block_ip) >= LOGIN_FAILED_IP_COUNT:
                block_id = block_ip.order_by("-id")[0].id
                BlackListIP.objects.filter(id=block_id).update(
                    blocked_ip=True,
                    blocked_time=timezone.now()
                )

```

### 그 외
* 기존에 사용했던 패스워드를 다시 사용할 수 없도록 한다.
* 로그인 실패에 대한 에러 메세지나 status_code도 해커에겐 정보가 될 수 있다.(장고를 활용해서 이 부분을 방어하는 방법을 봤었는데,, 어디서 봤는지 까먹었,,)  
* 위 방어 방법들이 잘 갖춰져 있다 하더라도 운영 중 모니터링은 필수요소이며, 개발자나 관리자는 로그인 시도의 실패에 대한 사항을 로그(기록)을 남기고 감시해야 한다. 이를 위해 이상 현상이 발생했을 경우 관리자에게 경고창을 띄우는 등의 방법을 사용할 수 있다.

* Django의 여러가지 라이브러리를 사용하면 보다 쉽게 설정할 수 있을 듯 하다.  
[Stack overflow(Throttling brute force login attacks in Django)](https://stackoverflow.com/questions/11477067/throttling-brute-force-login-attacks-in-django)


참고사이트:  
[OWASP.org(open web application security project)](https://www.owasp.org/index.php/Blocking_Brute_Force_Attacks)  
[howsecureismypassword](https://howsecureismypassword.net/)  
[(브루트 포스) 공격](https://m.mkexdev.net/426)  
