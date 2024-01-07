---
title: 깃허브 블로그 만들기3 - 댓글 기능 구현 (Feat. Utterances)
categories: blog
tags: jekyll

toc: true
toc_sticky: true
---
[깃허브 블로그 만들기2 - 로컬PC에서 실시간 변경사항 확인](https://isckd.github.io/2024-01-01-make-github-blog(2)) 에 이어서 포스팅한다.

# 1. 댓글 라이브러리 선정
댓글 기능으로 유명한 라이브러리로는 여러가지가 있다.
- Disqus
- Utterances
- Giscus

각각의 장단점을 보면서 어떤 댓글 라이브러리를 사용해볼지 생각해보자.

### Disqus
![10](https://github.com/isckd/blog-comment/assets/100770637/95554a8c-3864-4477-8458-685a46fb1466)
소셜 플랫폼으로 로그인해 댓글 작성이 가능하다.
Utterances, Giscus 는 Github 계정으로 댓글을 다는 방식에 비해 접근성이 비교적(?) 좋다. <br>
하지만 개발 블로그를 보는 사람은 대부분 Github 계정이 있으니 이 부분은 패스. <br>
그리고 댓글 기능 치고 무거운 편인데다가 무료 라이센스일 경우 광고가 붙는 아이러니한 상황이 발생한다.
### Utterances
![11](https://github.com/isckd/blog-comment/assets/100770637/f025cd99-89c6-4ec9-af0f-47999897ecd9)
Github Repository 의 Issue 를 활용하여 댓글을 다는 방식이다.<br>
물론 Github 계정이 있어야 댓글을 달 수 있지만 크게 문제가 되지는 않는다.<br>
구현 방식이 간편하고, markdown 문법을 지원하며, 댓글(이슈) 알림이 가능하다.
### Giscus
Github Repository 의 Discussions 를 활용하여 댓글을 다는 방식이다.<br>
Utterances 와 마찬가지로 댓글 작성자가 Github 계정이 있어야 하며, 비슷한 구현 및 기능을 보유하고 있다.<br>
댓글 기능이 Issus 가 아닌 Discussions 에 있다는 것이 취지에 조금 더 맞다는 의견이 많아, <br>
최근에는 Utterances -> Giscus 로 이관하는 추세로 보인다.<br>
하지만 Github Repository 와 연동하려고 하니, **2024.01.01 기준 보안 관련 이슈가 있는지<br>
연동이 되질 않아 Utterances 으로 선정하였다. <br>**
**2024.01.01 기준 Giscus 를 사용하는 모든 블로그들의 댓글 기능이 먹통이 됐다 ㅋㅋ<br>
이 글을 보고 있다면 지금은 해결이 잘 됐는지 한 번 둘러보고 댓글을 남겨주었으면 한다.** <br>
[개발블로그로 유명한 jojoldu 의 블로그](https://jojoldu.tistory.com/704)
![12](https://github.com/isckd/blog-comment/assets/100770637/d8c7f819-d585-47b6-8129-f9cc836617a6)


# 2. Utterances 설치 및 연동
우선 댓글이 달릴때마다 issue 에 저장될 Repository 를 생성한다. <br>
private 이 아닌 public 으로 생성함에 유의하자.
![13](https://github.com/isckd/blog-comment/assets/100770637/7d69f74d-04ee-4418-afd8-67817a1c98e7)

[install Utterances](https://github.com/apps/utterances) 에서 Repository 와 연동하자.
![14](https://github.com/isckd/blog-comment/assets/100770637/000f15f1-140e-47e4-8964-25c28e9fdb66)

install 을 진행 후, 아래 화면에서 방금 생성한 repository 와 연동한다.
![15](https://github.com/isckd/blog-comment/assets/100770637/9b1b9e79-99f4-4644-b009-83c373401501)

[Utterances 연동](https://utteranc.es/) 에서 연동 작업을 추가로 진행하자.
![16](https://github.com/isckd/blog-comment/assets/100770637/47df89d1-f321-4bc8-9bd5-17e12ad876b1)

스크롤을 아래로 내리다보면, 여러가지 입력 및 선택창이 보인다. <br>
repo: 에는 {Github계정명}/{생성한 Repository명} 을 작성하고, <br>
블로그 글 경로를 이슈의 제목으로 선택한다.
![17](https://github.com/isckd/blog-comment/assets/100770637/479651be-3d52-4815-9991-9585a9d28e45)

label 은 선택이니, 편한대로 작성한다.<br>
Theme 는 현재 내 Jekyll 테마인 beautiful-Jekyll 기준 GitHub Light 가 가장 어울려 선택했다.<br>
여기까지 완료했다면 내가 설정한 것을 기준으로 코드가 작성되는데, 이를 복사한다.
![18](https://github.com/isckd/blog-comment/assets/100770637/b56ad605-a635-49ca-86d8-a9b985068b34)

이제 코드를 삽입하는 과정만 남았다.<br>
댓글은 포스팅 글에만 적용한다는 것을 가정하고, Jekyll 테마 기준으로 포스팅글은<br>
**_layouts/post.html** 에 작성된다.<br>

기존에는 가장 하단에 아래 코드가 삽입되어 있었는데, 우리는 무료버전 Jekyll 을 사용하므로 <br>
comments.html 파일 자체가 없다. 그래서 {% include comments.html %} 부분을 <br>
주석처리하고 방금 복사한 코드를 넣자.<br>
```html
      <script src="https://utteranc.es/client.js"
        repo="isckd/blog-comment"
        issue-term="pathname"
        label="comments"
        theme="github-light"
        crossorigin="anonymous"
        async>
      </script>
```

댓글 기능이 완료되었다!
![11](https://github.com/isckd/blog-comment/assets/100770637/f025cd99-89c6-4ec9-af0f-47999897ecd9)

다음 포스팅 내 Github Pages 를 구글 검색에 노출시키는 방법에 대해 올릴 예정이다.<br>
[깃허브 블로그 만들기4 - 구글 검색 노출시키기](https://isckd.github.io/2024-01-01-make-github-blog(4))