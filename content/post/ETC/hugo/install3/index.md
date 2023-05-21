+++
author = "IceBlueHalls"
title = "따라하기 제일 쉬운 Hugo로 블로그 만들기 #3"
date = "2023-05-20"
description = "hugo로 블로그 만들기 3편"
tags = [
    "Hugo"
]
categories = [
    "Hugo"
]
series = ["Hugo"]
aliases = ["Hugo"]
image = "1.PNG"
slug = "etc/hugo/install3"
+++

## hugo 자동 배포 스크립트 추가

![1.PNG]
기존 hugo의 테마를 받은 깃 페이지에서 예시 파일로 제공되는 github > workflows에 있는 deploy.yml을 다운받습니다.

![2.PNG]
myBlog에도 똑같이 .github > workflows에 해당 파일을 붙여넣습니다.

![3.PNG]
해당 파일을 열고 아래 하단으로 내려갑니다.

![4.PNG]

하단에 Deploy 하는 방법을 설정할 수 있는데, 다음과 같이 설정합니다.

external_repository: {git 닉네임}/{git 블로그 주소}
user_name: {git 닉네임}
user_email: {git 이메일 아이디}
public_branch: {깃 블로그 주소의 메인 브런치}

![5.PNG]
블로그 내용을 자동 배포하려면 이 배포가 정상적인지 확인을 하는 깃허브 토큰이 필요합니다.

깃허브로 가서 우측 상단의 프로필을 클릭하여 Settings를 클릭합니다.

![6.PNG]
스크롤을 제일 밑으로 내리면 Developer Settings를 클릭합니다.

![7.PNG]
Personal access tokens를 클릭하고 우측 상단에 Generate new tokens를 클릭합니다.

![8.PNG]
토큰의 이름은 HUGO로 설정합니다. 위 deploy.yml에서 토큰 이름이 secrets.HUGO로 되어있기 때문에 맞춰주어야합니다.

만료 날짜는 최대한 오래합니다. 토큰이 만료되면 재등록을 해야하는 귀차늠이 있습니다,

권한은 repo 하나만 추가해주어도 됩니다.

![9.PNG]
토큰 생성이 완료되면 토큰 문자가 노출되는데 이것을 복사하여 다른 곳에 저장합니다. 이 페이지를 나오면 더는 볼 수 없기 때문에 다른 곳에 저장하는 것을 추천드립니다.

잊어버리면 재발급해야합니다.

![10.PNG]
깃 블로그의 내용은 myblog에서 이루어지며, 해당 깃에 변화된 내용을 블로그 깃에 재배포하는 것이기 때문에 myblog에 토큰을 입력해야합니다.

myblog의 메뉴 중 Secrets and variables를 클릭합니다.

![11.PNG]
초록 버튼 New repository secret을 클릭합니다.

![12.PNG]
이름을 HUGO라고 지정합니다. 위 deploy.yml에서 토큰 이름이 secrets.HUGO로 되어있기 때문에 맞춰주어야합니다.

Secret은 복사했던 토큰 문자를 붙여넣습니다.

![13.PNG]
토큰까지 깃허브 웹에서 설정이 완료되었다면 deploy.yml을 서버에 업로드해줍니다.(푸시해줍니다.)

![14.PNG]
그리고 cotent/post/에 아무 md 파일이나 생성해보고 테스트로 업로드해봅니다.

![15.PNG]
myBlog의 깃허브 웹에서 Actions을 보면 deploy.yml의 실행 결과를 확인할 수 있습니다.

![16.PNG]
위에서 성공 표시가 나온다면 블로그 깃에서는 자동으로 커밋 및 푸시가 발생하는 것을 확인할 수 있습니다.

![17.PNG]
다시 자신의 블로그로 가서 확인해보면 깃 블로그에서 자동으로 글이 업로드 되는 것을 볼 수 있습니다.