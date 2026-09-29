# Docker + GitHub Actions CI/CD Pipeline

## 1. 프로젝트 개요

Docker와 GitHub Actions를 활용하여
소스 코드 변경부터 Docker 이미지 빌드,
Docker Hub Push, 원격 Linux 서버 자동 배포까지
구현한 CI/CD 프로젝트입니다.

### 목표

- Git 기반 소스 코드 관리
- GitHub Actions를 이용한 CI/CD 자동화
- Docker 이미지 자동 빌드 및 테스트
- Docker Hub 이미지 저장 및 관리
- SSH를 통한 원격 서버 자동 배포

## 2. 아키텍처
                    ┌─────────────┐
                    │  Developer  │
                    └──────┬──────┘
                           │
                       git push
                           │
                           ▼
                    ┌─────────────┐
                    │   GitHub    │
                    └──────┬──────┘
                           │
                           ▼
                 ┌───────────────────┐
                 │  GitHub Actions   │
                 │                   │
                 │  Docker Build     │
                 │  Container Test   │
                 │  Docker Push      │
                 └─────────┬─────────┘
                           │
                           ▼
                    ┌─────────────┐
                    │ Docker Hub  │
                    └──────┬──────┘
                           │
                       SSH Deploy
                           │
                           ▼
                 ┌───────────────────┐
                 │    devops-lab     │
                 │   Rocky Linux 8.9 │
                 │                   │
                 │  Docker Pull      │
                 │  Container Run    │
                 └───────────────────┘
