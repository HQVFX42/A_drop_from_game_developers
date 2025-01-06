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
