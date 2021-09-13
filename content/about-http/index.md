---
emoji: 📡                                                                                                                   
title: HTTP
date: '2021-06-18 16:09:38'
author: 이준영
tags: HTTP
categories: Web
---

# HTTP란?

**HyperText Transfer Protocol**의 약자로 World Wide Web상에서 정보를 주고 받을 수 있는 프로토콜이며 다시 말해 컴퓨터들끼리 HTML파일을 주고받을 수 있도록 하는 소통방식 또는 약속

---

## HTTP통신의 특징


### 1. Request / Response (요청/응답)

보내는 주체와 받는 주체가 있으며 사람도 소통하려면 보내는 것과 받는 것이 있듯이 컴퓨터도 사람과의 소통방식과 큰 차이가 없음
`Client`에서 `Request`, `Server`에서 `Response`해주는 것이 한 세트
> **HTTP 통신의 핵심은 요청과 응답이다!**

### 2. Stateless

각각의 HTTP 통신은 모두 독립적이기 때문에 과거의 통신의 결과를 보존하지 않음
즉, **매 통신마다 필요한 모든 정보를 담아서 요청을 보내야 함**
>여러번의 통신 진행과정에서 연속된 데이터 처리가 필요한 경우를 위하여 로그인 토큰, 브라우저의 쿠키, 세션, 그리고 로컬 스토리지 같은 기술이 필요에 의해 만들어짐

만약 통신이 독립적이지 않고 내용을 전부 다 저장해버리면 각 통신간의 진행이나 연결상태 처리까지 관리해야 하여 정보가 섞이고 충돌될 수 있음

---

## Request / Response 구조
프로젝트를 진행할 때 FE에서 BE에게 데이터를 요청하고 BE는 요청을 처리해서 응답을 주는데 이 Request와 Response에 대한 구조와 메세지를 잘 파악하면 대부분의 에러를 잡아낼 수 있음
### 1. Request의 구조
>**Request는 Message에 불과하다!**

HTTP요청은 `Client`에서 `Server`에 데이터 처리를 시작하게 하기 위해 보내는 메세지이며 Request의 구조는 크게 세 부분으로 구성되어 있음

#### 첫 번째 : Start Line
Request의 첫 번째 줄이며 세 부분으로 구성되어 있음

1. `HTTP Method`
>해당 요청의 의도한 action을 정의하는 부분이며 주로 `GET`, `POST`, `DELETE`가 많이 쓰임
2. `Request target`
> 해당 Request가 전송되는 목표 `url`
3. `HTTP Version`
> 말 그대로 사용되는 HTTP버전을 뜻하고 주로 1.1 버전이 널리 쓰임

EX👇🏻
```http
GET /login HTTP/1.1
>>> GET method로 login이라는 Request Target에 HTTP 1.1버전으로 Request
```



#### 두 번째 : Headers
해당 Request에 대한 추가 정보(메타 데이터)를 담고 있는 부분
>`Key: Value`값으로 되어있음 (`JS`의 `Object`, `Python`의 `Dictionary`형태)

- 자주 사용되는 Headers의 정보👇🏻
```
Headers: {
  Host: 요청을 보내는 Target의 주소 | Request하는 웹사이트의 기본 주소가 됨
  User-Agent: 요청을 보내는 Client의 정보 (ex. chrome, firefox, etc..)
  Content-Type: 해당 요청이 보내는 메세지 body의 Type (ex. application/json)
  Content-Length: body 내용의 길이
  Authorization: 회원의 인증/인가를 처리하기 위해 로그인 토큰을 담음
}
```


#### 세 번째 : Body
해당 요청의 실제 내용이며 주로 `Body`를 사용하는 `Method`는 `POST`이며 요청 `Method`에 따라 존재하지 않을 수 있음
```
Body: {
  "user_email": "google_god@gmail.com"
  "user_pwd": "GoDGoOglE!@"
}

```


---

### 2. Response의 구조

>**Response도 Request와 마찬가지로 Message다!**

Response의 구조도 크게 세 부분으로 구성되어 있음

#### 첫 번째 : Status line
Response의 첫 번째 줄이며 Response는 Request에 대한 처리상태를 `Client`에 알려주며 내용을 시작함

1. `HTTP Version`
>`Request`의 `HTTP Version`과 동일함
2. `Status Code`
>`Response Message` 의 상태 코드
3. `Status Text`
>`Response Message`의 상태를 간략하게 설명해주는 `Text`

EX👇
```
HTTP/1.1 404 Not Found
>>>HTTP 1.1버전으로 응답하고 있으나 FE에서 보낸 요청(ex. 로그인 시도 등)에 대해 유저의 정보를 찾을 수 없어서 
   404 상태 메세지를 보냄
   
   
HTTP/1.1 200 SUCCESS
>>>FE에서 보낸 요청에 대해 성공하여 200 상태 메세지를 보냄
```
#### 두 번째 : Headers
Request의 Headers와 동일하나 Response에서만 사용되는 Header의 정보들이 있음

#### 세 번째 : Body
Request의 Body와 일반적으로 동일하며 Request의 `Method`에 따라 Body가 존재하지 않을 수 있듯이 Response도 형태에 따라 데이터를 전송할 필요가 없는 경우엔 Body가 없을 수도 있음
>가장 많이 사용되는 Body의 타입은 [`JSON(LINK)`](https://velog.io/@ambitiouskyle/JSON%EC%9D%B4-%EB%8F%84%EB%8C%80%EC%B2%B4-%EB%AD%98%EA%B9%8C) 이다!

---
## HTTP Request Methods
`GET`, `POST`, `DELETE` 세 가지만 정리 해보려고 한다
>해당 `Method`들은 `Client`의 입장에서 Request의 의도가 담긴 것으로 `Server`의 입장에서 생각하면 혼란이 올수도 있으니 주의❗❗❗❗❗

### 1. GET
- 데이터를 서버로 부터 받아올 때 주로 사용
- 데이터를 받아오기만 할 때 사용됨
> 가장 간단하고 많이 사용되는 `HTTP Method`임
-> 웹 페이지를 띄울 때 필요한 정보들을 모두 `GET`으로 Request를 보내 받아온 것들을 화면에 띄움

### 2. POST
- 데이터를 생성 / 수정 할 때 주로 사용
- 대부분의 경우 Request에 Body가 포함되어 보내짐

### 3. DELETE
- 특정 데이터를 서버에서 삭제 요청을 보낼 때 사용
  `ex. 장바구니에서 제품 삭제 등`

---

## Response Status Codes
`Status Code`의 숫자에 각각 의미가 내포되어 있고 `Status Code`만 봐도 응답의 정상여부 파악가능
- `200: OK`
>문제 없이 Request에 대한 처리가 `Server`에서 이루어지고 나서 오는 `Status Code`

- `201: Created`
>무언가가 잘 생성되었을때 오는 `Status Code`

- `400: Bad Request`
>해당 Request가 잘못되었을 때 오는 `Status Code`
>
>`ex. int형으로 받아야 하는데 str로 오는 경우 등`
- `401: Unauthorized`
>Request를 진행하려면 먼저 로그인을 하거나 회원가입이 필요하다는 의미
- `403: Forbidden`
> - 해당 요청에 대한 권한이 없다는 의미
>- 접근 불가능한 정보에 접근했을 경우
- `404: Not Found`
> 요청된 `URL`이 존재하지 않는다는 의미
- `500: Internal Server Error`
> `Server`에서 에러발생시 볼 수 있는 `Status Code`
> 
> 💢💢💢💢**API개발을 하는 BE 개발자들이 싫어함**💢💢💢💢

----

## 세 줄 요약👀
1. HTTP는 컴퓨터 끼리의 소통을 위한 통신규약
2. HTTP 통신은 **Request**와 **Response**로 이루어져 있음
3. HTTP 통신의 매 Request와 Response는 독립적으로 과거의 상태를 알지 못함 - **Stateless**


---

<br>
잘못된 부분은 Feedback 부탁드립니다👊🏻👊🏻👊🏻👊🏻👊🏻👊🏻👊

```toc
```