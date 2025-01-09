# Contents
- [Garbage Collection](#Unreal-Engine의-Garbage-Collection-시스템)
- [Network](#UE-Network)
- [String Handling](#String-Handling)

---

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

---

# String Handling

## FName
- 콘텐츠 브라우저 에서 새 에셋 이름을 지을 때, 다이내믹 머티리얼 인스턴스의 파라미터를 변경할 때, 스켈레탈 메시에서 본에 접근할 때, 모두 FName 을 사용합니다.  
FName 은 문자열 사용에 있어서 초경량 시스템을 제공하는데, 주어진 문자열이 재사용된다 해도 데이터 테이블에 한 번만 저장되는 것입니다. FName 은 대소문자를 구분하지 않습니다.  
변경도 불가능하여, 조작할 수 없습니다. 이처럼 FName 의 정적인 속성과 저장 시스템 덕에 찾기나 키로 FName 에 접근하는 속도가 빠릅니다.  
FName 서브시스템의 또다른 특징은 스트링에서 FName 변환이 해시 테이블을 사용해서 빠르다는 점입니다.

## FText
- 언리얼 엔진(UE) 에서 텍스트 현지화를 위한 주요 컴포넌트는 FText 클래스입니다.  
이 클래스는 다음 기능을 제공하여 텍스트 현지화를 지원하므로 모든 사용자 대상 텍스트는 이 클래스를 사용해야 합니다.

## FString
- FName 이나 FText 와는 달리, FString 은 조작이 가능한 유일한 스트링 클래스입니다.  
대소문자 변환, 부분문자열 발췌, 역순 등 사용가능한 메서드는 많습니다. 
검색, 변경에 다른 스트링과의 비교도 가능합니다.  
그러나 바로 그것이 FString 이 다른 불변의 스트링 클래스보다 비싸지는 이유입니다.

## Encoding
- 일반적으로 문자열 변수 리터럴을 설정할 때는 TEXT() 매크로를 사용해야 합니다.  
TEXT() 매크로를 지정하지 않으면 리터럴은 지원되는 문자가 매우 제한되는 ANSI를 사용하여 인코딩됩니다.  
FString에 전달되는 모든 ANSI 리터럴은 TCHAR(네이티브 유니코드 인코딩)로 변환해야 하므로 TEXT()를 사용하는 것이 더 효율적입니다.