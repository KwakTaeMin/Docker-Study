# Docker-Study
Docker Study

도커

- 오픈소스 가상화 플렛폼

- 도커 컨테이너 생성 데모
	- 젠킨스, 워드프레스, 로켓챗

- 도커란

	- 확장성 / 이식성
	- 도커가 설치되어 있다면 어디서든 커네팅너 실행
	- 특정 회사나 서비스에 종속적이지않음

	- 표준성
	- 컨테이너라는 표준으로 서버를 배포하므로 모든 서비스들의 배포과장이 동일해짐
	
	- 이미지
	- 이미지에서 컨테이너를 생성하기 때문에 반드시 이미지를 만드는 과정이 필요
	- DOCKERFILE을 이용하여 이미지를 만들고 처음부터 재현가능

	- 설정관리
	- 환경변수에 따라 동적으로 생성

	- 자원관리
	- 컨테이너는 삭제후 새로만들면 모든데이터 초기화
	- 업로드 파일을 외부스토리지와 링크하여 사용 / S3같은 별도의 저장소 필요
	- 세션이나 캐시를 MEMCAHCED나 REDIS와 같은 외부로 분리


	클라우드 이미지 보다 관리 쉬움
	다른 프로세스와 격리되어 가상머신처럼 사용하지만 성능저하 없음
	복잡한 기술을 몰라도 사용할 수 있음
	이미지 빌드 기록이 남은
	코드와 설정으로 관리 > 재현 및 수정 가능
	오픈소스 > 특정 회사 기술에 종속적이지 않음

	
도커의 미래(컨테이너의 미래)

	- 여러대의 서버와 여러개의 서비스를 관리하기 쉽게

	- 스케줄링
		- 컨ㅌ테이너를 적당한 서버에 배포해 주는 작업
		- 여러대의 서버 중 가장 할일없는 서버에 배포하거나 그냥 차례대로 배포 또는 아예 랜덤하게 배포
		- 컨테이너 개수를 여러 개로 늘리면 적당히 나눠서 배포하고 서버가 죽으면 실행 중이던 컨테이너를 다른 서버에 띄워줌
	- 클러스터링
		- 여러 개의 서버를 하나의 서버처럼 사용
		- 작게는 몇 개 안 되는 서버부터 많게는 수천 대의 서버를 하나의 클러스터로
		- 여기저기 흩어져 있는 컨테이너도 가상 네트워크를 이용하여 마치 같은 서버에 있는 것처럼 쉽게 통신

	- 서비스 디스커버리
		- 서비스를 찾아주는 기능
		- 클러스터 환경에서 컨테이너는 어느 서버에 생성될지 알 수 없고 다른 서버로 이동
		- 따라서 컨테이너와 통신을 하기 위해서 어느 서버에서 실행중인지 알아야 하고 컨테이너가 생성되고 중지될 때 어딘가에 ip와 Port 같은 정보를 업데이트 해줘야 함
		- 키 벨류 스토리지에 정보를 저장할 수도 있고 내부 DNS 서버를 이용


도커 명령어
	- run 
		- -d 백그라운드
		- -t 호스트와 컨테이너와 포트를 연결해주는 
		- -v 디렉토리
		- -e 컨테이너 내에서 사용할 수 있는 환경변수 설정
		- --name 컨테이너 이름 설정
		- --rm 프로세스 종료 시 컨테이너 삭
		- -it -i와 -t를 동시에 사용한 것으로 터미널 입력을 위한 옵션
		- --network network연결
		* Alpine 초소형 컨테이너 (5mb)
	- exec 
		- 컨테이너 안에 명령어를 실행할 경우 사용

	- ps
		- 도커 컨테이너의 상태
		- ps -a 중단된 컨테이너도 확인
	- stop
		- 도커 컨테이너 중단
	- rm
		- 도커 컨테이너 삭제
	- logs
		- 도커 컨테이너에 대한 삭제
		- logs -f name 테일 처럼 계속 볼 수 있음
	- images
		- 도커 이미지로 인하여 컨테이너 생성
	- pull 
		- 도커 이미지 다운로드
	- rmi
		- 도커 이미지 삭제
	- network
		- network create app-network 식으로 만들면 다른 컨테이너와 연결이 쉽다
	- -v
		- 로컬 데이터를 저장해놓을수 있다

	- docker compose
		- docker 명령어들을 yml 파일로 만들어 컨테이너 여러개 또한 관리가 가능	 
		- docker-compose up
		- docker-compose down 	


- 이미지
	- 레이어드 파일 시스템 기반
	- AUFS, BTRFS, Overlayfs
	- 이미지는 프로세스들가 실행되는 파일들의 집합 (환경)
	- 프로세스는 환경(파일)을 변경 가능
	- 해당 환경(파일)을 저장하여 이미지 생성

- 이미지는 읽기전용 & 쓰기전용으로 두가지로 나뉨

- 기본 베이스가되는 BaseImage를 통하여 Git또는 필요한 프로그램을 설치하여 Docker Commit을 통해 변경된 이미지를 저장할 수 있다.

- Dockerfile
	- RUN : 쉘 명령어 실행
	- CMD : 컨테이너 실행 명령어 (EntryPoint 인자로 사용)
	- EXPOSE : 오픈되는 포트 정보
	- ENV : 환경변수 설정
	- ADD : 파일 또는 디렉토리 추가 URL/ZIP 사용 가능
	- VOLUME : 외부 마운트 포인트 설정 
	- USER : RUN, CMD, ENTRYPOINT를 실행하는 사용자
	- WORKDIR : 작업 디렉토리 설정
	- ARGS : 빌드타임 환경변수 설정
	- LABEL : Key / Value 데이터
	- ONBUILD : 다른 빌드의 베이스로 사용될 때 사용하는 명령어 
	- COPY : 로컬에 있는 파일 또는 디렉토리를 이미지에 추가
	- ENTRYPOINT : 컨테이너 기본 실행 명령어
	- FROM : Base Image 설정 
	- WORKDIR : 작업 디렉토리 지정

- DokcerFile 장점
	- DockerFile을 통해 설치하게 되면 설치 기록을 명확하게 파악할 수 있어 관리하기 용이하고 도커 이미지를 만들 때 무엇이 설치되고 실행되었는지 알 수 있다.
	- DokcerFile을 통해 해당 이미지를 업데이트하거나 수정하기에 용이하다.
	
	
- docker build -t <만들 도커 이미지 이름> <Dockerfile이 위치한 폴더 디렉토리>


- Docker Hub (이미지 저장소)
	- docker login
	- docker push <image>
	- docker pull <image>
	- 무료 시 Public (한개만 Private)
	- 유료 시 Private 가능
	
