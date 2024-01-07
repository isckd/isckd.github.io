---
title: 깃허브 블로그 만들기5 - 목차에 따른 ScrollSpy 구현 (Table of Contents, TOC)
categories: blog
tags: jekyll

toc: true
toc_sticky: true
---
[깃허브 블로그 만들기4 - 구글, 네이버, 다음 검색 노출시키기 (Google search console, Naver search advisor, 다음 검색 엔진, SEO)](https://isckd.github.io/2024-01-01-make-github-blog(4)) 에 이어서 포스팅한다.

# 1. TOC
글을 읽을 때 목차가 있다면 원하는 부분으로 쉽게 이동이 가능하고, 가독성이 좋아진다.<br>
블로그를 커스텀하다보니 사이드바가 너무 허전하다는 느낌을 받아, 이번엔 목차(toc) 를 적용하고자 한다.

***

<br>

# 2. TOC 구현하기
이제는 적용을 해보자. Jekyll 이 좋은게, toc 플러그인까지도 지원을 한다.<br>
이래서 레퍼런스가 많은 라이브러리를 사용해야 함을 다시금 느낀다.


## 2.1 Default TOC 적용

[jekyll-toc doc](https://github.com/toshimaru/jekyll-toc) 에 적용 방법이 나와있다.
우선 Gemfile 에 아래 내용을 추가한다.
<br><br>
```ruby
gem 'jekyll-toc'
```
이후 _config.yml 파일에 아래 내용을 추가한다.

{: .box-warning}
들여쓰기를 하지 않는다는 것에 유의하자. 공백으로 대체.<br>
들여쓰기 때문에 구동 시 parsing 에러 때문에 시간을 많이 잡아먹었다.

```ruby
plugins:
  - jekyll-toc
```

이후 모든 post 의 .md 파일의 가장 윗부분에 아래 내용을 적용한다.

```
---
layout: post
toc: true

...

---
```

post.html 파일에서 아래 내용을 적용한다. 포스팅하는 .md 파일의 body에 적용한다는 뜻이다.<br>
원래는 toc 없이 {% raw %} {{ content }} {% endraw %} 만 있었을 것이다.

{% raw %}
```html
<div class="blog-post">
    {{ content | toc }}
</div>
```
{% endraw %}



이제 로컬에서 구동시켜보자.
<br><br>
![default toc 적용](https://github.com/isckd/isckd.github.io/assets/100770637/d70705ad-2d53-436a-bb29-b92d3c7128c9)
<br><br>

잘 적용된 것을 볼 수 있다.이제 커스텀을 해보자.

## 2.2 jekyll-toc doc 가이드에 따라 커스텀해보기
위에서 언급했던 [jekyll-toc doc](https://github.com/toshimaru/jekyll-toc) 에 가이드가 잘 나와있다. 순서대로 적용해보자.<br><br>

### 2.2.1 TOC 형식 지정
_config.yml 파일에 아래 내용을 수정하며 커스텀이 가능하다.<br>
아래 값이 default 이니 처음에 적용하지 않아도 잘 적용되었을 것이다.<br>

```yml
toc:
  min_level: 1
  max_level: 6
  ordered_list: false
  no_toc_section_class: no_toc_section
  list_id: toc
  list_class: section-nav
  sublist_class: ''
  item_class: toc-entry
  item_prefix: toc-
```

{: .box-note}
각 파라미터는 아래와 같은 의미를 지닌다. <br><br>
min_level : html 의 <h1> ~ <h6> 의 레벨 중에 어느 레벨부터 적용할 건지 <br>
max_level : html 의 <h1> ~ <h6> 의 레벨 중에 어느 레벨부터 적용할 건지 <br>
ordered_list : 아이콘 대신 1. 2. 3 의 형식의 목차를 사용할건지 <br>
no_toc_section_class : toc 를 사용하지 않는 html class 을 명명한다. <br>
list_id : 생성되는 리스트의 html id 를 명명한다. <br>
list_class : 생성되는 리스트의 html class 를 명명한다. <br>
...

<br>

### 2.2.2 TOC CSS 적용
방금 위에서 적용한 list_class 명을 기준으로 아래 코드에 적용시킨다. <br>
base.html 에 적용하면 된다. base 라는 layout 은 모든 html 파일에도 적용되어 있어 나는 base.html 에 적용했다.

```css
.section-nav {
  background-color: #fff;
  margin: 5px 0;
  padding: 10px 30px;
  border: 1px solid #e8e8e8;
  border-radius: 3px;
}
```
위와 같이 기본 가이드대로 적용하면 아래와 같이 출력된다.<br>
디자인이라고 하기엔 민망하니 적용되었다는 것만 보고 css 는 알아서 커스텀하자.
<br><br>
![doc guide toc css](https://github.com/isckd/isckd.github.io/assets/100770637/fcc28299-29d8-4329-8fff-520eaf3c6f6a)


{: .box-warning}
sidebar 뿐 아니라 이것저것 커스텀하다보니 지금껏 사용한 beautiful-jekyll 테마는 레퍼런스도 적고 귀찮은게 한두가지가 아니었다.<br>
커스텀하기 쉬운 테마를 찾다보니 minimal-mistake 가 가장 github star 수가 많은 걸 보아 그 테마로 넘어가고자 한다.
