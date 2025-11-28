# C# → WebAssembly 기본 테스트 가이드

Unity 없이 순수 C# 코드만으로 덧셈 함수를 작성하고, WebAssembly로 변환하여 브라우저 콘솔에 로그를 출력하는 가이드입니다.

**사용 기술:** .NET + Blazor WebAssembly

---

## 📋 투두 리스트

- [ ] .NET SDK 설치
- [ ] VSCode에 C# 개발 환경 구축
- [ ] Blazor WebAssembly 프로젝트 생성
- [ ] C# 덧셈 함수 작성
- [ ] 브라우저 콘솔에 로그 출력하는 코드 작성
- [ ] 프로젝트 실행 및 브라우저에서 테스트

---

## 1️⃣ .NET SDK 설치

### macOS

```bash
# Homebrew 사용
brew install --cask dotnet-sdk

# 또는 공식 사이트에서 다운로드
# https://dotnet.microsoft.com/download
```

### 설치 확인

```bash
dotnet --version
# 출력 예: 8.0.100
```

---

## 2️⃣ VSCode C# 개발 환경 구축

### 필요한 확장 설치

1. **C# Dev Kit** (Microsoft 제공)
   - VSCode 실행
   - Extensions (Cmd+Shift+X)
   - "C# Dev Kit" 검색 및 설치

2. 자동으로 함께 설치되는 것들:
   - C# (기본 C# 언어 지원)
   - IntelliCode for C# (AI 자동완성)

---

## 3️⃣ Blazor WebAssembly 프로젝트 생성

### 프로젝트 생성

```bash
# 프로젝트 디렉토리로 이동
cd /Users/dopplix/git/pengpeng

# Blazor WebAssembly 프로젝트 생성
dotnet new blazorwasm -o CSharpWasmTest

# 프로젝트 폴더로 이동
cd CSharpWasmTest
```

### VSCode로 프로젝트 열기

```bash
code .
```

---

## 4️⃣ C# 덧셈 함수 작성

### Pages/Index.razor 파일 수정

기존 `Pages/Index.razor` 파일을 다음과 같이 수정:

```razor
@page "/"
@inject IJSRuntime JS

<PageTitle>C# WASM Test</PageTitle>

<h1>C# WebAssembly 덧셈 테스트</h1>

<p>브라우저 콘솔(F12)을 열어서 결과를 확인하세요!</p>

@code {
    protected override async Task OnInitializedAsync()
    {
        // 덧셈 함수 호출
        int result = Add(10, 25);

        // 브라우저 콘솔에 출력
        await JS.InvokeVoidAsync("console.log", $"10 + 25 = {result}");
        await JS.InvokeVoidAsync("console.log", "덧셈 테스트 성공!");
        await JS.InvokeVoidAsync("console.log", $"C# 코드가 WebAssembly로 실행되었습니다!");
    }

    // 덧셈 함수
    int Add(int a, int b)
    {
        return a + b;
    }
}
```

### 코드 설명

- `@inject IJSRuntime JS`: JavaScript와 통신하기 위한 서비스 주입
- `OnInitializedAsync()`: 페이지 로드 시 자동 실행
- `Add(int a, int b)`: 순수 C# 덧셈 함수
- `JS.InvokeVoidAsync("console.log", ...)`: C#에서 브라우저 콘솔로 출력

---

## 5️⃣ 프로젝트 실행

### 개발 서버 실행

```bash
dotnet run
```

출력 예시:
```
info: Microsoft.Hosting.Lifetime[14]
      Now listening on: http://localhost:5000
info: Microsoft.Hosting.Lifetime[0]
      Application started.
```

### 브라우저에서 확인

1. **브라우저 열기**
   - http://localhost:5000 접속

2. **개발자 도구 열기**
   - Chrome/Edge: `F12` 또는 `Cmd+Option+I`
   - Safari: `Cmd+Option+C`

3. **Console 탭에서 확인**
   ```
   10 + 25 = 35
   덧셈 테스트 성공!
   C# 코드가 WebAssembly로 실행되었습니다!
   ```

---

## 🔍 실제로 일어나는 일

### 빌드 프로세스

```
C# 코드 (Add 함수)
    ↓ .NET 컴파일러
IL (Intermediate Language)
    ↓ Blazor WebAssembly
WebAssembly (.wasm)
    ↓ 브라우저
실행!
```

### 생성되는 파일들

프로젝트를 빌드하면 `bin/Debug/net8.0/wwwroot/_framework/` 폴더에:

```
_framework/
├── dotnet.wasm                    # .NET 런타임 (WASM)
├── CSharpWasmTest.dll            # 여러분의 C# 코드가 컴파일된 DLL
├── blazor.boot.json              # 부트스트랩 설정
└── ... (기타 .NET 라이브러리들)
```

**중요:** C# 코드가 직접 WASM으로 변환되는 게 아니라:
1. C# → .NET IL (DLL)
2. 브라우저에서 `dotnet.wasm` (WASM으로 된 .NET 런타임)이 DLL을 실행

---

## 🚀 빌드 및 배포

### 프로덕션 빌드

```bash
dotnet publish -c Release
```

빌드된 파일 위치:
```
bin/Release/net8.0/publish/wwwroot/
```

이 폴더를 웹 서버에 배포하면 됩니다 (GitHub Pages, Netlify, Vercel 등).

---

## 📊 프로젝트 구조

```
CSharpWasmTest/
├── Program.cs              # 앱 진입점
├── Pages/
│   └── Index.razor        # 여기에 덧셈 함수 작성
├── wwwroot/
│   └── index.html         # HTML 템플릿
└── CSharpWasmTest.csproj  # 프로젝트 설정
```

---

## 💡 다른 방법: 더 간단한 콘솔 출력

만약 HTML/UI 없이 순수하게 C# → WASM → 콘솔만 테스트하고 싶다면:

### Program.cs 직접 수정

```csharp
using Microsoft.AspNetCore.Components.WebAssembly.Hosting;
using Microsoft.JSInterop;

var builder = WebAssemblyHostBuilder.CreateDefault(args);

var host = builder.Build();

// JSRuntime 가져오기
var jsRuntime = host.Services.GetRequiredService<IJSRuntime>();

// 덧셈 함수
int Add(int a, int b) => a + b;

// 실행
int result = Add(10, 25);
await jsRuntime.InvokeVoidAsync("console.log", $"10 + 25 = {result}");
await jsRuntime.InvokeVoidAsync("console.log", "덧셈 테스트 성공!");

await host.RunAsync();
```

---

## 🔧 트러블슈팅

### dotnet 명령어를 찾을 수 없음
```bash
# PATH 확인
echo $PATH

# .NET SDK 재설치
brew reinstall --cask dotnet-sdk
```

### VSCode IntelliSense가 작동하지 않음
- VSCode 재시작
- `Cmd+Shift+P` → "OmniSharp: Restart OmniSharp"

### 포트 5000이 이미 사용 중
```bash
# 다른 포트 사용
dotnet run --urls "http://localhost:5001"
```

---

## ✅ 결과 확인

브라우저 콘솔에서 이렇게 출력되면 성공:
```
10 + 25 = 35
덧셈 테스트 성공!
C# 코드가 WebAssembly로 실행되었습니다!
```

**축하합니다!** C# 코드가 WebAssembly로 변환되어 브라우저에서 실행되었습니다!

---

## 🎯 다음 단계

1. **C#에서 JavaScript 함수 호출하기**
2. **JavaScript에서 C# 함수 호출하기** (JSImport/JSExport)
3. **텔레그램 Web App API 연동**

---

## 🔗 참고 자료

- Blazor 공식 문서: https://learn.microsoft.com/aspnet/core/blazor/
- .NET WebAssembly: https://dotnet.microsoft.com/apps/aspnet/web-apps/blazor
- C# JavaScript 상호운용: https://learn.microsoft.com/aspnet/core/blazor/javascript-interoperability/
