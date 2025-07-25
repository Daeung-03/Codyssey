# Document for Problem1 by Daeung
---


# Docker 컨테이너에 대한 이해

## 1. 가상머신과 컨테이너의 차이

### 가상머신 (Virtual Machine)

가상머신은 하이퍼바이저를 통해 완전한 운영체제를 가상화하는 기술입니다. 각 가상머신은 자체적인 운영체제, 커널, 라이브러리를 포함하고 있어 상당한 시스템 자원을 필요로 합니다.

### 컨테이너 (Container)

컨테이너는 **운영체제 수준의 가상화**를 사용하여 애플리케이션을 격리된 환경에서 실행하는 기술입니다. 컨테이너의 주요 특징은 다음과 같습니다:

- **커널 공유**: 모든 컨테이너가 호스트 머신의 OS 커널을 공유합니다
- **가벼운 실행**: 애플리케이션당 별도의 OS가 필요하지 않아 서버 효율성이 높습니다
- **빠른 부팅**: 몇 초 만에 시작되며, 가상머신처럼 몇 분이 걸리지 않습니다


### 핵심 차이점 비교

| 특징 | 가상머신 | 컨테이너 |
| :-- | :-- | :-- |
| 운영체제 | 각각 완전한 OS 필요 | 호스트 OS 커널 공유 |
| 자원 사용량 | 높음 (OS 오버헤드) | 낮음 (경량화) |
| 부팅 시간 | 분 단위 | 초 단위 |
| 격리 수준 | 완전 격리 | 프로세스 수준 격리 |
| 밀도 | 낮음 | 높음 (동일 하드웨어에 더 많은 인스턴스) |

## 2. 컨테이너와 이미지의 차이

### Docker 이미지 (Image)

Docker 이미지는 **읽기 전용 템플릿**으로, 컨테이너를 생성하기 위한 설계도 역할을 합니다. 이미지의 특징:

- **정적 스냅샷**: 환경의 정적인 스냅샷으로 애플리케이션 실행에 필요한 모든 것을 포함
- **불변성**: 한 번 생성되면 변경되지 않는 읽기 전용 파일
- **구성 요소**: 코드, 런타임, 시스템 도구, 시스템 라이브러리, 설정을 포함
- **공유 가능**: 여러 위치에서 동일한 이미지를 배포할 수 있음


### Docker 컨테이너 (Container)

컨테이너는 **이미지의 실행 가능한 인스턴스**입니다. 컨테이너의 특징:

- **실행 환경**: 이미지가 Docker Engine에서 실행될 때 생성되는 런타임 환경
- **격리된 프로세스**: 필요한 모든 파일을 포함한 격리된 프로세스
- **상태 변경**: 실행 중 파일이나 디렉토리를 생성하고 수정할 수 있음
- **생명주기**: 생성, 시작, 중지, 삭제 등의 생명주기를 가짐


### 관계 이해

- **하나의 이미지 → 여러 컨테이너**: 동일한 이미지에서 여러 개의 컨테이너를 생성할 수 있습니다
- **이미지는 설계도, 컨테이너는 실제 건물**: 이미지는 컨테이너 내부에 무엇이 들어갈지를 나타내는 읽기 전용 매니페스트이고, 컨테이너는 소프트웨어를 실행하는 환경입니다


## 3. 컨테이너 런타임의 정의

### 컨테이너 런타임 개념

컨테이너 런타임은 **컨테이너가 실행되는 환경**을 의미합니다. Docker의 경우, **Docker Engine**이 컨테이너 런타임 역할을 수행합니다.

### Docker Engine의 역할

Docker Engine은 다음과 같은 기능을 제공합니다:

- **컨테이너 관리**: Docker 컨테이너와 컨테이너 객체를 관리하는 지속적인 프로세스
- **API 처리**: Docker Engine API를 통해 전송된 요청을 수신하고 처리
- **이미지 실행**: 컨테이너 이미지가 런타임에 컨테이너가 되도록 하는 환경 제공


### 런타임 환경의 특징

컨테이너 런타임 환경은 다음을 포함합니다:

- **필요한 모든 구성 요소**: 코드, 종속성, 라이브러리 등 애플리케이션 실행에 필요한 모든 요소
- **호스트 독립성**: 호스트 머신의 종속성을 사용하지 않고 애플리케이션 코드를 실행
- **격리된 실행 환경**: 애플리케이션이 시스템의 나머지 부분에 영향을 주지 않고 실행되는 격리된 환경

컨테이너 런타임은 **이미지를 실제로 실행 가능한 컨테이너로 변환하고 관리하는 핵심 구성 요소**로서, 현대적인 애플리케이션 배포와 운영에서 중요한 역할을 담당합니다.

## 4. 컨테이너 런타임 종류 3가지

CNCF Landscape에서 제공하는 컨테이너 런타임은 컨테이너화된 애플리케이션을 실행하는 소프트웨어로, 다음과 같은 주요 3가지 유형이 있습니다.

### 4.1 containerd
**containerd**는 CNCF의 졸업(Graduated) 프로젝트로, 5017년 3월에 인큐베이팅 레벨로 승인되었다가 이후 졸업 수준으로 승격된 오픈소스 컨테이너 런타임입니다. 

- **특징**: 안정적이고 신뢰할 수 있는 컨테이너 런타임으로 설계됨
- **사용 환경**: Docker Desktop과 Kubernetes에서 광범위하게 사용됨
- **표준 준수**: OCI(Open Container Initiative) 표준을 완전히 지원

### 4.5 Docker
**Docker**는 가장 널리 알려진 컨테이너 플랫폼으로, 내부적으로 containerd를 사용하는 고수준 컨테이너 런타임입니다.

- **특징**: 사용자 친화적인 인터페이스와 풍부한 생태계 제공
- **구조**: Docker Engine이 컨테이너 런타임 역할을 수행
- **지위**: Kubernetes에서 가장 일반적으로 사용되는 컨테이너 런타임 중 하나

### 4.3 CRI-O
**CRI-O**는 Kubernetes Container Runtime Interface(CRI)를 위해 특별히 설계된 경량 컨테이너 런타임입니다.

- **특징**: Kubernetes에 최적화된 최소한의 런타임 구현
- **목적**: Kubernetes와의 직접적인 통합을 위해 개발
- **장점**: 불필요한 기능을 제거하여 보안과 안정성에 집중

## 5. Docker 이미지의 레이어

Docker 이미지는 **다중 레이어 구조**로 구성되어 있으며, 각 레이어는 이전 레이어 위에 적용되는 파일시스템 변경사항의 집합입니다.

### 5.1 레이어의 기본 개념

#### 레이어 구성
- **개별 레이어**: 특정 Dockerfile 명령어가 추가, 변경, 삭제한 파일들만 포함
- **스택 구조**: 여러 레이어가 스택처럼 쌓여서 하나의 완전한 이미지 형성
- **유니언 파일시스템**: 모든 레이어가 논리적으로 하나의 파일시스템으로 병합

#### 불변성 특징
Docker 이미지 레이어는 **항상 불변(immutable)** 입니다.

- **읽기 전용**: 한 번 생성된 레이어는 수정할 수 없음
- **변경 시 새 레이어**: 모든 변경사항은 새로운 레이어에서 적용
- **컨테이너 실행**: 컨테이너 시작 시 최상단에 읽기-쓰기 레이어 추가

### 5.2 레이어의 장점과 효율성

#### 효율적인 재사용
레이어 시스템은 다음과 같은 이점을 제공합니다:

- **효율적인 재사용**: 이미지 간 공유 레이어는 캐시되어 중복 저장되지 않음
- **빠른 배포**: 변경되지 않은 레이어는 재사용되어 빌드 및 푸시/풀 시간 단축
- **증분 업데이트**: 수정된 레이어만 전송되어 대역폭 사용량 최소화
- **빌드 일관성**: 각 레이어가 특정 Dockerfile 명령어에 매핑되어 재현 가능한 빌드 제공