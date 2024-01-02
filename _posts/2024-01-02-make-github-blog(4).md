---
layout: post
title: 깃허브 블로그 만들기4 - 구글 검색 노출시키기 (Google search console, SEO)
subtitle: jekyll 을 사용한 Github blog 제작기
# gh-repo: daattali/beautiful-jekyll
# gh-badge: [star, fork, follow]
tags: [Github, SEO]
# comments: true
author: mud_cookie
---
[깃허브 블로그 만들기3 - 댓글 기능 구현 (Feat. Utterances)](https://isckd.github.io/2024-01-01-make-github-blog(3)) 에 이어서 포스팅한다.

# 1. Google search console 기본 설정
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

{: .box-note}
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

<br><br>

# 2. Google search console 연동

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

{: .box-note}
**곧바로 '성공' 상태가 뜨지 않을 수 있다. <br>
이제부터는 google 측에서 승인해주기를 기다리면 된다.<br>
나 같은 경우에는 바로 성공이 떴지만 실시간으로 구글에 노출되지는 않았다.<br>
노출이 되는 시점에 시간이 얼마나 소요되었는지 공유하겠다. (2024.01.02 등록)**

![스크린샷 2024-01-02 222458](https://github.com/isckd/isckd.github.io/assets/100770637/df29167c-9145-46a4-822f-b2b1580c9b7b)

아래 탭에서 실제 등록되었는지 확인이 가능한 듯 하다.
![스크린샷 2024-01-02 223303](https://github.com/isckd/isckd.github.io/assets/100770637/68f192c2-2e39-4aaa-ac17-2b62705db406)


구글 검색 노출을 추가한 김에 Daum, Naver 에도 노출되도록 설정하고자 한다.<br>
이어서는 일별, 총 노출수 등도 사이드 탭에 추가해보도록 하자.