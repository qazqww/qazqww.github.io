---
layout: post
title:  "블로그 오류 일지"
categories: blog
---

## 1. jekyll 프로젝트 생성 시 Confilct
<br>
### 발생 상황

```bash
> jekyll new ./
```

새로운 Jekyll 프로젝트 생성 시 발생

### 오류 문구

```bash
Conflict: C:/Users/qazqw/Documents/sources/qazqww.github.io exists and is not empty.
          Ensure C:/Users/qazqw/Documents/sources/qazqww.github.io is empty or else try again with `--force` to proceed and overwrite any files.
```

Confilct 발생

### 원인

프로젝트를 만들고자 하는 디렉토리가 비어있지 않기 때문에 발생

### 해결 방법

```bash
> jekyll new ./ --force
```

--force나 -f 옵션을 사용해 강제로 프로젝트를 생성하도록 하였다.

### 참고 사이트

[개인 Blog 만드는 절차 with Jekyll & GitHub Pages](https://cjy-tech.github.io/make-blog-with-jekyll-and-github_pages/)
<br><br>

## 2. tzinfo 오류
<br>
### 발생 상황

```bash
> bundle exec jekyll serve
```

테마 적용 후 프로젝트 실행 시 발생

### 오류 문구

```bash
Dependency Error: Yikes! It looks like you don't have tzinfo or one of its dependencies
installed. In order to use Jekyll as currently configured, you'll need to install this
gem. The full error message from Ruby is: 'cannot load such file -- tzinfo'
If you run into trouble, you can find helpful resources at https://jekyllrb.com/help/!
```

Dependency Error로 tzinfo 문제를 알려준다.

### 원인

Windows 환경에서 발생하는 오류.

윈도우에는 루비 인터프리터가 타임존 정보를 처리할 시 필요한 정보 원천이 없기 때문에, 환경변수 TZ로 대신한다. 이를 설정해줄 gem이 필요한 것 같다.

### 해결 방법

Gemfile 에 다음 내용을 추가

```ruby
# Windows does not include zoneinfo files, so bundle the tzinfo-data gem
gem 'tzinfo'
gem 'tzinfo-data', platforms: [:mingw, :mswin, :x64_mingw]
```

### 참고 사이트

[Github Pages 04. 타임존 관리](https://jennysgap.tistory.com/entry/Github-Pages-04-%ED%83%80%EC%9E%84%EC%A1%B4-%EA%B4%80%EB%A6%AC)
<br><br>

## 3. webrick 관련 오류
<br>
### 발생 상황

위 오류를 해결한 뒤

```bash
> bundle exec jekyll serve
```

다시 프로젝트 실행 시도 시 발생

### 오류 문구

```bash
C:/Ruby31-x64/lib/ruby/gems/3.1.0/gems/jekyll-3.9.2/lib/jekyll/commands/serve/servlet.rb:3:in
`require': cannot load such file -- webrick (LoadError)
```

webrick 파일을 찾을 수 없다고 뜬다.

### 원인

ruby 3.0.0부터 webrick이라는 gem이 기본적으로 포함되지 않기 때문에 따로 설치해주어야 한다.

### 해결 방법

```bash
> bundle add webrick
```

webrick을 설치해주자

### 참고 사이트

[jekyll 실행 시킬 때 `require': cannot load such file -- webrick (LoadError) 오류가 난다면 bundle add webrick](https://junho85.pe.kr/1850)