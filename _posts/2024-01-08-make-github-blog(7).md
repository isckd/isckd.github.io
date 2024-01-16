---
title: 깃허브 블로그 만들기7 - 주관적인 포스팅 팁 모음
categories: blog
tags: jekyll minimal-mistakes

toc: true
toc_sticky: true
---
minimal mistakes 포스팅 팁 모음집

***

# Minimal-Mistakes 테마 관련

<br>

## 문자 박스

{: .notice}
{% raw %} {: .notice} 문자 박스 {% endraw %}

{: .notice--primary}
{% raw %} {: .notice--primary} 문자 박스 {% endraw %}

{: .notice--warning}
{% raw %} {: .notice--warning} 문자 박스 {% endraw %}

{: .notice--success}
{% raw %} {: .notice--success} 문자 박스 {% endraw %}

{: .notice--danger}
{% raw %} {: .notice--danger} 문자 박스 {% endraw %}

{: .notice--info}
{% raw %} {: .notice--info} 문자 박스 {% endraw %}

<br><br>

***

# 이미지 업로드

<br>
이미지 캡처 후 바로 편집이 필요하고, <br>
이미지를 repository 에 직접 올리자니 용량이 쌓여 불편해서 내가 업로드하는 방식을 공유한다.<br>

[pickpick 다운로드](https://picpick.net/download/) 에서 다운로드를 진행한다.<br>
다운로드 후 옵션에 진입한다.<br>

![스크린샷 2024-01-16 204755](https://github.com/isckd/blog-comment/assets/100770637/b5d43fab-f563-48bf-ad32-caa138ebe58d)<br>

이후 옵션 탭에서 지정 화면 캡처 단축키를 본인 입맛에 맞게 설정한다.<br>
Ctrl + Shift + S 는 Intellij 에서 Setting 을 들어가는 단축키이지만 나는 크게 개의치 않아 이대로 진행한다.<br>

![스크린샷 2024-01-16 204911](https://github.com/isckd/blog-comment/assets/100770637/d9bff1b6-6b88-4522-bfb3-5e25a422e9c5)<br>

캡처 탭에서 캡처 후 캡처된 이미지에 대해 무엇을 실행할 것인지를 지정한다. <br>
나는 픽픽에디터로 바로 편집할 수 있게 지정했다.<br>

![스크린샷 2024-01-16 205456](https://github.com/isckd/blog-comment/assets/100770637/31a090e3-82b7-4047-9cea-825500926a11)<br>

이후 자동 저장 탭에서 자동 저장을 활성화하고, 위치는 본인이 편한 디렉토리로 지정한다.<br>

![스크린샷 2024-01-16 205513](https://github.com/isckd/blog-comment/assets/100770637/83d15a72-f4f5-4753-a9a1-4dc4eeedfbde)<br>

단축키 실행 시 마우스 드래그로 캡처가 가능하다.<br>

![스크린샷 2024-01-16 205303](https://github.com/isckd/blog-comment/assets/100770637/fa56c2bb-526b-4092-be41-75183a9bb939)<br>

여기까지가 이미지 캡처 후 바로 편집을 간편하게 하는 방법이다. <br>
이제는 repository 에 직접 이미지를 올리고 push 하는 방법이 아닌 github issue 서버에 올리는 형식으로 변경해보자.<br>

우선 본인 repository 중 아무 소스코드도 없는 repository 의 issue 탭에 진입해 new issue 를 클릭한다.<br>
없으면 빈 repository 를 만들고, issue 탭이 없다면 Settings 탭에서 활성화가 가능하다.<br>

![2024-01-16 21 01 15](https://github.com/isckd/blog-comment/assets/100770637/9147835f-fea1-4b71-a767-deeeec9b43e4)<br>

이후 캡처 및 편집한 이미지를 드래그해 description 에 드롭한다. 복사 & 붙여넣기도 가능하다.<br>
그러면 텍스트가 한 줄 나오는데, 이를 복사해 마크다운 포스팅 파일에 붙여넣으면 바로 이미지가 적용된다.<br>
본인 블로그 repository 용량 문제 신경쓸 필요 없고, 이미지를 가져다 대기만 해도 <br>
내 블로그와 무관한 서버에 업로드가 되니 내가 본 방법 중에는 가장 합리적인 선택이다.<br>

![2024-01-16 21 08 04](https://github.com/isckd/blog-comment/assets/100770637/e5285eda-48d9-41ab-ac49-e01940a20e96)

<br><br>

***

# 자주 안쓰여서 잘 까먹는 마크다운 문법 (Minimal-Mistakes)
<br>

## 표 작성

### 기본 표 작성

```
|column1|column2|column3|column4|
|---|---|---|---|
|content1|content2|content3|content4|
|content5|content6|content7|content8|
|content9|content10|content11|content12|
```

|column1|column2|column3|column4|
|---|---|---|---|
|content1|content2|content3|content4|
|content5|content6|content7|content8|
|content9|content10|content11|content12|

### 열 정렬 (왼쪽, 가운데, 오른쪽)

```
|기본값|왼쪽정렬|가운데정렬|오른쪽정렬|
|---|:---|:---:|---:|
|content1|content2|content3|content4|
|content5|content6|content7|content8|
|content9|content10|content11|content12|
```

|기본값|왼쪽정렬|가운데정렬|오른쪽정렬|
|---|:---|:---:|---:|
|content1|content2|content3|content4|
|content5|content6|content7|content8|
|content9|content10|content11|content12|

<br>

## 코드블럭 특수문자 코드 인식

{% raw %}
~~~
```
{%{% endraw %} raw {% raw %}%} {% endraw %} {% raw %}
특수문자 입력해도 문자열로 인식! !@#$%^&*
{%{% endraw %} endraw {% raw %}%} {% endraw %} {% raw %}
```
~~~
{% endraw %}


***

<br><br>

# 카테고리 관련

[카테고리 만들기- github repository](https://github.com/ansohxxn/ansohxxn.github.io/blob/master/) 기준 <br>
- 새로운 카테고리 만들 떄 /_pages/categories 에서 새 파일 만들어야 함.
- nav_list_main 에서 좌측 사이드바 카테고리에 카테고리 추가 및 상위폴더 추가 가능.



{: .notice}
{: .notice--primary}
{: .notice--warning}
{: .notice--success}
{: .notice--danger}
{: .notice--info}