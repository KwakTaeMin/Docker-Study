# Docker Monitering System 구축

## 사내 Grafana 구축 목적
- IIS 모니터링 대시보드
- Windows Server 모니터링 대시보드 
- 문제 발생 시 Notification 할 수 있는 시스템 필요

## 개인적인 목표
- 도커 컨테이너 생성 및 관리 방법 (docker-compose.yml)
- 도커 이미지 생성 관리 방법 (Dockerfile)

## Docker Grafana Image 다운로드 
```
docker pull grafana/grafana
//도커 이미지 다운로드 확인
docker images
```

## Docker Grafana 실행
```
docker run -d -p 3000:3000 --name grafana grafana/grafana:latest
//도커 프로세스 확인
docker ps
```

## Grafana 사용을 위한 사내 방화벽 요청
- 서버의 root 계정권한이 없어 Server 방화벽 및 Cloud 방화벽 Port 3000 오픈을 요청하여 진행

## Grafana 접속확인
- http://{SERVER_IP}:3000
- 아이디 admin / 패스워드 admin 입력 후 로그인 확인


## Docker Promethous Image 다운로드
```
docker pull prom/prometheus
```

## Docker Promethous 실행
```
docker run \
    -p 9090:9090 \
    -v /path/to/prometheus.yml:/etc/prometheus/prometheus.yml \
    prom/prometheus
```
