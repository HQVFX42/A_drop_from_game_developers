# Unreal Engine의 Garbage Collection 시스템

## 기본 원리

### Mark & Sweep 알고리즘
- Root Set에서 시작하여 참조된 객체들을 추적(Mark)
- 추적되지 않은 객체들을 메모리에서 제거(Sweep)
- 멀티스레드 기반으로 병렬 처리 수행

### Root Set
- GC 대상에서 제외되는 기본 객체들의 집합
- AddToRoot() 함수를 통해 수동으로 Root Set에 추가 가능
- Root Set에서 시작하여 참조 그래프 구성

## 작동 과정

### 1. 도달성 분석
- Root Set부터 시작하여 모든 참조된 UObject를 추적
- 참조 그래프를 구성하여 Reachable/Unreachable 객체 구분
- 약 10만개 이상의 객체가 있을 경우 GC clusters 사용

### 2. 메모리 정리
- Unreachable로 표시된 객체들을 제거
- 해시 테이블 압축(Compaction) 수행
- 메모리 단편화 방지

## 개발자 도구

### 디버깅
- log loggarbage verbose 명령어로 GC 로그 확인
- obj gc 명령어로 수동 GC 실행
- GC 실행 시간 및 각 단계별 소요 시간 확인 가능

---

# UE-Network

## State

### Authority
- 서버가 가지는 절대적 권한
- 게임의 진실성(Source of Truth) 담당
- 모든 게임플레이 결정과 상태 변경의 최종 권한 보유
- 클라이언트의 입력을 검증하고 처리

### Simulated Proxy
- 서버의 상태를 복제하여 시뮬레이션하는 클라이언트 측 액터
- 서버로부터 업데이트를 받아 동기화
- 자체적인 판단이나 상태 변경 불가
- 서버의 상태를 단순히 따라가는 역할

### Autonomous Proxy
- 클라이언트 측에서 자체적으로 시뮬레이션 수행
- 클라이언트 예측(Client Prediction) 구현
- 로컬에서 먼저 행동을 수행하고 서버에 확인
- 네트워크 지연을 보완하는 역할
- 서버와 불일치 시 보정(Correction) 적용

### 네트워크 최적화
- 적절한 업데이트 빈도 설정
- 네트워크 대역폭 관리
- 지연 보상 메커니즘 구현
- 상태 동기화 우선순위 설정

## Replication