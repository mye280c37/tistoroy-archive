# 비밀번호 암호화는 어디서 이루어져야 하는가?

## 비밀번호 암호화는 어디서 이루어져야 하는가?

보안을 위해서 비밀번호 암호화는 필수이다. 하지만 다음과 같은 질문이 생긴다.

<ol>
<li> 보안을 위해 애초에 프론트 단에서 비밀번호를 암호화해서 request를 보내야 하는가? </li>
<li> 그냥 plain text를 보내고 백엔드 단에서 비밀번호를 암호화해서 데이터베이스에 저장해야할까? </li>
</ol>

1번이 좀 더 보안 측면에서 좋지 않을까 하는 생각이 들지만,

Https를 사용한다면 일단 Https의 자체적인 보안이 있기 때문에 plain text를 보내도 괜찮다. 애초에 해커가 request 자체를 가져가버린다면 request가 사용자에게서 온건지 해커에게서 온건지 구분이 불가능하다.

굳이 보안에 더 신경쓴다면 양방향 암호화를 통해 프론트단과 백엔드단에서 각각 다른 방식의 암호화를 사용하는 방식도 있다.

누가 보내는지도 확인하기 위해 ip주소나 기타 식별할 수 있는 데이터를 같이 섞어 암호화할 수도 있지만 이 또한 웹페이지의 경우 우리가 짠 코드가 브라우저 서버에서 돌아가고, java script가 콘솔창에 다 공개되어 있기 때문에 큰 의미는 없다.

## 결론

(Https를 사용한다는 전제 하에) 프론트 단에서는 평문으로 비밀번호를 보내고 암호화는 백엔드 단에서 하자

## reference

- [https://okky.kr/questions/696808](https://okky.kr/questions/696808)
- [https://www.palindrom615.dev/client-side-hashing-is-not-helping](https://www.palindrom615.dev/client-side-hashing-is-not-helping)