+++
author = "IceBlueHalls"
title = "따라하기 제일 쉬운 Hugo로 블로그 만들기 #1"
date = "2023-05-18"
description = "깃 리포지토리 생성, Hugo 다운로드, 환경변수 설정까지"
tags = [
    "Hugo"
]
categories = [
    "Hugo"
]
series = ["Hugo"]
aliases = ["Hugo"]
image = "10.PNG"
slug = "etc/hugo/install1"
+++

## 1. GitHub에서 레포지토리 생성
레포지토리는 콘텐츠 저장용 하나, 보여지기 용 하나.  
총 2개의 레포짓토리가 생성됩니다.

* 콘텐츠 저장용 : myBlog
* 보여지기용 : mntchocopsycho.github.io

### 1. 콘텐츠 저장용 블로그 레포지토리 생성
Github에서 새로운 respository를 생성합니다.
![](1.PNG)

생성 시, 이름은 상관이 없으며 중요한 점은 Public으로 설정되어있어야합니다.
![](3.PNG)

![](4.PNG)

### 2. 보여지기용 블로그 레포지토리 생성

콘텐츠용 블로그 레포지토리가 생성되었으면 이번에는 보여지기용(실제 URL) 레포지토리를 생성합니다.  

여기 또한 public으로 설정해야 하며, 이름은 자신의 닉네임이 들어간 주소로 만들어야합니다.

```
{블로그 이름}.github.io
```

![](5.PNG)

![](6.PNG)

## 콘텐츠용 블로그 주소로 다운로드


myBlog 레포지토리에 들어가서 HTTPS로 된 git clone 주소를 복사합니다.

![](7.PNG)

그리고 컴퓨터에 다운받습니다.(여기서는 Fork 라는 Git UI 프로그램을 사용했습니다.)

주소는 아무곳이나 상관없습니다.

![](8.PNG)

![](9.PNG)

## 3. Hugo 다운로드

[Hugo 다운로드 주소](https://gohugo.io/installation/)에 접속합니다.

![](10.PNG)

컴퓨터가 Windows 이므로 [Windows](https://gohugo.io/installation/windows/)를 클릭합니다.

![](11.PNG)


Prebuilt Binaries에서 [latest release](https://github.com/gohugoio/hugo/releases/tag/v0.111.3)를 클릭하여 Hugo의 진짜 다운로드 페이지로 이동합니다.

![](12.PNG)

제일 밑으로 스크롤을 내려 **hugo_extended_0.xxx_windows-amd64.zip** 을 다운받습니다.

extended로 다운받는 이유는 일반 hugo로는 일부 테마에서는 scss(css 진화판)을 읽지 못해서 extended로 다운받았습니다.

![](13.PNG)

`C:\Program Files\Hugo`에 압축을 해제합니다.
Hugo 폴더 안에 hugo.exe, LICENSE, README.md가 존재하면 됩니다.

## Hugo 환경 변수 설정

![](14.PNG)
Hugo를 cmd에서 호출하기 위해서는 환경 변수를 설정해야합니다.

제어판의 시스템 환경 변수 편집을 들어갑니다.

![](15.PNG)
시스템 설정에서 **환경 변수**를 클릭합니다.

![](16.PNG)
시스템 변수의 Path를 클릭하여 편집을 클릭합니다.

![](17.PNG)
Hugo 폴더를 새로 추가합니다.(만약 다른 곳에 설치하였다면 해당 경로로 설장합니다.)

![](18.PNG)
설정이 완료되었다면 완료를 클릭하여 테스트를 실시해봅니다.

cmd를 열어서 hugo version을 입력해봅니다.  
Hugo의 버전을 알려주는 메세지가 나오면 성공한 것입니다.