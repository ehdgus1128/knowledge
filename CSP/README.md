

# CSP (Content Security Policy)

  

## 용어
신뢰된 [웹 페이지](https://ko.wikipedia.org/wiki/%EC%9B%B9_%ED%8E%98%EC%9D%B4%EC%A7%80 "웹 페이지") 콘텍스트에서 악의적인 콘텐츠를 실행하게 하는 [사이트 간 스크립팅](https://ko.wikipedia.org/wiki/%EC%82%AC%EC%9D%B4%ED%8A%B8_%EA%B0%84_%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8C%85)(XSS), [클릭재킹](https://ko.wikipedia.org/wiki/%ED%81%B4%EB%A6%AD%EC%9E%AC%ED%82%B9 "클릭재킹"), 그리고 기타 [코드 인젝션](https://ko.wikipedia.org/wiki/%EC%BD%94%EB%93%9C_%EC%9D%B8%EC%A0%9D%EC%85%98 "코드 인젝션") 공격을 예방하기 위해 도입된 [컴퓨터 보안](https://ko.wikipedia.org/wiki/%EC%BB%B4%ED%93%A8%ED%84%B0_%EB%B3%B4%EC%95%88 "컴퓨터 보안") 표준

## 문제 발생
게시판 작업 진행 시 input/textarea 내에 script 태그를 입력.저장 후 read 했을 때  script가 실행되는 XSS 문제 발생.
```html
<script>alert(1)</script>

<img src="#" onclick=alert('1')>
```
대부분은 이것으로 쿠키를 가로채어(쿠키 로그인) 정보를 탈취하는 데 목적을 둔다고 알고 있다. 브라우저가 내 태그와 삽입된 태그를 모두 유효하게 실행하여 문제가 된다.   
단순히 script 태그를 특정 문자로 바꿔버리는 방법을 생각하거나(<script ->'NO')  src="#" 이미지를 엑박으로 교체하는 방법을 생각하던 중 알게 된 CSP.

## 해결
1. HTTP 헤더 제어
```
Content-Security-Policy: policy
```
policy 는 여러가지 보안 정책을 둘 수 있다. ( default-src 'self' 등 참고 : <https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP>)

2. meta 태그 삽입
```html
<meta  http-equiv="Content-Security-Policy"
content="default-src 'none'"  />
```
---
(기존 소스 수정 방법)

## 결론 및 느낀점
CSP는 최대한 제한적인 정책을(어떤 도메인은 어플리케이션 접근 가능한지, 어떤 리소스는 유효 한지 등 정책) 적용하여 XSS를 막는 데 큰 도움을 줄 수 있는 도구라고 배웠다.  이후로 소스 작업에서 CSP를 사용하지 않은 프로젝트들도 많았지만, 개인적으로 XSS는 조심하는 계기가 되었고 그 외 웹 취약성, 보안 이쪽으로도 더욱 고민하며 임하게 되었다.

## 번외
- https://owasp.org/www-community/xss-filter-evasion-cheatsheet   
XSS Cheat Sheet를 보는데 테스트 해보고 대응해야 하는 것이 엄청 많다. 적당히 충분히 예방하는 것이 중요할 것 같다.

## 참고
1. <https://ko.wikipedia.org/wiki/%EC%BD%98%ED%85%90%EC%B8%A0_%EB%B3%B4%EC%95%88_%EC%A0%95%EC%B1%85>
2. <https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP>
3. https://developers.google.com/web/fundamentals/security/csp