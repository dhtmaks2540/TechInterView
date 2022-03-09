# OAuth

아래와 같은 창을 본적이 있을 것이다. 별도의 회원가입 없이 로그인을 제공하는 플랫폼의 아이디만 있으면 서비스를 이용할 수 있다. 외부 서비스에서도 인증을 가능하게 하고 그 서비스의 API를 이용하게 해주는 것, 이것을 바로 OAuth라고 한다.

![](https://developers.naver.com/inc/devcenter/images/cont/img_naverid01.png)

## OAuth 란?

> OAuth 2.0 은 다양한 플랫폼 환경에서 권한 부여를 위한 산업 표준 프로토콜이다.

간단하게 인증(Authentication)과 권한(Authorization)을 획득하는 것으로 볼 수 있다.

* 인증은 시스템 접근을 확인하는 것 (로그인) -> 인증만 하는 것은 openID
* 권한은 행위의 권리를 검증하는 것

<br/>

## OAuth의 배경

third party Application 에 아이디와 비밀번호를 제공하고 싶지 않은 요구가 첫번째입니다. 개인정보를 여러 곳에 입력하면서 피싱에 둔감해지고 무엇보다 Application이 안전하다는 보장이 없기 때문에 보안에 취약했습니다.

당시에는 인증과 권한을 부여하는 요구를 만족 시킬 수 있는 인증방식이 없어서 Twitter의 주도로 OAuth 1.0이 탄생하였습니다.

<br/>

### 비밀번호 인증방식의 문제

* 신뢰: 사용자가 애플리케이션에 ID/PW를 제공하기 꺼려함
* 피싱에 둔감해짐: 각 종 애플리케이션들에 ID/PW 를 계속 제공하는 경우
* 접근범위가 늘어남에 따른 위험 부담: ID/PW를 모두 알고 있는 애플리케이션은 모든 권한을 가짐
* 신뢰성의 제한: PW 를 변경한다면 애플리케이션은 동작을 하지 못하게 됨
* 폐기문제: 권한을 폐기할 수 있는 유일한 방법이 PW를 변경하는 것,

<br/>

## OAuth 구성(1.0a)

![](https://i2.wp.com/earlybird.kr/wp-content/uploads/2013/02/oauth2_triangle2.png?w=624)

3-legged-auth로 유저(user), 컨슈머(consumer), 서비스 프로바이더(service provider)로 구성되어 있다. 

**그런데 OAuth 1.0은** 구현이 복잡하고 웹이 아닌 어플리케이션의 자원이 부족하였다. HMAC을 통해 암호화를 하는 번거로운 과정을 겪는다. 또한 인증토큰(access token)이 만료되지 않았다.

<br/>

# OAuth 2.0

## 달라진 점

* 기능의 단순화, 기능과 규모의 확장성 등을 지원하기 위해 만들어 졌다
* 1.0a는 만들어진 다음 표준이 된 반면 2.0은 처음부터 표준 프로세스로 만들어짐.
* https가 필수여서 간단해 졌다.
* 암호화는 https에 맡김.
* 1.0a는 인증방식이 한가지였지만 2는 다양한 인증방식을 지원한다.
* api서버에서 인증서버를 분리 할 수 있도록 해 놓았다.

<br/>

## OAuth 2.0 구성

* Resource Owner: 서드파티 애플리케이션 (Google, Facebook, Kakao 등)에 이미 개인정보를 저장(회원가입)하고 있으며 Client가 제공하는 서비스를 이용하려는 사용자이다. 'Resource'는 개인정보라고 생각하면 된다.

* Client: Resource Server 에서 제공하는 자원을 사용하는 애플리케이션이다. 즉, OAuth 2.0을 사용해 서드파티 로그인 기능을 구현할 자사 또는 개인 애플리케이션 서버이다.

* Resource Server(API server): 사용자의 개인정보를 가지고 있는 애플리케이션 (Google, Facebook, KaKao 등) 서버이다. Client는 Token을 이 서버로 념겨 개인정보를 응답 받을 수 있다.

* Authorization Server: 권한을 부여(인증에 사용할 아이템을 제공)해주는 서버이다. 사용자는 이 서버로 ID, PW를 넘겨 Authorization Code를 발급 받을 수 있다. Client는 이 서버로 Authorization Code를 넘겨 Token을 발급 받을 수 있다.

<br/>

## 인증과정

#### 페이코 개발자센터 OAuth 2.0 프로세스

![](https://developers.payco.com/static/img/@img_guide.jpg)

<br/>

## OAuth 인증 프로세스 (Authorization Code Grant)

![](https://developers.payco.com/static/img/@img_guide2.jpg)

발급받은 Access Token은 서비스에서 자체적으로 저장, 관리해야 한다.

<br/>

## 인증 종류

OAuth 2.0의 인증종류는 아래 4가지가 있다.

* Authorization Code Grant
* Implicit Grant
* Resource Owner Password Credentials Grant
* Client Credentials Grant

<br/>

|  | **Confidential Client** | **Public Client** |
|:----:|:----:|:----:|
| 3-legged | Authorization Code | Implicit |
| 2-legged | Resource Owner Password Credentials | Client Credentials |

2-legged는 잘 쓰이지 않는다.

<br/>

### Authorization Code Grant

* 서버사이드 코드로 인증하는 방식
* 권한서버가 클라이언트와 리소스서버간의 중재역할.
* Access Token을 바로 클라이언트로 전달하지 않아 잠재적 유출을 방지.
* 로그인시에 페이지 URL에 response_type=code 라고 넘긴다.

<br/>

### Implicit Grant

* token과 scope에 대한 스펙 등은 다르지만 OAuth 1.0a과 가장 비슷한 인증방식
* Public Client인 브라우저 기반의 어플리케이션(Javascript application)이나 모바일 어플리케이션에서 이 방식을 사용하면 된다.
* OAuth 2.0에서 가장 많이 사용되는 방식이다.
* 권한코드 없이 바로 발급되서 보안에 취약
* 주로 Read only인 서비스에 사용.
* 로그인시에 페이지 URL에 response_type=token 라고 넘긴다.

<br/>

### Password Credentials Grant

* Client에 아이디/패스워드를 저장해 놓고 아이디/패스워드로 직접 access token을 받아오는 방식이다.
* Client 를 믿을 수 없을 때에는 사용하기에 위험하기 때문에 API 서비스의 공식 어플리케이션이나 믿을 수 있는 Client에 한해서만 사용하는 것을 추천한다.
* 로그인시에 API에 POST로 grant_type=password 라고 넘긴다.

<br/>

### Client Credentials Grant

* 어플리케이션이 Confidential Client일 때 id와 secret을 가지고 인증하는 방식이다.
* 로그인시에 API에 POST로 grant_type=client_credentials 라고 넘긴다.

<br/>

### Access Token

앞서 말한 4가지 권한 요청 방식 모두, 요청 절차를 정상적으로 마치면 클라이언트에게 Access Token이 발급됩니다. 이 토큰은 보호된 리소스에 접근할 때 권한 확인용으로 사용됩니다. 문자열 형태이며 클라이언트에 발급된 권한을 대변하게 됩니다. 계정 아이디와 비밀번호 등 계정 인증에 필요한 형태들을 이 토큰 하나로 표현함으로써, 리소스 서버는 여러 가지 인증 방식에 각각 대응 하지 않아도 권한을 확인 할 수 있게 됩니다.

<br/>

### Refresh Token

한번 발급받은 Access Token 은 사용할 수 있는 시간이 제한되어 있습니다. 사용하고 있던 Access Token 이 유효기간 종료 등으로 만료되면, 새로운 액세스 토큰을 얻어야 하는데 그때 이 Refresh Token 이 활용됩니다. 권한 서버가 Access Token 을 발급해주는 시점에 Refresh Token 도 함께 발급하여 클라이언트에게 알려주기 때문에, 전용 발급 절차 없이 Refresh Token을 미리 가지고 있을 수 있습니다. 토큰의 형태는 Access Token과 동일하게 문자열 형태입니다. 단 권한 서버에서만 활용되며 리소스 서버에는 전송되지 않습니다.

<br/>

### 토큰의 갱신 과정

클라이언트가 권한 증서를 가지고 권한서버에 Access Token 을 요청하면, 권한 서버는 Access Token과 Refresh Token 을 함께 클라이언트에 알려줍니다. 그 후 클라이언트는 Access Token을 사용하여 리소스 서버에 각종 필요한 리소스들을 요청하는 과정을 반복합니다. 그러다가 일정한 시간이 흐른 후 액세스 토큰이 만료되면, 리소스 서버는 이후 요청들에 대해 정상 결과 대신 오류를 응답하게 됩니다. 오류 등으로 액세스 토큰이 만료됨을 알아챈 클라이언트는, 전에 받아 두었던 Refresh Token을 권한 서버에 보내어 새로운 액세스 토큰을 요청합니다. 갱신 요청을 받은 권한 서버는 Refresh Token 의 유효성을 검증한 후, 문제가 없다면 새로운 액세스 토큰을 발급해줍니다. 이 과정에서 옵션에 따라 Refresh Token 도 새롭게 발급 될 수 있습니다.

<br/>

**참조**

* [원본링크1](https://hwannny.tistory.com/92)
* [원본링크2](https://showerbugs.github.io/2017-11-16/OAuth-%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C)
* [OAuth와 춤을](https://d2.naver.com/helloworld/24942)