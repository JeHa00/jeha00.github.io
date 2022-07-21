# 0. Introduction

- window 10 환경에서 Hugo 라는 SSG의 한 종류를 사용해서 총 6단계를 거쳐서 `<user-id>.github.io` 주소의 github page를 만들고 배포한 후, contents를 업로드하는 것까지 내용을 다룬다. 

> 1. [Static Site Generator 로 Hugo를 선택한 이유](#1-static-site-generator-로-hugo를-선택한-이유)
> 2. [Github blog 만들기 위한 local 환경 조성](#2-github-page-만들기-위한-local-환경-조성)
> 3. [새로운 2개 github repo 와 local 연결하기](#3-새로운-2개-github-repo-와-local-연결하기)
> 4. [Github page에 theme 적용](#4-github-page에-theme-적용)
> 5. [Contents 생성과 업로드, utterances로 댓글 기능 추가, deploy.sh로 자동화](#5-contents-생성과-업로드)
> 6. [Utterances로 댓글 기능 추가, deploy.sh로 자동화](utterances로-댓글-기능-추가-deploysh로-자동화)

<br>

- 마지막으로, 테마를 커스텀마이징하는 지름길과 후기를 남겼다. 

> 7. [To customize blog theme & 후기](#6-to-customize-blog-theme-&-후기)

- 위 과정들에서 필요한 개념들, 부딪혔던 error 및 해결책도 각 단계 마지막 부분에 작성했다.

<br>

🚩 동일한 주제로 다룬 블로그들을 보면 3단계와 4단계의 순서를 바꿔서 진행한다. 4단계 진행 후, 3단계를 진행해도 무방하다. 다만, 나는 그 과정에서 헷갈린 부분이 있어서 마지막에 theme 적용을 하기로 선택했다.

<br>

---

# 1. Static Site Generator 로 Hugo를 선택한 이유

<br>

## 1.1 SSG란?

- `Github` page를 만들 때 SSG의 종류들로 'Jekyll', 'Hexo', 'Hugo' 가 많이 언급된다. 

- 그러면 SSG란 무엇인가?? 
	- '정적 페이지(Static Site)'란 HTML, CSS, JS를 미리 올려서 서버가 바뀌지 않는 HTML page를 보여주는 것을 말하는데, 이 정적 페이지를 보다 간편하게 만들어주는 것이 `SSG(Static Site Generator)`다.   

<br>

> 이와 반대로 동적 페이지는 client에 반응하여 HTML page를 동적으로 만들어진 페이지를 말한다. 더 상세한 정보를 원하시는 분은 [정적 웹은 뭐고 동적 웹은 뭔가요?](https://bit.ly/3gVLiIi) 이 영상을 참고하시길 바란다.  

<br>

## 1.2  SSG의 종류와 Hugo를 선택한 이유

- hugo를 선택하기에 앞서 각 SSG의 특징들에 대해 알아야 하기 때문에, `Jekyll`, `Hexo`, `Hugo`의 각 특징들은 다음과 같다.

```
 Jekyll
  -루비 기반
  -현재 가장 인기 있음(깃헙 별 수 제일 많음)
  -한글 레퍼런스도 제일 많음
  -느리다는 제보가 많음(몇 십개의 포스팅 뿐인데도 빌드 하는데 5분씩 걸린다고)
  -윈도우 공식 지원 안됨

Hexo
  -자바스크립트(Node.js) 기반
  -한글 레퍼런스 꽤 많음
  -메인 개발자가 손을 놓은 듯
  -개발자가 중국인? 이라 구글링하면 중국어 글이 많이 나옴

Hugo
  -Golang 기반
  -빌드 빠름
  -문서화 잘돼있음
  -깃헙 별 수가 헥소보다 많음
  -한글 레퍼런스가 거의 없음

출처: http://tadakichi.tistory.com/188
```

- 그래서 4가지 이유로 Hugo를 선택했다.
	- 남들이 안해본 걸 해보자.
	- 한글 레퍼런스가 거의 없기 때문에, 내가 기여할 수 있는 부분이 다른 것보다 있을 것이다.
	- 내가 원하는 디자인 대부분이 Hugo였다.
	- 앞으로 계속해서 기술 블로그를 작성할 것이기 때문에, 빠른 빌드를 원했다.

<br>

---

# 2. Github page 만들기 위한 local 환경 조성

> 첫 번째, git을 설치한다. 그리고, github 에 가입한다.  
> 두 번째, Visual Studio Code (VSC), Atom 같은 에디터를 설치한다.  
> 세 번째,  window 환경에 Hugo를 설치한다.  


## 2.1 첫 번째

- github 가입은 [Github](https://github.com/) 에 들어가서 오른쪽 상단에 있는 `Sign up`을 클릭하여 진행한다. 
- 그러면 최종적으로 `https://github.com/user-name/` 을 가진다.    
- github page의 url은 `[user-name].github.io` 로 가진다.  

<br>

## 2.2 두 번째

- 나는 visual studio code를 사용한다. [visual studio code](https://code.visualstudio.com/) 여기에 들어가 설치한다.

<br>

## 2.3 세 번째

- window 환경에 hugo를 설치한다. [window에서 Hugo 설치하기](https://gohugo.io/getting-started/installing#windows) 이 영상 하나 따라하면 쉽다. 하지만, 글로 보고 싶은 분들을 위해 작성한다.
    - [hugo 다운로드](https://github.com/gohugoio/hugo/releases) 로 들어가서 아래로 scroll을 내리면 window 버전을 다운받아 `C:\Hugo\bin` 디렉토리를 생성해서 다운받은 압축 파일을 해제
    - 어느 위치에서나 Hugo가 실행할 수 있도록 `윈도우 검색`으로 `시스템 환경 변수 편집`을 검색하여 들어간다. `고급` 탭의 `환경 변수` 로 들어간다. `사용자 변수` 란의 `path`를 클릭 후, `편집` 을 클릭한다. `새로 만들기`를 클릭하여 `C:\Hugo\bin` 경로를 추가한다.
    - cmd에 `echo %PATH%` 를 입력하여 추가한 경로가 있는지 확인한다.
    - 해제한 압축 파일에서 hugo 실행하여 설치 후, cmd에 `hugo version` 으로 동작 확인한다.

<br>

---
# 3. 새로운 2개 github repo 와 local 연결하기

## 3.1 Submodule 개념 이해하기

❗ 이런 개념이 있구나 정도만 이해하고 3.1을 넘어가기. 이해하기 어렵다면 생략하고 바로 다음 3.2 chapter로 넘어가 따라해보자. 

<br>

- 이 단계를 진행하기 전에 `submodule` 개념을 알아야 한다. 영어 독해가 가능하신 분들은 [How to Set Up a Hugo Site on Github Pages - with Git Submodules!](https://bit.ly/3sM7daw) 이 링크에 들어가 보시기 바란다. submodule에 대해 그림과 함께 잘 설명되어있다.
아래 내용은 위 블로그에서 submodule에 대한 부분을 번역한 내용이다. 오역이 있다면 댓글로 알려주시면 감사하겠다.

- `public` folder는 [3.4 B repo를 public 폴더에 submodule로 연결하기](#34-b-repo를-public-폴더에-submodule로-연결하기) 파트에서 아래 명령어로 만들어진다.  

    ```yml
    # blog 폴더의 submodule로 branch main에 B repo를 add 한다.
    # sample: git submodule add https://github.com/JeHa00/JeHa00.github.io.git public
    > git submodule add -b main https://github.com/<user-name>/<B repo 명칭>.git public
    ```

    ![image](https://www.adamormsby.com/posts/000/how-to-set-up-a-hugo-site-on-github-pages-with-submodules/img/explain-submodules.jpg)

    출처: [How to Set Up a Hugo Site on Github Pages - with Git Submodules!](https://bit.ly/3sM7daw)



> 왜 Git submodule인가??  
> 모든 git project는 repository에 저장된다.  
> 이 git submodule은 한 레포 안에서 다른 레포를 참조하도록 해준다.  
> 그래서 프로젝트 안에 프로젝트를 효과적으로 운영할 수 있다.  
> 중요한 건 submodule은 main project와 달리 자신만의 commit과 branch histroy를 가진다.   
> 그래서 프로젝트들을 분리시킬 수 있다. 이는 매우 강력한 도구다.    
> 아래 그림에서는 git project에서 submodule을 사용할 시, 어떻게 코드가 포함되는지를 보여준다.    
> ....  
> 메인 repository의 submodule로 `public` folder를 하위 폴더로 설정하여, 독립된 branch history를 갖는 개체로 대할 수 있다.  
> 출처: https://www.adamormsby.com/posts/000/how-to-set-up-a-hugo-site-on-github-pages-with-submodules/  


<br>

- A repo가 `blog` 에 `remote origin`으로 연결된다.
- B repo가 `public` 폴더 형태로, `blog`의 submodule로 들어간다.

    ![image](https://www.adamormsby.com/posts/000/how-to-set-up-a-hugo-site-on-github-pages-with-submodules/img/submodules-diagram-with-public.jpg)

    출처: [How to Set Up a Hugo Site on Github Pages - with Git Submodules!](https://bit.ly/3sM7daw)

<br>

---

## 3.2 New repository 2개 만들기

> 자신의 github에 2개의 repository를 만든다.



- 2개의 repository를 각각 A,B라고 하자.
- 2개의 repository는 `public`과 `private` 중 `public`으로 만든다. `private` 으로 하면 site에 배포가 안될 수도 있다.
- A는 `user-name/blog` 로, B는 `user-name/user-name.github.io` 로 명칭을 만든다.  
  ex) A의 url은 `github.com/JeHa00/blog` / B의 url은 `github/JeHa00/JeHa00.github.io`
- B repo의 이름이 github page로 쓰일 url.


❗❗ **주의: 두 repo를 만들 때 주의사항**  

- A repo에는 README.md 만들지 말기  
    - README.md가 있다면 나중에 `git push` 시에 충돌이 일어난다.

- B repo에 README.md를 만들기  
    - repo가 비어있으면 submodule로 연결이 안된다.


<br>

## 3.3 Hugo new site 생성 및 remote add origin A repo 실행

-  Visual Studio Code의 terminal 또는 Window의 cmd에 입력한다.

```yml
경로 C:\Hugo

# 새로운 Hugo project 생성
# sample: Hugo new site blog
> Hugo new site <project 명칭>

# project folder인 blog로 이동
# sample: cd blog
> cd <project 명칭>

#3. local git 초기화
> git init

# blog의 remote origin으로 A repo 추가하기
# sample: git remote add origin https://github.com/Jeha00/blog.git
> git remote add origin https://github.com/<user-name>/<A repo 명칭>.git

# commit 하기 위해 모든 파일을 stage에 올리기
> git add .

# commit
> git commit -m "add origin"

# the remote origin 에 push 하겠다. branch는 master로 하겠다.
> git push origin master
```

- `remote origin`이 잘 되었는지 확인하기 위해서는 `git remote -v`를 입력하면 뜬다.

- `error: src refspec master does not match any` 가 뜬다면 이는 stage에 오른 파일이 없다는 의미이므로, `git add .` 를 했는지 확인하기
- `hugo new site <project 명칭>` 명령으로 local에서 컨텐츠를 관리하기 위한 장소 생성

- 이 때 경로는 `C:\Hugo` 인 상태에서 terminal에 입력한다.
- `<project 명칭>`을 A repo 이름과 똑같이 한다.
- project를 새로 생성해서 project 파일 경로는 `C:\Hugo\<project 명칭>` 일 것이다.
- 위 명령어로 생긴 tree는 다음과 같다.

```yml
Hugo/project 명칭
├─archetypes
├─content
├─data
├─layouts
├─static
├─themes
└─config.toml
```

- `git remote add origin https://github.com/<user-name>/<A repo 명칭>.git` Hugo new site로 만든 project 에 대한 remote origin으로 A repo를 추가하겠다는 의미다.

- `C:\Hugo\<project 명칭>` 경로에서 git push를 하면 앞으로 A repo에 저장된다.
- 그 결과, A repo안에 구성은 다음과 같다.

```yml
<user-name>/A repo 이름
├─archetypes
└─config.toml
```

<br>

## 3.4 B repo를 public 폴더에 submodule로 연결하기

```yml
# blog 폴더의 submodule로 branch main에 B repo를 add 한다.
# sample: git submodule add https://github.com/JeHa00/JeHa00.github.io.git public
> git submodule add -b main https://github.com/<user-name>/<B repo 명칭>.git public
```

- 이 명령어로 public 폴더가 생성되고, 이 폴더의 remote origin이 B repo가 된다.
- `public` 폴더가 생긴 걸 알 수 있다.

```yml
Hugo/project 명칭
├─archetypes
├─content
├─data
├─layouts
├─public
├─static
├─themes
├─.gitmodules
└─config.toml
```

- 하지만 `public` 폴더가 생긴다고 연결된 게 아니다. 확실하게 연결이 되었다면 `.gitmodules` 파일이 생기고, 이 안에 아래와 같은 코드가 생겨야 한다.

```
[submodule "public"]
	path = public
	url = https://github.com/JeHa00/jeha0.github.io.git
	branch = main
```

- 만약 `public` folder는 생겼지만, `.gitmodules`와 위 코드가 없다면 다음 조치를 취한다.
- 첫 번째, `public` folder를 삭제한다.
- 두 번째, local 문서에서 `C:\Hugo\<new project 명칭>` folder로 들어가 `숨긴 파일` 보이기를 하여, `.git` 폴더의 `modules` 폴더를 삭제한다.
- 세 번째, terminal에 `C:\Hugo\<new project 명칭>` 경로에서 `git rm --cached public` 을 입력한다.
- 첫 번째, 두 번째만 실행한다면 다음과 같은 error 종류들이 뜰 수 있다.  
  `error: 'public' does not have a commit checked out`  
  `error: 'public' already exists in the index`  
  `error: a git directory for 'public' is found locally with remote(s)`
- 이렇게 뜨는 이유는 cach에 `public`이 아직 남아있기 때문이다. 그래서 이를 제거하고자 `git rm --cached public` 을 입력한다.
- 그리고, 다시 submodule 명령어를 실행하여, `.gitmodules` 에 위 코드가 생기는지 확인한다.
- 다시 `https://github.com/<user-name>/blog.git` 에 `public @` 폴더가 생겼는지 확인한다. 이 폴더가 생겼다면 submodule 등록이 확실하게 완료되었다.

- 만약, `Permission denied (publickey)` 오류가 뜬다면 SSH 보안키를 등록해야한다.

- 위 방법대로 했지만 `public` 폴더와 B repo가 submodule 연결이 되지 않는다면, B repo에 아무런 file이 존재하지 않아서다.
- [3.2 New repository 2개 만들기](https://jeha00.github.io/post/hugo%EB%A1%9C-github-page-%EB%A7%8C%EB%93%A4%EA%B3%A0-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0/#32-new-repository-2%EA%B0%9C-%EB%A7%8C%EB%93%A4%EA%B8%B0) 내용처럼 B repo에 README.md를 추가하여 empty repo로 만들지 말자.

<br>

## 3.5 public directory와 project root directoy git push 단계

```yml
# public directory에 site build 수행하기
> hugo

# public dicrectory로 이동
> cd public

> git add .
> git commit -m 'first build'
> git push origin main

# the project root 로 되돌아가기
> cd ..

> git add .
> git commit -m 'first build - update submodule reference'
> git push origin master
```

- 위 명령어로 생긴 tree는 다음과 같다.

```yml
Hugo/<project 명칭>
├─archetypes
├─content
├─data
├─layouts
├─public
│ ├─categories
│ └─tags
├─resources
│ │─_gen
│ ├─assets
│ └─images
├─static
│─themes
├─.gitmodules
└─config.toml
```

- A repo의 directory는 아래와 같다.
- 아래 image처럼 `public@`가 떠야 한다. `submodule`이 잘 연결되었다는 의미다.

![image](https://user-images.githubusercontent.com/78094972/154519110-95573fbf-29eb-4c8c-aff6-2d3f5a3c7c06.PNG)

<br>

---


# 4. Github page에 theme 적용

<br>

## 4.1 원하는 테마 찾기

- [Hugo Theme Star Ranking](https://hugo-theme-ranking.oika.me/), [jamstackthemes](https://jamstackthemes.dev/ssg/hugo/) 그리고, [Hugo Themes: Complete List](https://master--hugothemes.netlify.app/) 이 3가지 사이트 정도라면 충분히 찾을 수 있다.

- 처음에 테마 찾는 데 많은 시간을 썼는데, 지금 생각해보면 기본 테마를 찾은 다음에 customizing 하는 방법이 더 빠른 방향이었다.

<br>

## 4.2 submodule을 사용하여 테마 적용하기

- `submodule`로 테마를 적용한 이유는 업데이트된 테마를 쉽게 가져올 수 있기 때문에, clone보다 submodule로 만드는 게 더 낫다고 한다. (by [submodule이 나은 이유](https://discourse.gohugo.io/t/adding-a-theme-as-a-submodule-or-clone/8789))

- 테마 또한 `submodule`로 적용한다. 구조는 아래와 같다.
  ![image](https://www.adamormsby.com/posts/000/how-to-set-up-a-hugo-site-on-github-pages-with-submodules/img/submodules-diagram-with-theme.jpg)

출처: [How to Set Up a Hugo Site on Github Pages - with Git Submodules!](https://bit.ly/3sM7daw)

```yml
경로 C:\Hugo\<New Project 명칭>
# root project folder에 submodule로 테마를 적용한다
# git submodule add .git themes/Paper-Mod
> git submodule add <theme 경로>.git themes/<테마명>
```

- 원하는 theme을 `fork` 한다.
- `fork`를 하는 이유는 테마를 직접 수정할 수 없고, 수정한 버전을 유지하기 어렵기 때문에, fork하여 자신의 github으로 가져온다.

- fork한 테마의 경로를 복사하는 방법은 다음과 같다.


![image](https://user-images.githubusercontent.com/78094972/154526225-b52a80b0-cdfb-462b-8a74-b37d8b253525.PNG)

- 위 이미지에서 url 옆에 있는 버튼을 클릭하면 복사된다.
- `git submodule add <theme 경로>.git themes/<테마명>` 에서 themes란 밑에 이미지의 themes folder를 말한다. 이 folder 밑에 `<테마명>` folder를 만들어, 테마 자료들을 다운받는다는 의미다.

```yml
Hugo/<project 명칭>
├─archetypes
├─content
├─data
├─layouts
├─public
│ ├─categories
│ └─tags
├─resources
│ │─_gen
│ ├─assets
│ └─images
├─static
│─themes
│   └─<테마명>
├─.gitmodules
└─config.toml
```

- 다음으로 `config.toml` 에 `theme =` 이 없으면 추가하여, `theme = <테마명>` 을 적는다.
- `BaseURL`을 B repo의 끝 부분인 `https://<user-name>.github.io/` 로 수정한다. 이 경로가 앞으로 github page의 URL이 된다.
- theme 적용 또한 `submodule`이므로 `.gitmodules` 파일에 다음과 같이 2개가 등록되었다.

```yml
#example
[submodule "public"]
path = public
url = https://github.com/<user-name>/<B repo 명칭>.git
branch = main

[submodule "<테마명>"]
path = themes/<테마명>
url = 테마 url
```

- theme 적용 후, theme 폴더 안에 `examplesite` 란 의미의 폴더가 있다면, 그 폴더 안에 있는 `config.toml`을 복사하여 `C:\Hugo\<New Project 명칭>` 경로에 있는 `config.toml`에 복사 붙여 넣는다. `BaseURL`만 다시 수정한다.

<br>

## 4.3 테마 적용되었는지 테스트하기

```yml
경로 C:\Hugo\<New Project 명칭>
# D 란 draft 문서까지 포함해서 보겠다는 의미다.
> hugo server -D
```

- 그러면 `https://localhost:1313/` 이 뜬다. 이를 클릭하면 테마가 적용되었는지 알 수 있다.
- terminal 작업을 다시 할려면 `Ctrl + C`를 눌러 중단한다.
- `hugo server -D` 가 돌아가는 동안에는 글의 수정을 바로 확인할 수 있다.

<br>

---


# 5. Contents 생성과 업로드

<br>

## 5.1 Contents 생성과 `public` folder에 반영하기

```yml
경로 C:\Hugo\<New Project 명칭>
# contents 생성
> hugo new post/test1.md

# 생성된 글 public 폴더에 반영하기
> hugo -t <테마이름>
```

- `hugo new post/test1.md`는 `\content\post\test1.md` 경로로 생성된다.

- contents 생성 후, `hugo server -D`로 `localhost`에는 생성한 contents가 보이는데, github page에는 안보인다면 `hugo -t <테마이름>`명령을 하지 않았기 때문이다.

- 여기서 `<테마이름>`은 `git submodule add <theme 경로>.git themes/<테마명>` 에서 테마명과 동일해야 한다.

<br>

## 5.2 컨텐츠 업로드

```yml
# public dicrectory로 이동
> cd public

> git add .
> git commit -m 'commit message'
> git push origin main

# the project root 로 되돌아가기
> cd ..

> git add .
> git commit -m 'commit message'
> git push origin master
```

<br>

---

# 6. Utterances로 댓글 기능 추가, deploy.sh로 자동화

## 6.1 Utterances로 댓글 기능 추가

- 사용 방법
1. Github에 `<user-name>/blog-comments` 같이 private이 아닌 public 저장소를 만든다.
2. [Utterance](https://utteranc.es/)에서 1번에서 만든 repository를 입력한다.
3. [Utterance](https://utteranc.es/)에서 Mapping 방식 6가지 중 한 가지를 선택한다.
4. 2번 3번에 따라 [Utterance](https://utteranc.es/)에서 생성된 script를 복사하여 각자의 적절한 템플릿 위치가 추가한다. 추가 위치는 각 theme의 `README.md` 를 꼼꼼히 읽어본다.

[아웃사이더님의 블로그 글](https://blog.outsider.ne.kr/1356?category=1)에서 보고 가져온다.

<br>

## 6.2 deploy.sh로 자동화

- `deploy.sh` 파일명으로 아래 코드를 저장한 후, `C:\Hugo\<프로젝트 명칭>` 경로에 저장한다.   
- 이후 `Git Bash` 프로그램을 사용하여 `C:\Hugo\<프로젝트 명칭>` 경로로 이동 후, `bash deploy.sh`를 입력하면 the project root와 submodule 모두 순차적으로 push가 실행된다.  
- 나는 submodule은 `main` default branch에, the project root는 `master` default branch로 설정했다.  

```yml
#!/bin/bash

echo -e "\033[0;32mDeploying updates to GitHub...\033[0m"

# Build the project.
hugo -t <테마명>

# Go To Public folder
cd public
# Add changes to git.
git add .

# Commit changes.
msg="rebuilding site `date`"
if [ $# -eq 1 ]
  then msg="$1"
fi
git commit -m "$msg"

# Push source and build repos.
## master 대신 각자 연결한 branch 명으로 수정하면 된다.
git push origin main

# Come Back up to the Project Root
cd ..


# blog 저장소 Commit & Push
git add .

msg="rebuilding site `date`"
if [ $# -eq 1 ]
  then msg="$1"
fi
git commit -m "$msg"

## master 대신 각자 연결한 branch 명으로 수정하면 된다.
git push origin master
```

---

<br>

# 7. To customize blog theme & 후기

## To customize blog theme

- 커스텀마이징을 위한 제일 좋은 방법은 각자 선택한 테마 템플릿의 `README.md`를 꼼꼼히 읽어보는 게 제일 빠른 지름길이라 생각한다.


- 사용자가 원하는 기능들을 어떻게 추가하면 되는지 설명해논 템플릿이 많기 때문에, 반드시 `README.md` 를 `꼼꼼히` 읽기 바란다.

<br>

## 후기

- git에 대해 더 숙지를 하고 나서 git page를 만들기 시작했다면 시간 소모를 줄일 수 있었을 것이다. 급한 마음에 github page를 시작하여 생각보다 많은 시행착오와 error들을 격었다. 이 error들은 대체로 git을 정확히 이해하지 못해서 발생하는 문제점들이다.


- 이번 일을 통해서 git이 개발자에게 사람의 숨쉬기와 같다는 걸 느껴서 [Pro git](https://book.naver.com/bookdb/book_detail.nhn?bid=7187291) 이란 책을 사서 꾸준히 공부하기로 결정했다.


- gith page를 만들었으니, TIL부터 시작하여 꾸준히 공부하자. Hugo를 선택한 것이 처음 시도할 때는 매우 힘들었지만, 지금 와서는 잘한 선택임을 느낀다.


- 진행하다가 부딪힌 error들에 대해서는 바로 바로 기록을 하자. 이는 반복적인 똑같은 삽질을 예방할 수 있다.

<br>

---

# Reference

- [정적 웹은 뭐고 동적 웹은 뭔가요?](https://www.youtube.com/watch?v=C06xRvXIAUk)

- [Jekyll, Hexo, Hugo 차이점 설명](http://tadakichi.tistory.com/188)

- [Hugo로 Github.io 블로그 만들기](https://github.com/Integerous/Integerous.github.io)

- [How to Set Up a Hugo Site on Github Pages - with Git Submodules!](https://www.adamormsby.com/posts/000/how-to-set-up-a-hugo-site-on-github-pages-with-submodules/)

- [theme 적용에 submodule이 나은 이유](https://discourse.gohugo.io/t/adding-a-theme-as-a-submodule-or-clone/8789)

- [아웃사이더님의 블로그 글](https://blog.outsider.ne.kr/1356?category=1)
