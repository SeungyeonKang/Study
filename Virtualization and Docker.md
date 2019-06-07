# VIRTUAL MACHINE / VAGRANT / CONTAINER



- 원래 하나의 OS에는 한 대의 서버만 구동이 가능했다. 그러나 하나의 서버를 실행시키는데 그렇게 많은 CPU가 들지 않기 때문에 자원이 남게되는 비효율성이 발생하였고, 이 남은 공간을 이용해 다른 Guest OS를 실행시켜 다른 서버를 띄울 수 있는 "가상화"가 등장하게되었다. 이러한 가상화를 가능하도로 Hypervisor를 지원해주는 툴이 Virtual Box, VMware이다.

- Virtual Box  등과 같은 가상화 툴을 이용하면, 실제로 Guest OS를 설치하고, 설치한 OS 마다 내부적인 작업들(apt-get 설치 등,, )을 해주어야 하는데, 이러한 번거로운 과정을 줄이고, 실제 OS 설치 없이 이미지의 실행만으로 간단하게 가상화를 도와주는 툴인 "Vagrant"가 등장하게 되었다. Vagrant를 이용하면, 

  ```
  vagrant init ubuntu/xenial64
  ```

  와 같은 명령어로 이미지를 다운받아 Vagrantfile을 생성하고, 아래와 같이 Guest OS로 편하게 접속이 가능하다.

  ```
  vagrant up	//vagrantfile에 정의된 이미지를 띄움
  vagrant ssh 	// 해당 환경에 접속함
  ```

- 그러나, 이러한 가상화는 하나의 애플리케이션을 돌리는데 하나의 OS를 필요로하기 떄문에, 애플리케이션을 돌리는 자원 뿐만 아니라 그 외의 자원들도 많이 소모하게 된다는 단점이 있다. 그래서 등장하게된 것이 "Container"라는 것인데, 이는 Guest OS 없이, 호스트 OS를 같이 사용하고, 중복되는 바이너리 파일과 라이브러리들도 같이 사용하여 하나의 애플리케이션에 필요한 환경만을 제공해주는 것이었다.

  



# DOCKER

- 리눅스 컨테이너 개념을 실현할 수 있게 해주는 툴이 Docker이다. Docker는 리눅스 컨테이너를 기반으로 하기 때문에, 기본적으로 리눅스 환경에서만 사용이 가능하다. 따라서 windows나 mac에서 사용하려면 가상머신으로 리눅스를 설치하고, 그 내부에서 Docker를 설치해서 사용해야 한다.
- Windows와 Mac에서 Docker를 사용할 수 있게 해주는 Docker Machine이 있지만, 이것도 결국 내부적으로는 가상 머신으로 리눅스를 띄우고, 그 안에서 Docker를 사용한다. 

- Docker를 설치하여 docker pull로 원하는 이미지를 가져오고, 해당 이미지를 기반으로 컨테이너를 실행시킬수 있다.

  - 이미지와 컨테이너는 명백히 다른 개념이다. 특정 이미지를 기반으로 컨테이너를 실행시키고, 그 안에서 여러 작업들을 하더라도 이미지에는 영향을 받지 않는다. 컨테이너를 종료하고 이미지를 재실행시키면, 이미지 원본 그대로를 기반으로한 컨테이너가 실행된다. 

  - 특정 이미지를 기반으로한 컨테이너가 실행중일 경우, 이미지는 삭제되지 않는다. 이미지를 삭제하려면 컨테이너 먼저 종료하고 삭제 가능하다

  - ```
    docker rm	[container id]	//컨테이너 삭제
    docker rmi [image id]			//이미지 삭제
    ```

    

- 특정 이미지를 실행시키고, 다른 설치 등의 추가적인 작업이 필요한 경우, 특정한 이미지를 출발점으로 새로운 이미지 구성에 필요한 일련의 명령어들을 저장시키고 컨테이너가 실행될 때 명령어가 실행되도록 할 수 있는 Dockerfile을 이용하면 된다.

- Docker-compose는 실행에 필요한 옵션을 적어둘 수 있고, 여러 컨테이너의 실행순서와 의존성도 관리할 수 있다.

- 즉, 여러개의 컨테이너를 한번에 실행시키고 싶은 경우, Docker-compose.yml 파일을 작성하여 한꺼번에 실행시킬 수 있고, 컨테이너 실행시마다 안의 내부적인 작업이 필요한 경우 Dockerfile로 컨테이너 셋팅작업을 할 수 있다.

   





### Reference

Virtualization, Container

https://blog.lgcns.com/860

Vagrant vs Docker

https://khanrc.tistory.com/entry/Docker-Vagrant

Docker 이해

[https://www.44bits.io/ko/post/almost-perfect-development-environment-with-docker-and-docker-compose#docker-compose.yml-%ED%8C%8C%EC%9D%BC](https://www.44bits.io/ko/post/almost-perfect-development-environment-with-docker-and-docker-compose#docker-compose.yml-파일)

DockerFile과 Docker-compse

[https://kycfeel.github.io/2017/03/15/DockerFile%EA%B3%BC-Docker-Compose/](https://kycfeel.github.io/2017/03/15/DockerFile과-Docker-Compose/)