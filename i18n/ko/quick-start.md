# NetLogAI 빠른 시작 가이드

이 가이드는 NetLogAI를 빠르게 시작하고 핵심 기능을 사용하는 데 도움이 됩니다.

## 📋 전제 조건

시작하기 전에 다음이 설치되어 있는지 확인하세요:

- **Windows 10/11** (64비트)
- **Visual Studio 2022** 또는 **CMake Tools가 있는 Visual Studio Code**
- **Git for Windows**
- **관리자 권한** (일부 설치 단계에 필요)

## 🚀 설치 단계

### 1단계: 저장소 복제

```bash
# NetLogAI 저장소 복제
git clone https://github.com/leochong/netlogai-core.git
cd netlogai-core

# 하위 모듈 초기화 (오픈 소스 구성 요소 포함)
git submodule update --init --recursive
```

### 2단계: 빌드 환경 설정

```bash
# vcpkg 의존성 설치
./vcpkg/vcpkg install nlohmann-json lua curl openssl

# CMake 빌드 구성
cmake -B build -S . -DCMAKE_TOOLCHAIN_FILE=vcpkg/scripts/buildsystems/vcpkg.cmake
```

### 3단계: 프로젝트 빌드

```bash
# Release 모드로 빌드
cmake --build build --config Release

# 빌드 성공 확인
./build/Release/netlogai.exe --version
```

## 🎯 첫 번째 사용

### 기본 명령 테스트

```bash
# NetLogAI 상태 확인
./build/Release/netlogai.exe status

# 사용 가능한 파서 나열
./build/Release/netlogai.exe parser list

# AI 상태 확인
./build/Release/netlogai.exe ai-status
```

### 대화형 셸 시작

```bash
# 대화형 모드로 NetLogAI 실행
./build/Release/netlogai.exe

# 셸 내에서 도움말 보기
help

# 사용 가능한 명령 탐색
browse
```

## 🤖 AI 설정 (선택사항)

AI 기반 분석을 사용하려면:

```bash
# 대화형 AI 설정 마법사
./build/Release/netlogai.exe config ai

# 또는 환경 변수 설정
export NETLOGAI_AI_PROVIDER=anthropic
export NETLOGAI_ANTHROPIC_KEY=your-api-key
```

## 📊 예제 사용 사례

### 1. 로그 파일 분석

```bash
# 샘플 로그 파일 분석
./build/Release/netlogai.exe analyze sample-logs/cisco-router.log

# 특정 패턴 검색
./build/Release/netlogai.exe analyze --pattern "BGP" sample-logs/
```

### 2. AI 기반 분석

```bash
# 자연어 질문하기
./build/Release/netlogai.exe ask "네트워크에 문제가 있나요?"

# 특정 오류에 대한 설명
./build/Release/netlogai.exe ask error "%LINEPROTO-5-UPDOWN" --device-type cisco-ios
```

### 3. 장치 관리

```bash
# 네트워크 장치 추가
./build/Release/netlogai.exe device add Router1 --ip 192.168.1.1 --type cisco-ios

# 장치 목록 확인
./build/Release/netlogai.exe device list

# 장치 연결 테스트
./build/Release/netlogai.exe device show Router1
```

## 🎮 대화형 기능

### 로그 뷰어 제어

- **탐색**: `j/k` (위/아래), `Space/b` (페이지 아래/위)
- **검색**: `/` (검색), `n/N` (다음/이전 매치)
- **기능**: `l` (줄 번호), `t` (타임스탬프)
- **도움말**: `h` 또는 `?`

### 파일 브라우저

- **탐색**: 화살표 키, `Enter` (열기)
- **선택**: `Space` (토글), `a` (모두 선택)
- **작업**: `r` (새로고침), `/` (검색)

## ⚠️ 일반적인 문제 해결

### 빌드 오류

```bash
# vcpkg 의존성 재설치
./vcpkg/vcpkg remove --outdated
./vcpkg/vcpkg install nlohmann-json lua curl openssl

# 빌드 캐시 정리
rm -rf build/
cmake -B build -S . -DCMAKE_TOOLCHAIN_FILE=vcpkg/scripts/buildsystems/vcpkg.cmake
```

### 권한 문제

```bash
# 관리자 권한으로 명령 프롬프트 실행
# 또는 사용자 디렉토리에 설치
```

### AI 연결 문제

```bash
# AI 상태 확인
./build/Release/netlogai.exe ai-status

# AI 구성 테스트
./build/Release/netlogai.exe ai-test
```

## 📚 다음 단계

1. **[사용자 가이드](../user-guide.md)** - 고급 기능 학습
2. **[명령 참조](../command-reference.md)** - 모든 명령 탐색
3. **[구성 가이드](../configuration.md)** - 고급 설정
4. **[플러그인 개발](../plugin-development.md)** - 사용자 정의 기능 만들기

## 🆘 도움이 필요하세요?

- 📧 **이메일**: support@netlogai.com
- 🐛 **이슈**: [GitHub Issues](https://github.com/netlogai/netlogai-core/issues)
- 📖 **문서**: [프로젝트 위키](https://github.com/netlogai/netlogai-core/wiki)

---

축하합니다! 이제 NetLogAI를 사용할 준비가 되었습니다. 🎉