---
title: 깃허브 블로그 만들기4 - 구글, 네이버, 다음 검색 노출시키기 (Google search console, Naver search advisor, 다음 검색 엔진, SEO)
categories: blog
tags: jekyll SEO

toc: true
toc_sticky: true
---
[깃허브 블로그 만들기3 - 댓글 기능 구현 (Feat. Utterances)](https://isckd.github.io/blog/make-github-blog(3)) 에 이어서 포스팅한다.

## SEO 란?
궁금한 점이 생겼을 때, Google, Naver 등에 검색하여 답을 얻는 것이 일반적이다. <br>
물론 AI 가 생기고 판도가 많이 달라졌지만 근본적으로 개발 관련 검색엔진은 구글, 일상 관련 검색엔진은 네이버라고 본다.<br>
SEO 란 검색 엔진 최적화, 즉 구글, 네이버 등의 웹사이트의 검색 엔진에서 상위에 노출될 수 있도록 최적화 하는 과정이다. <br>
SEO 에 대해서 더 자세히 알아보고 싶다면 아래 링크로 들어가자. <br>
WIX 라는 홈페이지 제작 툴을 홍보하는 글이긴 하지만 정리가 잘 되어있다.<br>
[WIX Blog - SEO](https://ko.wix.com/blog/post/what-is-seo)


# 1. Google search console 설정
Github Pages 에 Push 를 해도 실제 구글에 잘 노출되지 않을 것이다. <br>
벨로그와 같은 플랫폼과 달리 구글에 노출되도록 하는 설정을 직접 추가해주어야 한다. <br>
[Google search console](https://search.google.com/search-console/about) 에서 진행하면 된다. <br>
![스크린샷 2024-01-02 212644](https://github.com/isckd/isckd.github.io/assets/100770637/58cd4336-828d-4e61-a5a0-7a50a01df159)

시작하기를 누르면 아래와 같은 화면이 나오는데, <br>
URL 접두어에서 github Pages 홈 주소를 넣으면 된다.<br>
ex) https://{Github계정명.github.io}<br>
![google search console 접속화면](https://github.com/isckd/isckd.github.io/assets/100770637/5e083815-c4c1-4ab3-913f-ff2bdf64e72c)

URL 확인 후 소유권 확인을 위해 다음과 같은 화면이 출력된다.
- HTML 파일 : 웹사이트에 HTML 파일 업로드 **(권장)**
- HTML 태그 : 사이트 홈페이지에 메타태그 추가
- Google 애널리틱스 : Google 애널리틱스 계정 사용
- Google 태그 관리자 : Google 태그 관리자 계정 사용
- 도메일 이름 공급업체 : DNS 레코드와 Google 연결

표시된 .html 파일을 다운로드 받는다.
![URL 접두어 선택](https://github.com/isckd/isckd.github.io/assets/100770637/371a972c-d372-4e4f-999d-adb92bb19db9)

{: .notice--info}
이후 해당 파일을 로컬PC의 블로그 디렉토리 내부에 넣는다.<br>
_config.yml 파일이 존재하는 곳에 넣으면 된다. <br>
로컬에서 테스트 후 나중에 Push 하자. <br>
http://127.0.0.1:4000/{HTML파일명}.HTML 이 브라우저에서 잘 출력되는지 확인한다. <br>
그리고 Push 한 뒤 '확인' 을 눌러 구글측에 .html 파일 검증을 한다.

잘 완료되었다면 아래와 같은 화면이 출력된다. 속성으로 이동하자.
<br><br>
![스크린샷 2024-01-02 215303](https://github.com/isckd/isckd.github.io/assets/100770637/a02c954f-abf4-480a-81c9-073ac3bcf012)

여기까지는 이 URL 이 등록만 시켜둔 것이고, 추가로 글을 읽어올 수 있도록<br>
sitemap.xml 및 robots.txt 파일이 필요하다.<br>
원래는 아래 Gemfile에 적용된 jekyll-sitemap 과 jekyll-feed 를 통해 자동으로 생성되어야 하나,<br>
Jekyll이 워낙 버전을 많이 타다보니 수동으로 생성하자. (당한 게 많다..)
```ruby
group :jekyll_plugins do
  gem "jekyll-paginate"
  gem "jekyll-sitemap"
  gem "jekyll-gist"
  gem "jekyll-feed"
  gem "jemoji"
  gem "jekyll-algolia"
  gem 'jekyll-include-cache'
  gem 'github-pages'
  gem 'faraday-retry'
  gem 'csv'
end
```

<br>

이전과 같이 _config.xml 파일이 위치한 곳과 동일한 곳에 <br>
sitemap.xml, robots.txt 파일을 작성하자.
### sitemap.xml
~~~xml
---
layout: null
---
{% raw %}
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://www.sitemaps.org/schemas/sitemap/0.9 http://www.sitemaps.org/schemas/sitemap/0.9/sitemap.xsd"
        xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
    {% for post in site.posts %}
    <url>
        <loc>{{ site.url }}{{ post.url }}</loc>
        {% if post.lastmod == null %}
        <lastmod>{{ post.date | date_to_xmlschema }}</lastmod>
        {% else %}
        <lastmod>{{ post.lastmod | date_to_xmlschema }}</lastmod>
        {% endif %}

        {% if post.sitemap.changefreq == null %}
        <changefreq>weekly</changefreq>
        {% else %}
        <changefreq>{{ post.sitemap.changefreq }}</changefreq>
        {% endif %}

        {% if post.sitemap.priority == null %}
        <priority>0.5</priority>
        {% else %}
        <priority>{{ post.sitemap.priority }}</priority>
        {% endif %}

    </url>
    {% endfor %}
</urlset>
{% endraw %}
~~~

실제로 [localhost:4000/sitemap.xml](http://localhost:4000/sitemap.xml) 로 접속해 잘 나오는지 확인하자.

### robots.txt
```
User-agent: *
Allow: /

Sitemap: https://{Github계정명}.github.io/sitemap.xml
```

위 파일들을 repository에 Push 하고 이전에 '속성으로 이동' 한 페이지로 이동하자.<br>
닫아버렸다면 [https://search.google.com/search-console](https://search.google.com/search-console) 로 이동하자.<br>
Sitemaps 탭으로 이동 후, url 입력창에 sitemap.xml 을 입력 후 추가해보자.<br>
그러면 '제출된 사이트맵' 에 정상적으로 등록됨을 볼 수 있다.<br>

{: .notice--info}
**곧바로 '성공' 상태가 뜨지 않을 수 있다. <br>
이제부터는 google 측에서 승인해주기를 기다리면 된다.<br>
나 같은 경우에는 바로 성공이 떴지만 실시간으로 구글에 노출되지는 않았다.<br>
노출이 되는 시점에 시간이 얼마나 소요되었는지 공유하겠다. (2024.01.02 등록)**

![스크린샷 2024-01-02 222458](https://github.com/isckd/isckd.github.io/assets/100770637/df29167c-9145-46a4-822f-b2b1580c9b7b)

아래 탭에서 실제 등록되었는지 확인이 가능한 듯 하다.
![스크린샷 2024-01-02 223303](https://github.com/isckd/isckd.github.io/assets/100770637/68f192c2-2e39-4aaa-ac17-2b62705db406)

<br>

***

<br>

# 2. Naver search advisor 설정
[Naver search advisor](https://searchadvisor.naver.com) 에서 진행이 가능하다.
<br><br>
![스크린샷 2024-01-03 191010](https://github.com/isckd/isckd.github.io/assets/100770637/6a8fe5e1-1873-4830-b587-ec36dab88286)
<br><br>
이후 https://{Github계정명}.github.io 를 입력한다.
![스크린샷 2024-01-03 193352](https://github.com/isckd/isckd.github.io/assets/100770637/6a3f0fec-dddd-461e-ae2d-0ffafeda5ca8)
<br><br>
이후 파일을 HTML 파일을 다운로드, 내 로컬 PC 의 블로그 디렉토리에 옮긴 다음 Push 한다.<br>
Google 에 등록했을 때와 과정이 비슷하다.<br>
Push 가 완료되면 소유확인을 진행한다.
<br><br>
![스크린샷 2024-01-03 193558](https://github.com/isckd/isckd.github.io/assets/100770637/0460e421-a5fa-4682-8e44-82bba7b4c74c)
<br><br>
.html 검증이 완료되면 해당 링크를 클릭해 설정으로 진입한다.
<br><br>
![스크린샷 2024-01-03 194834](https://github.com/isckd/isckd.github.io/assets/100770637/03810cb6-be3c-4db1-b904-ed36a3f97ace)
<br><br>
요청 탭의 사이트맵 제출 탭에 진입해 https://{Github계정명}.github.io 를 입력한다.
<br><br>
![스크린샷 2024-01-03 200845](https://github.com/isckd/isckd.github.io/assets/100770637/b02c54b2-f7cf-4208-870f-d7cbfec18bf1)
<br><br>

{: .notice--danger}
~~2024.01.03 기준 네이버에 sitemap.xml 을 제출하는 과정 중에 오류가 발생하였다.<br>
분명 google 에 제출한 [sitemap.xml](#sitemapxml) 과 동일한데, 401 에러가 발생한다.<br>
다른 브라우저로 변경해도 동일하다. 원인을 도통 모르겠다.<br>
그래서 다른 sitemap.xml 으로 변경하려고 했으나, sitemap.xml 을 생성해주는 사이트에서는
내 블로그의 글들을 동적으로 긁어오는 것이 아니라 현재 포스팅된 글을 기준으로만 가져오기 때문에
차후 작성하는 글들에 대해서 동적으로 검색하지 않는 아이러니한 상황이었다.
구글에 제출한 sitemap.xml 은 liquid 문법으로 내 포스팅 글들을 동적으로 가져온다.
혹시라도 이 문제에 대해 아는 사람이 있다면,<br> 코멘트로 남겨주면 매우 고마울 것 같다.<br>~~
![스크린샷 2024-01-03 201206](https://github.com/isckd/isckd.github.io/assets/100770637/f429f3c9-42cd-4eb7-94dc-f9a2ee2eb075)
![스크린샷 2024-01-03 201348](https://github.com/isckd/isckd.github.io/assets/100770637/dd459064-a8da-4871-8091-8e30287ee8ec)
<br><br>

{: .notice--danger}
~~이 원인이 맞는지는 모르겠지만 [네이버 웹마스터 가이드](https://searchadvisor.naver.com/guide/request-crawl) 를 보면 수집 요청 결과의 상태가 <br>
'요청완료' 인 상태라서 인식하지 못하는 것일까도 싶다. <br>
처리결과 상태값이 변경되면 다시 시도해 볼 예정이다. <br>~~
네이버에는 GITHUB 블로그 등록이 되지 않는 것으로 보인다. 더 이상 기다리지 않을 예정이다.
![스크린샷 2024-01-03 203107](https://github.com/isckd/isckd.github.io/assets/100770637/319b4718-36e2-4f55-aeff-350f87887d60)


sitemap.xml 이 정상적으로 제출된다면, robots.txt 와 rss 도 같이 제출하면 된다.

***

<br>

# 3. 다음 검색 엔진 설정
[다음 검색등록](https://register.search.daum.net/index.daum) 에서 진행이 가능하다.
<br><br>
{Github계정명.github.io} 를 입력 후 확인으로 진입한다.
![스크린샷 2024-01-03 194436](https://github.com/isckd/isckd.github.io/assets/100770637/4b388b09-dc40-4334-9c04-937cd4dc8bfa)
<br><br>
이후 이메일을 입력하면 정상적으로 완료된다. <br>
sitemap.xml 이나 robots.txt 를 제출하지 않아도 완료된다.<br>
간편하긴 하지만 그만큼 검색률과 신뢰도가 떨어지는 것에는 감수하자.
<br><br>
![스크린샷 2024-01-03 202120](https://github.com/isckd/isckd.github.io/assets/100770637/5d0f580c-5d68-4343-9f27-7449a6b461db)

이어서는 포스트 우측에 스크롤 위치에 따라 동적으로 움직이는 목차 탭을 만들어보자.
[깃허브 블로그 만들기5 - 목차에 따른 ScrollSpy 구현 (Table of Contents, TOC)](https://isckd.github.io/blog/make-github-blog(5))