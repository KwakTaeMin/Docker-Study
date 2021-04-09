# Docker Monitering System 구축

## 사내 Monitoring 구축 목적
- IIS 모니터링 대시보드
- Windows Server 모니터링 대시보드 
- 문제 발생 시 Notification 할 수 있는 시스템 필요

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


## Docker Prometheus Image 다운로드
```
docker pull prom/prometheus
```

## Docker Prometheus Yml 파일 생성 
```
global:
  scrape_interval:     15s # By default, scrape targets every 15 seconds.

  # Attach these labels to any time series or alerts when communicating with
  # external systems (federation, remote storage, Alertmanager).
  external_labels:
    monitor: 'codelab-monitor'

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: 'prometheus'

    # Override the global default and scrape targets from this job every 5 seconds.
    scrape_interval: 5s

    static_configs:
      - targets: ['localhost:9090']
```

## Docker Prometheus 실행
- prometheus.yml 파일을 -v 볼륨으로 연결하여 실행한다.
```
 docker run -d \
  -p 9090:9090 \
  -v /home/rtdadm/monitoring/prometheus/config:/etc/prometheus \
  prom/prometheus
```

## Prometheus 접속 확인
- http://{SERVER_IP}:9090


## Grafana Datasource Prometheus 연결
![image](https://user-images.githubusercontent.com/11844343/114117100-eb436d80-9920-11eb-9086-9178fedc6eac.png)


## Prometheus Window Exporter 설치
- https://github.com/prometheus-community/windows_exporter/releases
- 해당 Windows 버전에 맞추어 설치 
- Window Exporter는 9182 포트로 열린다.

## Prometheus Window Exporter 설치 확인 
- http://localhost:9182/metrics
![image](https://user-images.githubusercontent.com/11844343/114119281-4ecf9a00-9925-11eb-83d3-61ee0667de57.png)

## Prometheus Window Exporter 서버를 prometheus.yml에 추가 등록
```
  - job_name: 'exporter_server'
    static_configs:
      - targets: ['{EXPORTER_SERVER}:9182']
```

## Prometheus 도커 재실행 
```
docker stop {CONTAINER_ID}
docker run -d \
  -p 9090:9090 \
  -v /home/rtdadm/monitoring/prometheus/config:/etc/prometheus \
  prom/prometheus
```
## Grafana Dashboard Import 
- https://grafana.com/grafana/dashboards/6593/revisions
- 해당 인터페이스가 맞는 Dashboard 


## Monitoring docker-compose.yml 파일 생성 관리
- Grafana에 볼륨 정보 추가(Dashboard나 계정 정보가 사라지지 않도록 볼륨 설정)
```
version: '2'

services:
  prometheus:
    image: prom/prometheus
    container_name: 'prometheus_1'
    ports:
    - "9090:9090"
    volumes:
    - /home/rtdadm/monitoring/prometheus/config:/etc/prometheus
  grafana:
    image: grafana/grafana:latest
    container_name: 'grafana_1'
    ports:
    - "3000:3000"
    volumes:
    - /home/rtdadm/monitoring/grafana/data:/var/lib/grafana
```

## Monitoring System 실행 및 종료 
```
docker-compose up -d
docker-compose down
```

