---
title: "VS Code에서 C# 코딩하기 (3 - 유닛테스트)"
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

[이전글]({{ page.previous.url }})에서 솔루션을 만들고 라이브러리 및 콘솔 프로젝트를 만드는 법을 알아 보았다. Unit Test를 이용하여 라이브러리를 테스트 해보자.

## 닷넷 클래스 라이브러리 테스트하기

참고 사이트: [Microsoft Documentation](https://learn.microsoft.com/en-us/dotnet/core/tutorials/testing-library-with-visual-studio-code?pivots=dotnet-6-0)

### 유닛 테스트 프로젝트 생성하기

* 솔루션이 있는 폴더에서 터미널을 실행한 후 다음 입력 (유닛 테스트 프로젝트 명: `StringLibraryTest`)

```
dotnet new mstest -o StringLibraryTest
```

* `UnitTest1.cs` 파일 분석하기
  * import `Microsoft.VisualStudio.TestTools.UnitTesting` namespace
  * `UnitTest1` 클래스에 `TestClassAttribute` 어트리뷰트 추가
  * `TestMethod1` 메서드에 `TestMethodAttribute` 어트리뷰트 추가
* 솔루션에 프로젝트 추가

```
dotnet sln add StringLibraryTest/StringLibraryTest.csproj
```

* 기존에 만든 `StringLibrary`를 레퍼런스로 추가하기

```
dotnet add StringLibraryTest/StringLibraryTest.csproj reference StringLibrary/StringLibrary.csproj
```

### 유닛 테스트 메서드 변경 후 실행하기

* `UnitTest1.cs` 파일의 내용을 다음으로 변경

```cs
using Microsoft.VisualStudio.TestTools.UnitTesting;
using UtilityLibraries;

namespace StringLibraryTest;

[TestClass]
public class UnitTest1
{
    [TestMethod]
    public void TestStartsWithUpper()
    {
        // Tests that we expect to return true.
        string[] words = { "Alphabet", "Zebra", "ABC", "Αθήνα", "Москва" };
        foreach (var word in words)
        {
            bool result = word.StartsWithUpper();
            Assert.IsTrue(result,
                   string.Format("Expected for '{0}': true; Actual: {1}",
                                 word, result));
        }
    }

    [TestMethod]
    public void TestDoesNotStartWithUpper()
    {
        // Tests that we expect to return false.
        string[] words = { "alphabet", "zebra", "abc", "αυτοκινητοβιομηχανία", "государство",
                               "1234", ".", ";", " " };
        foreach (var word in words)
        {
            bool result = word.StartsWithUpper();
            Assert.IsFalse(result,
                   string.Format("Expected for '{0}': false; Actual: {1}",
                                 word, result));
        }
    }

    [TestMethod]
    public void DirectCallWithNullOrEmpty()
    {
        // Tests that we expect to return false.
        string?[] words = { string.Empty, null };
        foreach (var word in words)
        {
            bool result = StringLibrary.StartsWithUpper(word);
            Assert.IsFalse(result,
                   string.Format("Expected for '{0}': false; Actual: {1}",
                                 word == null ? "<null>" : word, result));
        }
    }
}
```

* 테스트 실행하기

```
dotnet test StringLibraryTest/StringLibraryTest.csproj
```

* 릴리즈 버전을 테스트하기

```
dotnet test StringLibraryTest/StringLibraryTest.csproj --configuration Release
```
