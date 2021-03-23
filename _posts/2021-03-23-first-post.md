---
title: "[Github 블로그] Windows 환경에서 GitHub Pages와 Jekyll을 이용해 블로그 만들기"
excerpt: "앞으로 프로그래밍 공부를 기록하기 위해 GitHub Blog를 시작했다."

categories:
  - Blog
    tags:
      - [,Git, GitHub, GitHub Pages, Jekyll, Ruby, minimal-mistakes]
    
    toc: true
    toc_sticky: false

    date: 2021-03-23
    last_modified_at: 2021-03-24
---

 GitHub Pages는 GitHub에서 제공하는 정적 웹 사이트로, GitHub repository에 웹 리소스를 push하는 것만으로 간단히 웹사이트를 만들 수 있다. GitHub 저장소에 저장된 html 파일 등의 정적 웹 문서들을 GitHub에서 무료로 웹에서 볼 수 있도록 호스팅 서비스를 제공해주는 것이다.  

 Jekyll(지킬)은 Markdown(마크다운)으로 작성된 문서를 HTML로 변환하여 웹사이트를 구축할 수 있도록 돕는 정적 웹사이트 생성기(static website generator)로, Ruby로 작성되어 있다. GitHub Pages에 Jekyll을 결합하면 보다 더 생산적으로 블로그를 만들 수 있도록 해준다. 로컬 환경에서 Jekyll을 사용해 웹 사이트를 작성 및 테스트하고, GitHub repository에 웹 리소스를 push하면 매우 간단하게 웹 사이트를 호스팅할 수 있는 구조다.  

 정적 웹사이트(static website)는 Database 등은 사용할 수 없으나, 무료로 유지보수가 간편한 website hosting을 할 수 있다는 장점이 있다.  

이러한 GitHub Pages와 Jekyll을 이용해 블로그를 만들어보았다.  

- - -
# Github에서 repository 생성
Github에서 로그인 후, 블로그 용으로 사용할 새로운 레포지토리(repository, 저장소)를 생성한다.  
 이때 레포지토리의 이름을 `사용자이름.github.io`로 한다. ex) jxxxxharu.github.io  

```ruby
git config --global user.email "사용자이메일"
git config --global user.name "사용자이름"
```

그렇게 생성한 레포지토리는 일단 빈 공간으로 둔 채, 내 컴퓨터로 `clone`한다.  
```ruby
git init  
git clone "https://github.com/사용자이름/사용자이름.github.io"  
```

- - -
# Ruby 설치
Jekyll은 Ruby 기반이므로 로컬 개발 환경 세팅을 위해 Ruby를 설치한다.  
다음 링크를 이용한다. https://www.ruby-lang.org/ko/downloads/

- - -
# Jekyll 설치
```ruby
gem install jekyll
gem install minima
gem install bundler
gem install jekyll-feed
gem install tzinfo-data
```

- - -
# Jekyll 테마 다운로드 및 적용
## 테마 다운로드
원하는 Jekyll 테마를 다운받아 적용해보자. 필자는 'minimal-mistakes'를 사용하였다.  
[minimal-mistakes](https://github.com/mmistakes/minimal-mistakes)에서 소스를 다운로드 할 수 있고, GitHub 레포지토리를 zip 파일로 다운받는다.  
다운받은 zip 파일 안의 내용을 프로젝트 폴더(사용자이름.github.io)에 압축을 풀어 그대로 옮긴다.  

## 불필요한 파일 삭제
이 과정에서 불필요한 파일을 삭제하는데, 그 목록은 다음과 같다.  
- .editorconfig  
- .gitattributes  
- .github
- /docs
- /test
- CHANGELOG.md
- ~~minimal-mistakes-jekyll.gemspec~~
- README.md
- screenshot-layouts.png
- screenshot.png
위 목록에서 minimal-mistkaes-jekyll.gemspec을 지우면 server가 안 된다는 글을 봤기 때문에 그냥 냅두었다.

## _posts, _drafts 폴더 생성
그리고 최상위 경로에 _posts 폴더와 _drafts 폴더가 없다면 생성한다.
- _posts: 배포될 포스트들이 담기는 곳
- _drafts: 포스트 초안이 담기는 곳으로, 배포되지 않고 테스트 환경에서 보기가 가능하다.  

## .gitignore 생성
다음으로 최상위 경로에 .gitignore 파일이 없다면 생성하고, 있다면 아래 내용을 추가해준다.  
[Jekyll_gitignore_list](https://gist.github.com/bradonomics/cf5984b6799da7fdfafd)

## Gemfile 수정
최상위 경로의 Gemfile을 아래 내용으로 수정한다. 메모장에서 수정이 가능하다.
```
source "https://rubygems.org"

gem "jekyll", "~> 3.5"
gem "minimal-mistakes-jekyll"
gem "kramdown-parser-gfm"  // (Windows) Dependency Error 방지
```
그 다음, 아래 명령어를 수행한다.
```ruby
cd 사용자이름.github.io
bundle
```
`bundle`: Gemfile을 검사해서 필요한 목록을 설치  
설치가 완료되면 웹 호스팅이 가능하고, 이렇게 해서 기본적인 테마 적용이 끝난다.

- - -
# 테스트 및 배포
## 로컬에서 Jekyll 블로그 테스트
로컬에서 Jekyll 블로그를 테스트하기 위해 다음 명령어를 수행하고, 테마가 잘 적용되었는지 확인한다.  
```ruby
bundle exec jekyll serve
```
`bundle exec jekyll serve`: Jekyll 로컬 서버를 실행시킨다. 종료하려면 Ctrl + C  
`Server running...`이라는 문구가 나타나면 성공이다.  
나중에 `_config.yml` 파일을 수정하는 것은 반영되지 않아서 다시 명령어를 수행해야 반영된다.  

## GitHub Pages에 블로그 배포
완성된 블로그를 commit-push를 사용해 자신의 GitHub Page에 올린다.  
```ruby
git add .
git commit -m "커밋 메세지"
git push
```
`git add <파일>`: 새로운 파일을 추가하거나 이미 존재하는 파일을 스테이징  
`git commit -m "<메세지>"`: 커밋  
`git push`:  
`사용자이름.github.io`에 접속해 제대로 반영되었는지 확인한다.  

- - -
참고)    정말 감사합니다! 
https://junhobaik.github.io/jekyll-apply-theme/  
https://devinlife.com/howto/  
https://2ssue.github.io/blog/make_jekyll_blog/  
https://ansohxxn.github.io/blog/posting/  
