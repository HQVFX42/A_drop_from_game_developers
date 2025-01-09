# Contents
- [Instruction Pipelining](#Instruction-Pipelining)
- [Branch Prediction](#Branch-Prediction)
- [Normalized Device Coordinates (NDC)](#Normalized-Device-Coordinates-(NDC))

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
Branch prediction은 현대 프로세서 설계에서 중요한 기술로, 조건부 분기문의 결과를 예측하여 파이프라인 효율성을 높입니다.

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
- 파이프라인 스톨 감소
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