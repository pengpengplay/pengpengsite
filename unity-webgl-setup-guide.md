# Unity WebGL 테스트 환경 구축 가이드

Unity에서 C# 코드로 덧셈 함수를 만들고, WebGL로 빌드하여 브라우저 콘솔에 로그를 출력하는 테스트 프로젝트를 만드는 가이드입니다.

---

## 📋 투두 리스트

- [ ] VSCode에 C# 개발 환경 구축
- [ ] Unity Personal 설치
- [ ] Unity에서 새 프로젝트 생성
- [ ] C# 스크립트 작성 - 덧셈 함수 및 로그 출력
- [ ] Unity Scene에 GameObject 생성 및 스크립트 연결
- [ ] WebGL 플랫폼으로 빌드 설정
- [ ] WebGL 빌드 실행 및 브라우저에서 콘솔 확인

---

## 1️⃣ VSCode에 C# 개발 환경 구축

### 필요한 도구 설치

1. **VSCode 설치**
   - https://code.visualstudio.com/

2. **C# Dev Kit 확장 설치**
   - VSCode 실행
   - Extensions (Cmd+Shift+X)
   - "C# Dev Kit" 검색 및 설치 (Microsoft 제공)
   - 자동으로 C# 확장도 함께 설치됨

3. **.NET SDK 설치** (선택사항 - Unity는 Mono 사용하지만 VSCode IntelliSense를 위해)
   - https://dotnet.microsoft.com/download
   - 최신 .NET SDK 다운로드 및 설치

---

## 2️⃣ Unity Personal 설치

### Unity Hub 설치

1. **Unity Hub 다운로드**
   - https://unity.com/download
   - "Download Unity Hub" 클릭

2. **Unity Hub 설치 및 실행**
   - 다운로드한 파일 실행
   - 설치 완료 후 Unity Hub 실행

### Unity Editor 설치

1. **Unity Hub에서 로그인**
   - Unity 계정 생성 또는 로그인
   - Personal 라이선스 활성화 (무료)

2. **Unity Editor 설치**
   - Unity Hub > Installs 탭
   - "Install Editor" 클릭
   - 추천: LTS (Long Term Support) 버전 선택
   - 모듈 선택 시 **WebGL Build Support** 반드시 체크!
   - 설치 진행

---

## 3️⃣ Unity 프로젝트 생성

1. **새 프로젝트 만들기**
   - Unity Hub > Projects 탭
   - "New Project" 클릭
   - 템플릿: **3D (Built-in Render Pipeline)** 또는 **2D** 선택
   - 프로젝트 이름: `WebGL-Test`
   - 위치 선택
   - "Create Project" 클릭

2. **VSCode를 외부 에디터로 설정**
   - Unity Editor 메뉴: `Edit > Preferences` (Mac: `Unity > Settings`)
   - `External Tools` 선택
   - `External Script Editor`: VSCode 선택

---

## 4️⃣ C# 스크립트 작성

### 스크립트 생성

1. **Unity Project 창에서**
   - `Assets` 폴더 우클릭
   - `Create > C# Script`
   - 이름: `AdditionTest`

2. **VSCode에서 편집**
   - 생성된 `AdditionTest.cs` 더블클릭
   - VSCode가 자동으로 열림

### 코드 작성

```csharp
using UnityEngine;

public class AdditionTest : MonoBehaviour
{
    // Start는 게임 시작 시 한 번 호출됨
    void Start()
    {
        // 덧셈 함수 호출
        int result = Add(10, 25);

        // Unity 콘솔에 출력 (WebGL에서 브라우저 콘솔로 출력됨)
        Debug.Log("10 + 25 = " + result);
        Debug.Log("덧셈 테스트 성공!");
    }

    // 덧셈 함수
    int Add(int a, int b)
    {
        return a + b;
    }
}
```

**저장**: `Cmd+S` (Mac) 또는 `Ctrl+S` (Windows)

---

## 5️⃣ Unity Scene에 스크립트 연결

### GameObject 생성 및 스크립트 추가

1. **빈 GameObject 생성**
   - Unity Hierarchy 창에서 우클릭
   - `Create Empty`
   - 이름을 `GameManager`로 변경

2. **스크립트 연결**
   - Project 창에서 `AdditionTest.cs` 드래그
   - Hierarchy의 `GameManager`에 드롭

   또는

   - `GameManager` 선택
   - Inspector 창 하단의 "Add Component" 클릭
   - "AdditionTest" 검색 후 추가

3. **씬 저장**
   - `File > Save` (Cmd+S)
   - 씬 이름: `TestScene`

---

## 6️⃣ WebGL 빌드 설정

### 빌드 플랫폼 전환

1. **Build Settings 열기**
   - Unity 메뉴: `File > Build Settings`

2. **WebGL 플랫폼 선택**
   - 플랫폼 목록에서 "WebGL" 선택
   - "Switch Platform" 클릭
   - (처음이면 몇 분 소요될 수 있음)

3. **Scenes in Build 확인**
   - `TestScene`이 체크되어 있는지 확인
   - 없으면 "Add Open Scenes" 클릭

### WebGL 플레이어 설정 (선택사항)

1. **Player Settings 열기**
   - Build Settings 창에서 "Player Settings" 클릭

2. **압축 설정** (개발 중에는 Disabled 추천)
   - `Publishing Settings > Compression Format`: Disabled
   - (빌드 속도 향상을 위해, 배포 시에는 Gzip 또는 Brotli 사용)

---

## 7️⃣ WebGL 빌드 및 실행

### 빌드 실행

1. **빌드**
   - Build Settings 창에서 "Build And Run" 클릭
   - 빌드 폴더 선택 (예: `WebGL-Build`)
   - 빌드 진행 (처음이면 5-10분 소요 가능)

2. **자동으로 브라우저 실행**
   - 빌드 완료 후 자동으로 기본 브라우저가 열림
   - 로컬 서버(`localhost:xxxx`)에서 게임 실행

### 브라우저 콘솔 확인

1. **개발자 도구 열기**
   - Chrome/Edge: `F12` 또는 `Cmd+Option+I` (Mac)
   - Safari: `Cmd+Option+C`

2. **Console 탭에서 로그 확인**
   ```
   10 + 25 = 35
   덧셈 테스트 성공!
   ```

---

## 📁 빌드 결과물 구조

```
WebGL-Build/
├── index.html              # 게임 로더 HTML
├── TemplateData/           # UI 템플릿 파일
└── Build/
    ├── WebGL-Build.loader.js      # JavaScript 로더
    ├── WebGL-Build.framework.js   # Unity 프레임워크
    ├── WebGL-Build.wasm          # C# 코드가 변환된 WebAssembly
    └── WebGL-Build.data          # 게임 에셋 및 리소스
```

---

## 🔍 트러블슈팅

### 빌드 에러: WebGL Build Support가 없음
- Unity Hub > Installs > 설치된 버전 옆 톱니바퀴 > Add Modules
- WebGL Build Support 체크 후 설치

### 브라우저에서 게임이 안 열림
- 반드시 "Build And Run" 사용 (로컬 서버 필요)
- 직접 `index.html` 더블클릭하면 CORS 에러 발생

### 로컬 서버로 수동 실행하려면
```bash
# Python 3가 설치되어 있다면
cd WebGL-Build
python3 -m http.server 8000

# 브라우저에서 http://localhost:8000 접속
```

### VSCode에서 IntelliSense가 안 됨
- Unity에서 `Edit > Preferences > External Tools`
- "Regenerate project files" 클릭
- VSCode 재시작

---

## ✅ 다음 단계

이제 Unity C# → WebAssembly → 브라우저 콘솔 출력까지 확인했습니다!

**다음으로 할 수 있는 것:**
1. JavaScript와 C# 간 통신 (`.jslib` 플러그인)
2. 텔레그램 Web App API 연동
3. 실제 게임 로직 구현

---

## 🔗 참고 자료

- Unity 공식 문서: https://docs.unity3d.com/
- Unity WebGL 가이드: https://docs.unity3d.com/Manual/webgl-building.html
- Telegram Web Apps: https://core.telegram.org/bots/webapps
