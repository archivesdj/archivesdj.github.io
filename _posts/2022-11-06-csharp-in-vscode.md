---
title: "VS Code에서 C# 코딩하기 (1)"
categories:
    - 코딩
tags:
    - 코딩
    - C#
    - Code
    - Linux
toc: true
toc_sticky: true
---

## 필요사항

* [VS Code](https://code.visualstudio.com/) 및 [C# Extensions](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) 설치
* [.NET 6 SDK](https://dotnet.microsoft.com/en-us/download/dotnet/6.0)

## `HelloWorld` 프로젝트(앱) 만들기

참고 사이트: [Microsoft Documentation](https://learn.microsoft.com/en-us/dotnet/core/tutorials/with-visual-studio-code?pivots=dotnet-6-0)

* 프로젝트 폴더 생성: `mkdir HelloWorld`
* 프로젝트 폴더로 이동 후 VS Code 실행 `code .`
    * VS Code를 실행한 후에 `Open Folder`를 해도 됨
* `View > Terminal` 메뉴를 클릭하여 터미널 실행
    * VS Code 외부의 터미널을 사용해도 됨
* 터미널에서 다음 명령어 실행 (C# 콘솔 프로젝트 생성)
```
dotnet new console --framework net6.0
```
* 터미널에서 다음 명령어 실행
```
dotnet run
```

## 앱 디버그 하기

참고 사이트: [Microsoft Documentation](https://learn.microsoft.com/en-us/dotnet/core/tutorials/debugging-with-visual-studio-code?pivots=dotnet-6-0)

* `Build Configurations` 바꾸기
    * 새 프로젝트 생성시 기본적으로 `Debug`로 세팅되어 있어서 변경할 필요 없음. 향후 디버깅이 끝나면 `Release`로 변경하고 Publish하면 됨
* 원하는 위치에 중단점(breakpoint) 설정하기
* 터미널에서 입력을 받아서 디버깅을 하기 위한 세팅하기
    * `.vscode/lauch.json` 파일 열기
    * 콘솔 세팅을 `internalConsole`에서 `internalTerminal`로 변경
```
"console": "integratedTerminal",
```
* 디버그 뷰 실행하여 Debug 패널를 띄운 후 녹색 화살표 클릭
    * `Terminal`에서 필요한 입력 처리
    * `Debug Console`에서 원하는 값 변경하여 그 영향 보기
* `Release` 빌드
```
dotnet run --configuration Release
```
