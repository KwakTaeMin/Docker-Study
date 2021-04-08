# Docker Grafana 구축

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
