+++
author = "Byungkyu-ju"
categories = ["Hugo"]
date = "2017-12-27"
description = "github-hugo blog 운영하기"
featured = ""
featuredalt = ""
featuredpath = "date"
linktitle = ""
title = "github-hugo blog 운영하기"
type = "post"

+++
정적블로그 프레임워크중 하나인 hugo를 활용하여 블로그를 만들어 보았다.  
github pages를 통해 블로그를 운영하는 프레임워크는 대표적으로 3개가 있는데
jekyll / hexo / hugo 가 있다.  
jekyll이 github pages에 내장되있기도 하고,테마도 많고 사용자도 많긴 한데, 
ruby에 대해서 아는것도 부족하고 크게 흥미가 와닿지 않아 일단 제쳐두었다.  
(카카오 기술블로그도 jekyll로 만들었더라..)  
hexo는 node.js 기반인데.. 사실 만들어서 1년정도 방치해두다가 지워버렸다.  
그리고 이번에 적용하는 hugo는 go언어 기반이다.  
1년전 https://www.staticgen.com/ 에서 star수가 hexo 다음으로 3등이었던거 같은데 지금보니 hexo보다 더 많은 2등으로 되어있다.  
사실 별 생각없이 그냥 hugo로 해보자 해서 한거라..굳이 각 프레임워크별 장단점을 여기서 적진 않았다.  

이번에 hexo를 github pages에 올리면서 느낀건,  
hexo를 로컬에서 올리는건 쉬웠는데 github pages에 deploy하는부분이 힘들었던것 같다. 몰라서 힘들었지 알고나면 쉬운것을 빙빙 돌아서 한 느낌?  
아무튼 hugo를 github에 deploy까지 하는 내용을 정리하는겸, 공유하는겸 포스팅주제로 정했다.  

hugo 설치는 OS별로 설치하는 방법이 다르니 공식사이트에서 확인하고 설치.  
https://gohugo.io/getting-started/installing/  

다음 블로그를 생성할 폴더를 만들고, 해당 위치에서 hugo blog 생성.
```
mkdir blog
cd blog
hugo new site . 
```
그러면 아래와 같이 hugo 디렉토리가 생성된다.
```
archetypes      config.toml     content         data            layouts         static          themes
```

그리고
```
git init
git remote add origin 'github clone url'
git pull origin master
```
여기까지 git 연동까지 했고  
이제는 gitignore에 폴더를 지정할 차례이다.  

그런데,지금 디렉토리 구조를 보면 index파일도 없고 왜 이게 블로그가 되냐..라는 생각이 들꺼지만  
```
hugo
```
라고 명령어를 치면
```
archetypes      config.toml     content         data            layouts         public          static          themes
```
'public' 폴더가 생겼을꺼다.  
public폴더를 보면 index파일등 다양한 파일들이 만들어졌을껀데,  
public안에 있는 파일들이 실제로 블로그에서 보일 내용들이다.  

hugo는 정적사이트 생성기.  
'생성기'와 '생성된 사이트'는 다르다.  
위 구조에서 'public'폴더를 제외한 파일/폴더들은 정적 사이트를 생성하기 위한 '도구'이고 도구를 통해서 만들어진 '사이트'는 public폴더안의 내용들이다.  

그렇다면 생성기인 'blog'에는 'public'이 존재할 필요가 없다.  
로컬이야 있어도 무방하고, git에 올리지 않기 위해서 gitignore에 'public'폴더를 추가.  
```
vi .gitignore
```

```
public/
```
vi나 vim를 못쓴다면 .gitignore을 추가하고, 맨위에 public/을 추가.
다음 깃에 현재내용들을 추가하고 commit  
```
git add .
git status
git commit -m 'init'
```

다음 마스터에 푸시  
```
git push -u origin master
```
그리고 github에서 프로젝트를 확인하면, public을 제외한 '블로그생성기'의 역할을 수행할 파일들이 올라갔다.  

이제 블로그생성기는 만들었으니, 블로그를 찍어낼 테마를 구하자.  
hugo 블로그 테마는 https://themes.gohugo.io/ 여기서 마음에 드는 테마를 고른다.  

테마를 골랐으면  
다시 blog로 와서  
```
cd themes/
git clone '복사할 테마의 clone url'
```
다운이 완료됬으면, 
blog폴더내 config.toml파일로 들어간다.  
config.toml은 hugo의 설정파일인데, 이제는 테마의 설정파일을 사용할 예정이기 때문에  
clone한 themes > 다운받은테마 > exampleSite > config.toml 파일안에 있는 내용을 복사해서  
내가 사용할 blog폴더안의 config.toml에 그대로 붙여넣는다.  

그리고
```
hugo server
```
를 입력하면
```
localhost:1313
```
에서 로컬에서 실행되는 '테마가 적용된 블로그'를 미리보기처럼 볼 수 있다.  
그리고 수정하고싶은 항목들 수정하고 커밋하고 마지막에 푸시.  
```
git commit -m 'init'
git push origin master
```
이제 생성기가 사이트를 만들 준비를 마쳤으니, 배포를 해보자.  
다음은 생성기가 만들어낸 사이트를 배포할 프로젝트를 만든다.  
github에서는 '.github.io' 라고 무료 호스팅을 제공해주고 있다.  
https://pages.github.com/
위 사이트처럼 본인 사용자 이름에 맞게 레파지토리를 하나 만든다. ex) byungkyu-ju.github.io  
레파지토리를 만들었으면, 로컬에 레파지토리와 연동할 폴더를 하나 더 만든다.  
앞서 만든 blog와 동일한 위치에  
```
mkdir username(본인이름).github.io //폴더이름을 호스팅url이름으로(취향에 따라 달리해도 무방)
```
그리고 생성한 사이트를 clone
```
git clone username(본인이름).github.io
git pull origin master
```
다음 blog경로로 가서 생성기가 디플로이를 수행할 경로를 지정하고 디플로이한다.
```
hugo -d ../username(본인이름).github.io
```
username(본인이름).github.io 폴더에 가면 'public' 폴더에 있을법한 파일들이 생성되있다.  
그리고 username(본인이름).github.io 폴더에서 커밋후 푸시.  
그러면 https://username(본인이름).github.io 에서 디플로이된 사이트를 확인할 수 있다.  
다음부터는 blog에서 작업한 이후 username(본인이름).github.io에 디플로이하고 커밋하고 푸시하고 웹에서 확인하면 되는데....  

이제 과정을 알았으니, 이 디플로이하는 작업이 귀찮을꺼다.  

디플로이 하는 작업을 간소화하기 위해 쉘스크립트로 짜는 분들도 있고, netlify로 디플로이하는 분들도 있다.  
그런데 나는 어짜피 다른 프로젝트들도 ci를 할때 travis를 사용할 예정이기 때문에 travis로 디플로이를 할 것이다.  
github-hugo blog배포에 이어서 travis 연동은 다음 포스팅에서 다루도록 한다.