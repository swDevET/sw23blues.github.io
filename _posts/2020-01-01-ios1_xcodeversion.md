--- 
published: true
title: supported version of Xcode
layout: post
author: sw23blues
category: iOS
tags: 
- version
- xcode
- swift

---


This iPhone XS is running iOS 13.3 (17C54), which may not be supported by this version of Xcode.<br>
swift3.2 xcode9.1 에서 배포한 앱이 최신 디바이스에서 죽는 이슈가 발생하였다.

XX iphone doesn’t support any of XXXX.app’s architectures. You can add XX iphone’s arm64e architecture to XXXX.app’s Architectures build setting.<br><br>


xcode10.0 이상으로 업그레이드가 필요한데, swift3.2를 사용할 수 있는 최대 버전 xcode10.1로 셋팅했다.

1. 설치된 xcode 삭제
    1. Finder 열기
    2. '응용프로그램' 에서 Xcode 삭제
    3. '이동' 메뉴에서 '폴더로 이동' 선택 
    4. ~/라이브러리/developer 이동
    5. 'xcode' 폴더 삭제 후 재시작

2. [xcode 하위버전 다운로드](https://developer.apple.com/downloads)<br><br>

xcode10.1은 iOS12.1(16B91)까지 지원, 최신버전 13.3까지 테스트 할 수 있도록 셋팅했다.

1. [iOS버전 다운로드](https://github.com/filsv/iPhoneOSDeviceSupport)
2. Pasted in /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/DeviceSupport
3. 'xcode' 재시작<br><br>


**발생 이슈 처리**

<code>Could not locate device support files. This iPhone 7 Plus (Model 1661, 1784, 1785, 1786) is running iOS 11.3 (15E216), which may not be supported by this version of Xcode.</code>

[solution](https://stackoverflow.com/questions/49720178/xcode-not-supported-for-ios-11-3-by-xcode-9-2-needed-9-3)

<code>dyld_shared_cache_extract_dylibs failed</code>

[solution](https://stackoverflow.com/questions/58971725/how-to-use-ios-13-2-3-with-xcode-10-3-dyld-shared-cache-extract-dylibs-failed)
