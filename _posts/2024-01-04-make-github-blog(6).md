---
title: 깃허브 블로그 만들기6 - 지킬 테마 마이그레이션 beautiful-jekyll -> minimal-mistakes
categories: blog
tags: jekyll minimal-mistakes

toc: true
toc_sticky: true
---
[깃허브 블로그 만들기5 - 목차에 따른 ScrollSpy 구현 (Table of Contents, TOC))](https://isckd.github.io/blog/make-github-blog(5)) 에 이어서 포스팅한다.

***

<br>
# 1. 마이그레이션 준비 및 로컬 구동
<br>
테마 마이그레이션을 위해 기존 테마를 다 갈아엎을 필요가 있다. <br>
우선 기존 작업한 파일들이 필요하니 백업 후 테마를 집어넣자.<br>
기존 {Github계정명}.github.io 디렉토리에 내부 파일들을 복사해 눈에 잘 띄는 곳에 두고, <br>
[minimal-misaktes Theme Github Repository](https://github.com/mmistakes/minimal-mistakes) 에서 ZIP 파일로 다운받아 {Github계정명}.github.io 파일에 넣는다. <br><br>
![Download ZIP](https://github.com/isckd/blog-comment/assets/100770637/8597d778-98b9-4237-bfd6-14c05903c729)
<br>

{: .notice--info}
이후, 기존대로 bundle exec jekyll serve 를 진행하지 말고 GemFile 이 초기화 되었으니 기존 GemFile 을 덮어씌우고, 로컬 구동한다.

```ruby
bundle install
bundle exec jekyll serve
```

{: .notice--warning}
하지만 구동이 실패할 것이다. Minimal-mistakes 테마는 기본적으로 블로거의 이미지(favicon)를 등록하는 것이
Default 라 에러메시지가 발생할 것이다. 차근차근 진행해보자.

우선 브라우저 탭에 보여질 이미지로 어떤 것을 넣을지 선택한다.<br>
이후, 이것을 웹 아이콘으로 설정하기 위해 [realfavicongenerator.net](https://realfavicongenerator.net/) 에 진입한다.<br><br>
![2024-01-08 21 22 45](https://github.com/isckd/blog-comment/assets/100770637/5d7f4e0e-a326-430f-926d-b171236afab3)
<br><br>
이후 다음 화면의 최하단에서 아래와 같이 진행한다.<br>
![2024-01-08 21 23 50](https://github.com/isckd/blog-comment/assets/100770637/ba1cd00f-4255-409f-9344-3159bcf47bcf)
<br><br>

정상적으로 진행이 되었다면 **Favicon package** 를 다운로드 받고, 출력된 HTML5 코드를 사용해 favicon 을 적용해보자.<br><br>
![2024-01-08 21 26 17](https://github.com/isckd/blog-comment/assets/100770637/e68f3196-29c6-4f1e-89f3-fcea5bc3c4d0)
<br><br>
/assets 디렉토리에 logo.ico 라는 디렉토리를 생성하고, 다운로드 받은 package 를 집어넣는다. <br>
이후 HTML5 코드를 참고해 /_includes/_head/custom.html 의 코드를 아래와 같이 수정한다.

```html
<!-- start custom head snippets -->

<!-- insert favicons. use https://realfavicongenerator.net/ -->
<link rel="apple-touch-icon" sizes="180x180" href="{{site.baseurl}}/assets/logo.ico/apple-touch-icon.png">
<link rel="icon" type="image/png" sizes="32x32" href="{{site.baseurl}}/assets/logo.ico/favicon-32x32.png">
<link rel="icon" type="image/png" sizes="16x16" href="{{site.baseurl}}/assets/logo.ico/favicon-16x16.png">
<link rel="mask-icon" href="{{site.baseurl}}/assets/logo.ico/safari-pinned-tab.svg" color="#5bbad5">
<meta name="msapplication-TileColor" content="#da532c">
<meta name="theme-color" content="#ffffff">

<!-- end custom head snippets -->
```
<br>
이제 다시 아래 코드를 실행하면 구동이 잘 되면서, 브라우저 상단의 탭에 웹 아이콘이 잘 박혀 나오는 모습을 볼 수 있다.
```ruby
bundle install
bundle exec jekyll serve
```
![2024-01-08 21 38 15](https://github.com/isckd/blog-comment/assets/100770637/4047b984-16e7-430b-8a84-46988da8cb54)
<br><br>

***

# 2. 기초 세팅
기존 beautiful-jekll 테마와 마찬가지로 /_config.yml 디렉토리에 전반적인 블로그의 설정을 할 수 있다.<br>
Jekyll 테마는 굵직한 부분에서는 공통점이 있으니 다시 다른 테마로 마이그레이션 한다고 하더라도 알아두자.<br>

주석을 참고하며 세팅을 진행한다. 아래 파라미터들은 블로그의 전반적인 소개를 나타낸다.<br>
```yml
minimal_mistakes_skin    : "default" # 블로그의 바탕 스킨
                        # "air", "aqua", "contrast", "dark", "dirt", "neon", "mint", "plum", "sunrise"

# Site Settings
locale                   : "ko-KR"  # 블로그의 주요 언어
title                    : "MUD_COOKIE 개발블로그"  # 브라우저 상단에서 보이는 이름
title_separator          : "-"
subtitle                 : "2년차 백엔드 개발자"    # 화면 title 하단에 있는 소제목
name                     : "MUD_COOKIE"            # footer 영역 이름
description              : "백엔드 개발일지"            # description
url                      : "https://isckd.github.io" # blog url
baseurl                  : # "/blog"                # fallback url
repository               : #
teaser                   : # path of fallback teaser image, e.g. "/assets/images/500x300.png"
logo                     : # path of logo image to display in the masthead, e.g. "/assets/images/88x88.png"
masthead_title           : "MUD_COOKIE"     # 화면 title
```

<br>

아래 파라미터들은 sidebar 와 footer 의 기본 소개를 나타낸다.<br>
url 을 주석처리하면 아이콘 자체가 나타나지 않으므로, 참고해 작성하자.<br>

```yml
# Site Author
author:
  name             : "MUD_COOKIE"
  avatar           : # path of avatar image, e.g. "/assets/images/bio-photo.jpg"
  bio              : "2년차 Spring 백엔드 개발자"
  location         : "Republic of Korea"
  email            :
  links:
    - label: "Email"
      icon: "fas fa-fw fa-envelope-square"
      url: "mailto:isckd3@gmail.com"        # mail 주소 입력
    - label: "Website"
      icon: "fas fa-fw fa-link"
      # url: "https://your-website.com"     # url 주석처리 시 나타나지 않는다.
    - label: "Twitter"
      icon: "fab fa-fw fa-twitter-square"
      # url: "https://twitter.com/"
    - label: "Facebook"
      icon: "fab fa-fw fa-facebook-square"
      # url: "https://facebook.com/"
    - label: "GitHub"
      icon: "fab fa-fw fa-github"
      url: "https://github.com/isckd"
    - label: "Instagram"
      icon: "fab fa-fw fa-instagram"
      # url: "https://instagram.com/"

# Site Footer
footer:
  links:
    - label: "Twitter"
      icon: "fab fa-fw fa-twitter-square"
      # url:
    - label: "Facebook"
      icon: "fab fa-fw fa-facebook-square"
      # url:
    - label: "GitHub"
      icon: "fab fa-fw fa-github"
      url: "https://github.com/isckd"
    - label: "GitLab"
      icon: "fab fa-fw fa-gitlab"
      # url:
    - label: "Bitbucket"
      icon: "fab fa-fw fa-bitbucket"
      # url:
    - label: "Instagram"
      icon: "fab fa-fw fa-instagram"
      # url:
```
<br>
아래 코드는 최상위 디렉토리 내부의 .md 파일들이 어떠한 기본값을 가질건지 설정하는 값이다.<br>
_pages 디렉토리는 초기 프로젝트에 없었을 것이니 새로 만들어준다. 물론 최상단 디렉토리에 만들어야 한다.<br>
여기서 잠깐 언급하자면, _posts 디렉토리는 블로거가 포스팅할 글을 넣는 디렉토리이고, <br>
_pages 는 포스팅과 관련된 카테고리나 태그 등을 설정하는 디렉토리라고 보면 된다. <br>

{: .notice--info}
전반적인 코드 예시는 /docs/ 디렉토리에서 확인할 수 있다. 예제가 아주 잘 나와있으니 나중에 꼭꼭 참고하길 바란다.


```yml
# Defaults
defaults:
  # _posts                      : _posts 디렉토리 내 .md 파일들의 기본 설정값
  - scope:
      path: ""
      type: posts
    values:
      layout: single
      author_profile: true
      read_time: true
      show_date: true           # 포스팅 날짜를 표시한다.
      comments: true            # 댓글 기능 활성화 (하단에서 추가 작업 예정)
      share: true
      related: true
      sidebar_main: true        # sidebar 를 표시한다. (나중에 카테고리 등 커스텀할 때 쓰임)
      
  # _pages                      : _pages 디렉토리 내 .md 파일들의 기본 설정값
  - scope:
      path: "_pages"
      type: pages
    values:
      layout: single
      author_profile: true
      sidebar_main: true
```
<br>

***

<br>

# 3. 포스팅 이관
<br>
앞서 말했듯이 _posts 디렉토리에 기존에 작성했었던 포스팅.md 파일들을 옮긴다.<br>
제일 먼저 해야할 것은 .md 파일의 최상단에 있는 파라미터들을 조금 수정해주어야 한다.<br>
기존에 있던 값들을 지워버리고, 아래와 같은 파라미터에서 값만 바꾸자.<br>

참고할 점은 categories, tags (카테고리, 태그)의 value 에는 띄어쓰기를 기준으로 값을 구분하므로, <br>
띄어쓰기에 유의하길 바란다.<br>

추가적으로 toc 가 기본적으로 내장되어있다.. 이전 포스팅에서 사이드에 적용하려고 귀찮은 작업을 했던 점이<br>
minimal-mistakes 에서는 아주 간단하게 구현이 가능하다. <br>
toc_sticky 는 스크롤을 해도 고정해버리겠다는 뜻이다.
<br>

```
---
title: "깃허브 블로그 만들기6 - 지킬 테마 마이그레이션 beautiful-jekyll -> minimal-mistakes"
categories: blog
tags: jekyll

toc: true
toc_sticky: true
---
```

추가적으로 beautiful-jekyll 에서 사용했던 {: .box-note} 와 같은 것들을 box 들을 모두 버리고, <br>
아래 중에서 골라 사용하자.
```
{: .notice}
{: .notice--primary}
{: .notice--warning}
{: .notice--success}
{: .notice--danger}
{: .notice--info}
```

{: .notice}
notice 예시

{: .notice--primary}
primary 예시

{: .notice--warning}
warning 예시

{: .notice--success}
success 예시

{: .notice--danger}
success 예시

{: .notice--info}
info 예시

<br>

## nav 카테고리 적용
<br>
_data/navigation.yml 파일에서 categories 와 tags 를 적용한다.<br>

```yml
# main links
main:
  - title: "Quick-Start Guide"          # 아직까지는 Guide 에서 배울 것이 많으니 남겨둔다.
    url: https://mmistakes.github.io/minimal-mistakes/docs/quick-start-guide/
  - title: "About"                      # 이 링크에서도 minimal-mistakes 의 팁들이 많으니 남겨둔다.
    url: https://mmistakes.github.io/minimal-mistakes/about/
  # - title: "Sample Posts"             # 주석처리한 것들은 필요없다.
  #   url: /year-archive/
  # - title: "Sample Collections"
  #   url: /collection-archive/
  # - title: "Sitemap"
  #   url: /sitemap/
  # - title: "Introduce"
  #   url: /about/
  - title: "Category"                   # 카테고리 적용
    url: "/categories/"
  - title: "Tags"                       # 태그 적용
    url: /tags/
```

<br><br>

***

# 4. 댓글 기능 이관
<br>
다시 /_config.yml 파일을 수정한다. 위에서 comments: true 를 적용한 상태여야 한다. <br>
만약 댓글 기능을 아직 적용해보지 않았다면 [깃허브 블로그 만들기3 - 댓글 기능 구현 (Feat. Utterances)](https://isckd.github.io/blog/make-github-blog(3)) <br>
을 참고하길 바란다.<br>
utterances 를 그대로 사용할 예정이다.<br>

```yml
comments:
  provider               : utterances
  # false (default), "disqus", "discourse", "facebook", "staticman", "staticman_v2", "utterances", "giscus", "custom"
  utterances:
    theme                : "github-light"
    issue_term           : "pathname"
```

까지만 적용하고 _includes\comments-providers\utterances.html 파일을 수정한다.

```html
<script>
  'use strict';

  (function() {
    var commentContainer = document.querySelector('#utterances-comments');

    if (!commentContainer) {
      return;
    }

    var script = document.createElement('script');
    script.setAttribute('src', 'https://utteranc.es/client.js');
    script.setAttribute('repo', '{Github계정명/comment용 repository명}');
    script.setAttribute('issue-term', '{{ site.comments.utterances.issue_term | default: "pathname" }}');
    // {% if site.comments.utterances.label %}script.setAttribute('label', '{{ site.comments.utterances.label }}');{% endif %}
    script.setAttribute('label', 'comments')
    script.setAttribute('theme', '{{ site.comments.utterances.theme | default: "github-light" }}');
    script.setAttribute('crossorigin', 'anonymous');

    commentContainer.appendChild(script);
  })();
</script>
```

{: .notice--warning}
다만 위 minimal-mistakes 기능을 활용해 utterances 을 적용하면 로컬 구동 시 댓글창이 보이지 않는 이슈가 있다.<br>
이는 Push 해보고 동작 확인을 해보자.

<br><br>

***

# 5. 검색엔진 노출을 위한 파일 이관
<br>

SEO, 즉 검색 노출에 대한 
- {google~~긴hash값}.html 
- sitemap.xml
- robots.txt

파일을 마저 이관하자.<br>
상단에 언급했듯이 백업은 해두었길 바란다.<br>
만약 구글, 네이버, 다음에 대한 검색 노출을 적용해보지 않은 사람이라면 내가 이전에 작성한 포스트<br>
[깃허브 블로그 만들기4 - 구글, 네이버, 다음 검색 노출시키기 (Google search console, Naver search advisor, 다음 검색 엔진, SEO)](https://isckd.github.io/blog/make-github-blog(4))<br>
를 참고하기 바란다.

<br><br><br>

***

**reference**

아래는 내가 참고한 링크이다. 이 포스팅에서는 사이드바 카테고리에 대해 다루지 않았지만, 아주 정리가 잘 되어있어 첨부한다.<br>
[minimal-mistakes A to Z](https://eona1301.github.io/a_to_z/GithubBlog/) <br>
[카테고리 만들기](https://ansohxxn.github.io/blog/category/) <br>
[카테고리 만들기- github repository](https://github.com/ansohxxn/ansohxxn.github.io/blob/master/) <br>
