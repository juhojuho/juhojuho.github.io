---
title: "맥OS에서 윈도우10 설치 USB 만들기"
description: 
date: 2020-09-01
tags:
  - macos
layout: layouts/post.njk
---

우리나라에서 애플 제품으로만 사는 것은 다소 힘들다. 악명 높은 공인인증서의 존재와 공공 사이트에서도 윈도우 위주로 지원을 해주기 때문이다. 나는 맥북을 쓰고 있고 연구실 데스크탑도 iMac을 쓰고 있다. 마침 컴퓨존에서 특가로 나온 제품이 있길래 저렴한 가격에 윈도우용 데스크탑을 구매했다. 이제 윈도우10만 깔면 되는데 마침 우리학교에 각종 S/W를 제공하는 서비스가 있었다. 오호라 그럼 윈도우 설치용 usb만 만들면 금방 끝나겠구만! 했는데.. macOS에서 윈도우 설치 usb를 만드는 게 생각보다 까다로웠다..

## 1단계: Window10 IOS 다운로드
우선 설치하고 싶은 윈도우10 버전의 ISO 파일을 구하자. 또한 8GB 이상의 USB를 준비하자. 

## 2단계: UDB 포맷 방식 변경
USB를 맥에 연결하고 "Disk Utility" 프로그램을 연다. 상단의 "Erase" 버튼을 누루고 'Fromat' 칸에 "MS-ODS (FAT)"를 선택한다. 'Name'은 아무거나 입력해도 된다.

## 3단계: 복사 붙여넣기
다운로드받은 ISO 파일을 더블 클릭하여 마운트하자. ISO 파일 안의 세부 파일을 모두 복사하여 USB에 붙여넣자. 아마 드래그 해서 붙여넣는 방법으로 하면 용량이 너무 커서 오류가 뜰 것이자. 커맨드를 사용하면 해결된다. 터미널을 열고 아래와 같이 입력하자.
```bash
cd /Volumes/{PATHtoISOFiles}/ # ISO 파일이 마운트된 폴더로 이동
cp -R ./* /Volumes/{USBNames}/ # ISO 안의 모든 파일을 USB로 복사
```

## 4단계: 용량 쪼개기
끝~ 인줄 알았으나 아마 복사하는 도중 또다시 문제가 생겼을 것이다. 

> “install.wim: File too large”

우리가 2단계에서 선택한 `FAT(FAT32)` 포맷 지원하는 한 파일의 용량은 최대 4GB이다. 그런데 우리가 복사하는 파일 중 `ìnstall.wim`의 용량은 4GB가 넘는다. 여기서 문제가 발생한다. (설치하고자 하는 윈도우 버전에 따라 위 오류가 발생하지 않을 수도 있다. 예전 버전에는 해당 파일의 용량이 4GB 이하였다.)
해결책은 `install.wim` 파일을 4GB 이하의 파일 2개로 쪼개는 것이다. 먼저 `wimlib`이라는 library를 설치하자.
```bash
# Homewbrew가 설치되지 않았을 시
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
# wimlib을 Homebrew를 통해 설치
brew install wimlib
```
그 후, 아래의 커맨드로 파일을 분할하자.
```bash
# install.wim 파일을 3800MB(< 4GB)의 파일로 분할
wimlib-imagex split /Volumes/{PATHtoISOFiles}/sources/install.wim /Volumes/{USBNames}/sources/install.swm 3800
```

여기까지 마쳤으면 큰 문제없이 설치 USB가 완성되었을 것이다. 이제 데스크탑에 usb를 연결하고 바이오스에서 부팅 순서를 바꿔 재부팅하여 윈도우를 설치하면 된다. 이 부분에 대해서 구글 검색을 하면 자세한 설명이 많이 나온다.