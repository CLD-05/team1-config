# 파트 전체 업무(이윤범/전지훈)
CI/CD GitOps : GitHub Actions를 이용한 자동 빌드 및 AWS ECR 푸시 자동화. ArgoCD 설치 및 GitOps 저장소 구조 설계

# 현재 담당(전지훈) 목표 업무
- GitOps 저장소 구조 설계
- Kubernetes Deployment/Service 작성
- Kustomize 환경 구성
- ArgoCD Project/Application 설정
- GitOps 배포 환경 구성

# Team1 GitOps Configuration(현 결과 적용내용)

## 개요

본 저장소는 Team1 애플리케이션의 Kubernetes 배포 설정(GitOps)을 관리하기 위한 저장소입니다.

ArgoCD를 사용하여 GitHub 저장소의 변경사항을 감지하고 Kubernetes(EKS)에 자동 배포합니다.

---

# 디렉토리 구조

```text
team1-config
│
├─ apps
│  └─ team1-app
│      ├─ base
│      │   ├─ deployment.yaml
│      │   ├─ service.yaml
│      │   └─ kustomization.yaml
│      │
│      └─ overlays
│          └─ dev
│              └─ kustomization.yaml
│
└─ argocd
    ├─ application.yaml
    └─ project.yaml
```

---

# Kubernetes 리소스 설명

## Deployment

파일:

```text
apps/team1-app/base/deployment.yaml
```

애플리케이션 Pod를 생성 및 관리하는 리소스입니다.

주요 역할

- 컨테이너 이미지 실행
- Replica 관리
- Pod 자동 재생성
- 롤링 업데이트 수행

예시

```yaml
replicas: 1
```

Pod 1개를 생성합니다.

---

## Service

파일:

```text
apps/team1-app/base/service.yaml
```

Pod에 네트워크 접근을 제공하는 리소스입니다.

주요 역할

- Pod IP 변경과 무관한 고정 접근점 제공
- 로드밸런싱
- 내부 통신 지원

---

## Kustomization

파일

```text
apps/team1-app/base/kustomization.yaml
apps/team1-app/overlays/dev/kustomization.yaml
```

Kubernetes YAML을 조합하여 환경별 설정을 관리합니다.

주요 역할

- 여러 YAML 파일 묶음 관리
- 환경별(dev/prod) 설정 분리
- 공통 설정 재사용

현재는 dev 환경을 사용합니다.

---

# ArgoCD 구성

## Project

파일

```text
argocd/project.yaml
```

ArgoCD Project 설정 파일입니다.

주요 역할

- 배포 가능한 저장소 제한
- 배포 가능한 Namespace 제한
- 권한 범위 관리

---

## Application

파일

```text
argocd/application.yaml
```

ArgoCD가 실제 배포를 수행하기 위한 설정 파일입니다.

주요 역할

- Git 저장소 감시
- Kubernetes 배포 수행
- Sync 상태 관리
- 자동 배포 지원

---

# GitOps 배포 흐름

```text
개발자
 ↓
GitHub(team1-config)
 ↓
ArgoCD 감지
 ↓
Kubernetes(EKS)
 ↓
Deployment 적용
 ↓
Pod 생성
 ↓
Service 연결
```

Git 저장소의 변경사항이 발생하면 ArgoCD가 이를 감지하여 Kubernetes 클러스터에 자동 반영합니다.

---

# 개발 환경(dev)

현재 프로젝트는 dev 환경 기준으로 구성되어 있습니다.

설정 위치

```text
apps/team1-app/overlays/dev
```

향후 운영 환경(prod)이 필요할 경우 다음 구조로 확장 가능합니다.

```text
apps/team1-app/overlays
├─ dev
└─ prod
```

---

# 작성자

GitOps 초기 구성

- Deployment 작성
- Service 작성
- Kustomization 작성
- ArgoCD Project 작성
- ArgoCD Application 작성