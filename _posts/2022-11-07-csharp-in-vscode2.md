---
title: "VS Code에서 C# 코딩하기 (2 - 클래스라이브러리)"
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

[이전글]({{ page.previous.url }})에서 프로젝트를 만들고 디버그 및 릴리스로 컴파일하여 실행하고, 최종적으로 배포 버전을 만드는 법을 알아 보았다.
이제 라이브러리를 만드는 법을 알아보자.

## 라이브러리 만들기

### 솔루션 만들기

참고 사이트: [Microsoft Documentation](https://learn.microsoft.com/en-us/dotnet/core/tutorials/library-with-visual-studio-code?pivots=dotnet-6-0)

* 프로젝트 폴더 생성: `mkdir ClassLibraryProjects`
* 프로젝트 폴더로 이동 후 VS Code 실행 `code .`
    * VS Code를 실행한 후에 `Open Folder`를 해도 됨
* `View > Terminal` 메뉴를 클릭하여 터미널 실행
    * VS Code 외부의 터미널을 사용해도 됨
* 터미널에서 다음 명령어 실행
```
dotnet new sln
```

### 클래스 라이브러리 프로젝트 만들기

* 솔루션에 추가할 `StringLibrary` 프로젝트 만들기 (터미널에 다음 입력)
```
dotnet new classlib -o StringLibrary
```
* `-o` 또는 `--output` 옵션은 현재 폴더 아래에 새로운 폴더를 만들고 거기에 만든다는 뜻
* 솔루션 파일에 위에서 만든 프로젝트 추가하기
```
dotnet sln add StringLibrary/StringLibrary.csproj
```
* `Class1.cs`파일에 다음 내용을 복사
```cs
namespace UtilityLibraries;

public static class StringLibrary
{
    public static bool StartsWithUpper(this string? str)
    {
        if (string.IsNullOrWhiteSpace(str))
            return false;

        char ch = str[0];
        return char.IsUpper(ch);
    }
}
```
* 빌드하여 에러가 없는지 체크
```
dotnet build
```

### 콘솔 앱 만들기

* 솔루션에 추가할 `ShowCase` 프로젝트 만들기
```
dotnet new console -o ShowCase
```
* 솔루션 파일에 위에서 만든 프로젝트 추가하기
```
dotnet sln add ShowCase/ShowCase.csproj
```
* `ShowCase/Program.cs` 파일에 다음 코드 복사하기
```cs
using UtilityLibraries;

class Program
{
    static void Main(string[] args)
    {
        int row = 0;

        do
        {
            if (row == 0 || row >= 25)
                ResetConsole();

            string? input = Console.ReadLine();
            if (string.IsNullOrEmpty(input)) break;
            Console.WriteLine($"Input: {input}");
            Console.WriteLine("Begins with uppercase? " +
                 $"{(input.StartsWithUpper() ? "Yes" : "No")}");
            Console.WriteLine();
            row += 4;
        } while (true);
        return;

        // Declare a ResetConsole local method
        void ResetConsole()
        {
            if (row > 0)
            {
                Console.WriteLine("Press any key to continue...");
                Console.ReadKey();
            }
            Console.Clear();
            Console.WriteLine($"{Environment.NewLine}Press <Enter> only to exit; otherwise, enter a string and press <Enter>:{Environment.NewLine}");
            row = 3;
        }
    }
}
```
* 프로젝트 레퍼런스 추가하기
    * `ShowCase`프로젝트에서 `StringLibrary`프로젝트를 참조하기 때문에 레퍼런스를 추가해 주어야 한다.
```
dotnet add ShowCase/ShowCase.csproj reference StringLibrary/StringLibrary.csproj
```
* 앱 실행하기
```
dotnet run --project ShowCase/ShowCase.csproj
```