# Hugo를 사용한 정적인 블로그 만들기

>주소가 github.io인 개발 블로그들이 눈에 많이 띄었다.  
>찾아보니 Jekyll, Hexo, Hugo 등 Static Site Generator의 존재를 알게 되었다.  
>Hugo와 Github Page의 조합으로 Devlog로 사용할 [개인 블로그](https://ryan-han.com)를 만들기로 했다.  
>놀다 지친 2018년 여름휴가 막바지에 집중공략을 시작했다.

## 1. Static Site Generator 란?
[이 글](https://blog.nacyot.com/articles/2014-01-15-static-site-generator/)이 정적 웹사이트 생성기와 동적 웹사이트 생성기의 차이를 잘 설명해주고 있다.

## 2. Static Site Generator 선택 과정
[이 곳](https://www.staticgen.com/)에서 모든(?) Static Site Generator들을 한눈에 볼 수 있었다.  
[이 글](http://tadakichi.tistory.com/188)에서 가장 많이 사용하는 Jekyll , Hexo, Hugo를 비교하여 아래 내용을 참고하였다.
~~~
Jekyll
  -루비 기반
  -현재 가장 인기 있음(깃헙 별 수 제일 많음)
  -한글 레퍼런스도 제일 많음
  -느리다는 제보가 많음 (빌드 한번 다시 하는데 5분씩 걸린다고)
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
~~~



## 3. Hugo, 너로 정했다!
### 3.1. 내가 Hugo를 선택한 이유
  - Go로 제작되었다. (Go를 공부중이다.)
  - Hugo는 런타임에 다른 의존성이 필요하지 않아 빌드시간이 세계에서 제일 빠르다. ("Hugo is the world’s fastest static website engine.")
  - Hugo는 한글 Reference가 거의 없는 오픈소스이며 Jekyll에 비해 기여할 수 있는 여지가 남아있다. 즉, ***오픈소스에 기여할 기회 !!***

### 3.2. [CloudZ Labs](http://tech.cloudz-labs.io/posts/hugo/hugo/)에서 Hugo를 선택한 이유
  - Jekyll을 사용할 경우, 별도의 Build 과정 없이 Repository에 Push만으로 작성한 글들이 알아서 Publishing됩니다. 하지만, 글이 많아질 수록 Jekyll의 빌드 성능은 현저하게 저하됩니다. 하지만, Hugo는 Build 과정이 있어도 성능저하 없이, 빠르게 글을 Publishing할 수 있습니다. Go나 기타 종속성 없이, Hugo CLI를 통해서 쉽게 블로그 및 글을 생성할 수 있습니다. 그래서, Hugo로 블로그를 만들게 되었습니다.

## 4. Hugo + Github Page 만드는 과정

### 4.1. Hugo 설치
>나도 멋깔나게 `$ brew install hugo`를 mac 터미널에 입력해서 설치하고 싶었다.  
>하지만 현실은 WINDOWS...  
>Giraffe Academy의 [Windows에서 Hugo설치하기](https://gohugo.io/getting-started/installing#windows) 이 영상 하나면 설치는 쉽다. (젊은 형아가 영어로 설명해줌)  

- [Hugo 공식 깃헙](https://github.com/gohugoio/hugo/releases)에서 운영체제에 맞는 최신버전 다운로드
- `C:\Hugo\bin` 디렉토리 생성해서 다운받은 압축파일 해제
- 어느 위치에서나 Hugo가 실행될 수 있도록`$ set PATH=%PATH%;C:\Hugo\bin` 명령으로 환경변수에 `C:\Hugo\bin`추가
- 명령 프롬프트에 `$ hugo version` 혹은 `$ hugo help`로 동작 확인

### 4.2. Github 저장소 2개 만들기
- 하나는 Hugo의 컨텐츠와 소스파일들을 포함할 `<YOUR-PROJECT>` 저장소 생성 (나의 경우 `blog`라는 이름으로 생성)
- 다른 하나는 렌더링된 버전의 Hugo 웹사이트를 포함할 `<USERNAME>.github.io` 저장소 생성 (`integerous.github.io`)

### 4.3. Directory Structure 구성
- `$ hugo new site blog` 명령으로 로컬에서 컨텐츠를 관리하기 위한 장소(Hugo/blog) 생성
- `C:\Hugo\blog`에서 `$ dir`로 directory structure를 확인할 수 있다.

### 4.4. 테마 다운로드 및 설정
- https://themes.gohugo.io/ 에서 원하는 테마를 선택한다.
- 선택한 테마의 github에서 저장소 주소를 복사한다.
- `C:\Hugo\blog\themes`에 선택한 테마를 clone한다. `$ git clone https://github.com/선택한/테마`
- `config.toml` 파일을 선택한 테마의 설명서에 따라 수정한다.

### 4.5. Remote와 Submodule 설정
- 깃헙에 만든 `blog 저장소`를 local의 blog 디렉토리의 remote로 등록한다.
  - `C:\Hugo\blog` 로 이동
  - `$ git init`
  - `$ git remote add origin git@github.com:integerous/blog.git`
- `integerous.github.io 저장소`를 blog의 submodule로 등록한다.
  - `$ git submodule add -b masater git@github.com:integerous/integerous.github.io.git public`
  - 이렇게 함으로써 `hugo` 명령으로 `public`에 웹사이트를 만들 때, 만들어진 `public` 디렉토리는 다른 remote origin을 가질 것이다.

### 4.6. 컨텐츠 생성
- `C:\Hugo\blog` 디렉토리에서 `$ hugo new post/test1.md` 명령으로 파일을 생성하면 `\content\post\` 디렉토리에 `test1.md` 파일이 생성된다.
- 파일 생성 명령에 자동으로 편집기(나의 경우 VSCode)가 실행되도록 하려면
    1. `config.toml` 파일에 `newContentEditor = "실행파일 파일명"` 를 설정해준다.
    2. 실행파일 파일명이 Code.exe 면 `newContentEditor = "Code"` 라고 작성.
    3. 시스템 환경변수에 VS Code 실행파일이 있는 경로를 추가해준다. (필수!)

### 4.7. 컨텐츠 업로드 (블로그에)
- `C:\Hugo\blog`로 이동
- `$ hugo -t 테마이름` 명령을 통해 테마가 적용된 블로그 내용을 public에 생성한다.
- `$ cd public` public 디렉토리로 이동하여
- `$ git add .` 수정된 파일들을 index에 올린다.
- `$ git commit -m "커밋메세지"` 변경 내용을 commit하고
- `$ git push origin master` commit을 Integerous.github.io에 푸시
- `blog 저장소`에도 변경내용 푸시
  - `$ git add .`
  - `$ git commit -m "커밋메세지"`
  - `$ git push origin master`

### 4.8. 쉘 스크립트로 업로드 자동화하기
[Hugo Docs](http://gohugo.io/tutorials/github-pages-blog/)의 deploy.sh 파일을 활용하여 쉘스크립트 작성
  ~~~sh
  #!/bin/bash

  echo -e "\033[0;32mDeploying updates to GitHub...\033[0m"

  # Build the project.
  hugo -t hugo-theme-geppaku

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
  git push origin master

  # Come Back up to the Project Root
  cd ..


  # blog 저장소 Commit & Push
  git add .

  msg="rebuilding site `date`"
  if [ $# -eq 1 ]
    then msg="$1"
  fi
  git commit -m "$msg"

  git push origin master
  ~~~

# 5. Utterences (Github 댓글 위젯) 추가하기
>[아웃사이더님의 블로그 글](https://blog.outsider.ne.kr/1356?category=1)에서 Utterences의 존재를 알게 되었다.  
>Hugo 공식 문서에 소개된 댓글 위젯 중 Utterences는 없길래 생애 처음으로 오픈소스에 PR을 날려봄!!

### 5.1. Hugo Docs에서 내 Pull Request 받아줌!!!
>생애 첫 오픈소스 기여! (매우 소소한 기여지만.. 오픈소스에 기여하기 시작했다는 사실만으로 햄볶!!)  

  ![](https://github.com/Integerous/TIL/blob/master/ETC/images/myfirstPR.png?raw=true)
### 5.2. 작동 원리
[Utterance 프로젝트](https://utteranc.es/)의 작동 방식을 소개하자면,  
[Github의 이슈 검색 API](https://developer.github.com/v3/search/#search-issues)를 사용해서 각 글에 해당하는 이슈가 생성되고(최초 댓글 작성 시),  
댓글들은 해당 글로 생성된 이슈에 대한 댓글로 추가되는 방식이다. 댓글은 [Primer](https://primer.github.io/)를 이용해서 Github 스타일로 보여진다.

### 5.3. 사용 방법
1. Github에 public 저장소를 만들고(blog-comments 등으로)
2. [Utterance document](https://utteranc.es/)에서 방금 만든 저장소를 입력하고(나의 경우 Integerous/blog-comments)
3. 블로그 글과 Github 이슈를 매핑할 방법 6가지 중 한 가지를 선택하면
4. 밑에 아래와 같은 script를 자동으로 생성해준다.
    {{< highlight html >}}
    <script src="https://utteranc.es/client.js"
            repo="integerous/blog-comments"
            issue-term="pathname"
            crossorigin="anonymous"
            async>
    </script>{{< /highlight >}}

5. 위의 script를 본인의 블로그 템플릿중 원하는 위치에 넣으면
6. 끝!  

## *블로그 주소
https://ryan-han.com 

## *Reference
- [Hosting on Github](https://gohugo.io/hosting-and-deployment/hosting-on-github/)
- [Jekyll, Hexo, Hugo 간단 비교 글](http://tadakichi.tistory.com/188)
- [Hugo를 활용한 기술 블로그 구축기](http://tech.cloudz-labs.io/posts/hugo/hugo/)
- [Github Page에 Hugo 올리기](https://github.com/sabzil/blog/blob/master/content/post/tips/hugo.md)
- [페이스북 댓글을 Utterances로 교체하기](https://blog.outsider.ne.kr/1356?category=1)
- [Utterances 프로젝트](https://utteranc.es/)