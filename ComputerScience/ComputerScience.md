# Contents
- [Pipeline Stall](#Pipelin-Stall)
- [Instruction Pipelining](#Instruction-Pipelining)
- [Branch Prediction](#Branch-Prediction)
- [Normalized Device Coordinates (NDC)](#Normalized-Device-Coordinates-(NDC))
- [Design Patterns](#Desing-Patterns)
- [Stack Meoery & Stack Frame]()

## Pipeline Stall
Pipeline stall은 CPU의 파이프라인에서 발생하는 지연 현상으로, 파이프라인의 효율성을 저하시킨다

### 종류
1. 데이터 해저드 (Data Hazard)
    - 명령어 실행 순서에 따라 데이터 의존성으로 인한 지연
	- 예: RAW (Read After Write), WAR (Write After Read), WAW (Write After Write)

1. 제어 해저드 (Control Hazard)
    - 분기 명령어에 의한 지연
	- 예: 분기 명령어의 조건 판단 결과가 나오기 전까지 다음 명령어 실행을 지연

1. 구조적 해저드 (Structural Hazard)
	- 하드웨어 자원 충돌로 인한 지연
	- 예: 메모리 접근, I/O 작업

### 해결 방법
1. Forwarding (데이터 포워딩)
	- 명령어 실행 결과를 바로 다음 명령어로 전달하여 데이터 해저드 해결
1. Branch Prediction (분기 예측)
	- 분기 명령어의 결과를 예측하여 제어 해저드 해결
1. Pipeline Flush (파이프라인 플러시)
	- 잘못된 예측 시 파이프라인을 비우고 다시 시작

## Instruction Pipelining
Instruction pipelining은 현대 프로세서 설계에서 사용되는 기술로, 명령어 처리량을 증가시키는 데 사용된다

### 정의
- 명령어 수준의 병렬 처리를 단일 프로세서 내에서 구현하는 기술
- CPU 명령어 처리를 여러 단계로 나누어 동시에 여러 명령어를 처리하는 방식

### 작동 원리
1. 명령어 처리 과정을 여러 단계로 분할
1. 각 단계를 독립적인 하드웨어 유닛에 할당
1. 여러 명령어를 동시에 처리하되, 각 명령어는 서로 다른 단계에 있음

### 일반적인 파이프라인 단계
1. 명령어 인출 (Fetch)
1. 명령어 해독 (Decode)
1. 실행 (Execute)
1. 메모리 접근 (Memory access)
1. 결과 저장 (Write back)

### 장점
- 처리량 증가: 단위 시간당 처리되는 명령어 수 증가
- 자원 활용도 향상: CPU의 여러 부분을 동시에 사용
- 전체적인 시스템 성능 향상

### 단점
- 파이프라인 해저드(Hazards) 발생 가능:
	- 데이터 해저드
	- 제어 해저드
	- 구조적 해저드

### 결론
Instruction pipelining은 현대 프로세서의 핵심 기술로,  
복잡한 명령어 처리를 효율적으로 수행하여 전반적인 시스템 성능을 크게 향상시킬 수 있다

## Branch Prediction
Branch prediction은 현대 프로세서 설계에서 중요한 기술로, 조건부 분기문의 결과를 예측하여 파이프라인 효율성을 높인다

### 정의
조건부 분기 명령어의 결과를 예측하는 하드웨어 기술
프로세서의 명령어 파이프라인 효율을 향상시키는 데 사용됨

### 작동 원리
분기 명령어 감지
분기 방향 예측 (taken 또는 not taken)
분기 목표 주소 예측 (taken일 경우)
예측된 경로의 명령어 사전 실행

### 주요 예측 기법
1. 정적 예측 (Static Prediction)
	- 항상 같은 방식으로 예측 (예: 항상 taken 또는 not taken)
	- 간단하지만 정확도가 낮음
1. 동적 예측 (Dynamic Prediction)
	- 과거 실행 이력을 기반으로 예측
	- 주요 기법:
		1. 1-bit 예측기
		1. 2-bit 예측기
		1. 상관 예측기 (Correlating Predictor)

### 장점
- 파이프라인 스톨(Pipeline stall) 감소
- 명령어 처리량 증가
- 전체적인 프로세서 성능 향상

### 단점
- 잘못된 예측 시 파이프라인 플러시 필요
- 하드웨어 복잡도 증가

### 결론
Branch prediction은 현대 프로세서의 성능을 크게 향상시키는 핵심 기술로,  
지속적인 연구와 개선이 이루어지고 있다

## Normalized Device Coordinates (NDC)
Normalized Device Coordinates (NDC)는 컴퓨터 그래픽스에서 중요한 좌표계로, 3D 객체를 2D 화면에 표시하기 위한 중간 단계

### 정의
- NDC는 3차원 공간에서 정규화된 좌표계
- OpenGL에서는 각 축의 범위가 -1에서 1 사이
- DirectX에서는 각 축의 범위가 0에서 1 사이

### 특징
- 장치 독립적인 좌표계로, 다양한 해상도와 화면 크기에 대응할 수 있다
- 클리핑 연산을 쉽게 수행할 수 있다

### NDC 변환 과정
1. 월드 좌표계에서 뷰 좌표계로 변환
2. 뷰 좌표계에서 클립 공간으로 변환 (투영 행렬 사용)
3. 원근 분할(Perspective Division) 수행
4. NDC 좌표계로 변환

### NDC의 중요성
- 렌더링 파이프라인에서 핵심적인 역할이다
- 클리핑 연산을 효율적으로 수행할 수 있게 한다
- 다양한 하드웨어에서 일관된 결과를 얻을 수 있다

### 활용
- 텍스트, 선, 마커, 다각형 등을 화면의 원하는 위치에 배치할 때 사용된다
- 3D 그래픽스 프로그래밍에서 필수적인 개념

NDC는 3D 그래픽스 파이프라인에서 중요한 중간 단계로,  
개발자들이 효율적이고 일관된 방식으로 3D 객체를 2D 화면에 표시할 수 있게 해준다

## Design Patterns
### MVVM
- MVVM(Model-View-ViewModel) 디자인 패턴은 사용자 인터페이스 로직을 비즈니스 로직과 분리하여  
애플리케이션의 유지보수성과 테스트 용이성을 향상시키는 아키텍처 패턴입니다.  
MVVM은 다음 세 가지 주요 구성 요소로 이루어져 있다:  

- Model
	- 애플리케이션의 데이터와 비즈니스 로직을 담당
	- UI와 독립적으로 동작하며, 데이터의 유효성 검사, 데이터베이스 작업, 네트워크 요청 등을 처리

- View
	- 사용자에게 보여지는 UI 요소를 담당
	- 사용자 입력을 받아 ViewModel에 전달하고, ViewModel로부터 받은 데이터를 표시
	- 데이터 바인딩을 통해 ViewModel과 연결되어 자동으로 UI를 업데이트

- ViewModel
	- View와 Model 사이의 중재자 역할
	- Model의 데이터를 View에 적합한 형태로 변환하고 가공
	- View에 필요한 데이터와 명령을 제공
	- View의 상태를 관리하고 사용자 상호작용에 대한 로직을 처리

- MVVM 패턴의 주요 특징:
1. 데이터 바인딩: View와 ViewModel 사이의 자동 동기화를 제공
1. 관심사의 분리: UI 로직과 비즈니스 로직을 명확히 분리
1. 테스트 용이성: ViewModel은 UI에 독립적이므로 단위 테스트가 용이
1. 재사용성: ViewModel은 여러 View에서 재사용될 수 있다
1. 유지보수성: 각 구성 요소가 독립적이므로 수정과 확장이 용이

- MVVM 패턴의 장점:
View와 Model 사이의 의존성이 없어 모듈화와 병렬 개발이 가능
복잡한 UI 로직을 ViewModel로 분리하여 Controller의 부담을 줄인다
데이터 바인딩을 통해 UI 업데이트 코드를 줄일 수 있다

- 단점:
간단한 UI에서는 오히려 복잡도가 증가할 수 있다
데이터 바인딩 구현에 추가적인 학습이 필요할 수 있다
복잡한 애플리케이션에서는 ViewModel이 비대해질 수 있다
MVVM 패턴은 특히 복잡한 UI와 데이터 처리가 필요한 대규모 애플리케이션에서 효과적이며, 모바일 및 데스크톱 애플리케이션 개발에 널리 사용되고 있다

### Singleton