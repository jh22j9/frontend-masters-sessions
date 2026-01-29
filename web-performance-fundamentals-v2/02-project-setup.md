# Web Performance Fundamentals - Project Setup

## 1. 웹 성능의 정의 (Web Performance Definition)
웹 성능은 단순히 초기 로딩 속도만을 의미하지 않습니다. Todd Gardner는 성능을 다음과 같은 요소들의 합으로 정의합니다.

* **속도와 효율성 (Speed and Efficiency)**: 웹사이트가 로드(Load), 렌더링(Render), 그리고 사용자 상호작용(Interaction)에 응답하는 전반적인 과정.
* **사용자 체감 성능 (Perceptible Performance)**: 초기 로딩이 빨라도 버튼 클릭 반응이 느리거나, 스크롤이 끊긴다면 사용자는 여전히 해당 사이트를 느리다고 인식함.

## 2. 성능 저하의 주요 증상 (Factors of Slowness)
실습 앱에서 발견되는 주요 성능 문제 유형은 다음과 같습니다.

* **응답 지연 (Delays)**: 사용자가 페이지가 완료(Done)되었다고 느끼거나 상호작용할 수 있을 때까지 대기하는 긴 시간.
* **레이아웃 시프트 (Layout Shift)**: 로딩 중 요소들이 예기치 않게 움직여 사용자에게 불확실성(Unpredictable)을 주는 현상.
* **미디어 로딩 (Media Loading)**: 이미지나 비디오가 로딩되는 속도가 느리거나 재생 시 버벅임 발생.
* **인터랙션 품질 (Interaction Quality)**: 클릭 지연, 지연되는 스크롤(Laggy scrolls) 및 애니메이션 끊김 현상.

## 3. 실습 애플리케이션 진단 (Initial Application Analysis)
강의에서 사용하는 'Developer Stickers Online' 앱의 초기 상태 진단 결과입니다.

* **네트워크 패널 (Network Panel)**: 리소스 크기 약 20MB, 전체 완료(Finish)까지 약 17.8초 소요.
* **성능 패널 (Performance Panel)**: 0초에서 24초에 걸친 타임라인과 수많은 경고(Warnings) 및 연쇄 작업(Chains) 발생.
* **콘솔 로그 (Console Log)**: 브라우저가 탐지한 낮은 성능 점수(Bad performance scores) 출력.

## 4. 환경 구축 및 실행 절차 (Environment Setup)

### 1) 저장소 복제 및 의존성 설치
```bash
# 레포지토리 클론
git clone [https://github.com/trackjs/fems-web-perf-fundamentals.git](https://github.com/trackjs/fems-web-perf-fundamentals.git)

# 해당 디렉토리 이동
cd fems-web-perf-fundamentals

# 의존성 패키지(Dependencies) 설치
npm install
```

### 2) 애플리케이션 실행
```bash
# 서버 시작
npm start
```
* 서버 실행 후 브라우저에서 `http://localhost:3000` 접속 확인.

## 5. 학습 도구 활용 계획 (Tools for Optimization)
*중요: 강사 화면의 'Web Vitals Extension' 로그는 현재 크롬 개발자 도구(DevTools) 내 'Performance' 패널의 'Local and field metrics' 기능으로 통합되었습니다.*

* **네트워크 패널 (Network Panel)**: 리소스 로딩 순서, 크기 및 워터폴 차트(Waterfall Chart) 분석.
* **성능 패널 (Performance Panel)**: 메인 스레드(Main Thread) 작업 내역 분석 및 통합된 **Live Web Vitals** 지표 확인.
* **개발자 도구 (DevTools)**: 브라우저 내장 도구를 활용한 실시간 성능 최적화 전략 수립.