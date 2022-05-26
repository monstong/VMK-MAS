# VMK-MAS
VMware Korea study group for Modern application

### **본 문서는 개인적 스터디용이며, 모든 내용은 아래 링크들로부터 발췌하였습니다.**
## Members and Personal Goal
- Steven Lim
> My goal is...

- Yoon Hwan Cho
> My goal is...

- Hyejin Yoon
> My goal is...

- Hong Seok Kang
> My goal is to re-cap the concept of container and k8s architecture and to understand modern app architecture and whole workflow then can find how to advise modern app offering to a customer/partner.


## 0. Pre-requisites
- install the Git and VS Code in your laptop.
- 

## 1. Ground rule
- Rule 1
- Rule 2
- Rule 3
- 


## 2. Session Topic

### Topic candidate
- container
- k8s
- MSA
- CI/CD pipeline



### **Session #1. What's the container and k8s ?** 
- Host : Hong Seok Kang
- Date : 2022.05.27 11:00 AM KST
- Attendee : 
- Location : offline
- Agenda

  ### **들어가기 전에 ..**
    - 인프라쟁이가 하는 일 
      하드웨어 > OS , DB > 계정/보안 설정 > 인프라 HA 구성 > [메인 어플리케이션 설치] > 최종 테스트 후 인계
      그리고... 패칭, 모니터링, 용량 관리, 변경 관리 , 보안 관리 등등

    - 어플리케이션 운영자/개발자가 하는 일
      아키텍쳐 선정 , 프레임워크 선택  , 소프트웨어 선택
      관리 측면 : 소스 레포지토리 및 버전 관리 (중앙 집중식 - 메이븐 , 엔트  , 분산 관리 - Git) , 빌드 관리 , 배포 관리 
    https://goddaehee.tistory.com/199

    - 모놀리스 vs MSA
      통 ? vs 쪼개기 ? 
      쪼갠 애들 끼리 통신은 ? 

  - Servicemesh vs API gateway vs Messaging Queue
  https://arcentry.com/blog/api-gateway-vs-service-mesh-vs-message-queue/ 


  ### **컨테이너 기반 기술 : namespace, cgroup , union filesystem**

    [출처] https://tech.ssut.me/what-even-is-a-container/
    
    [출처] https://bluese05.tistory.com/28 

    > namespaces
    ```
    VM에서는 각 게스트 머신별로 독립적인 공간을 제공하고 서로가 충돌하지 않도록 하는 기능을 갖고 있습니다. 리눅스에서는 이와 동일한 역할을 하는 namespaces 기능을 커널에 내장하고 있습니다. 글을 쓰는 시점을 기준으로 현재 리눅스 커널에서는 다음 6가지 namespace를 지원하고 있습니다:

    - mnt (파일시스템 마운트): 호스트 파일시스템에 구애받지 않고 독립적으로 파일시스템을 마운트하거나 언마운트 가능
    - pid (프로세스): 독립적인 프로세스 공간을 할당
    - net (네트워크): namespace간에 network 충돌 방지 (중복 포트 바인딩 등)
    - ipc (SystemV IPC): 프로세스간의 독립적인 통신통로 할당
    - uts (hostname): 독립적인 hostname 할당
    - user (UID): 독립적인 사용자 할당
    ```
    
    > cgroup (Control Groups)
    ``` 
    cgroups(Control Groups)는 자원(resources)에 대한 제어를 가능하게 해주는 리눅스 커널의 기능입니다. cgroups는 다음 리소스를 제어할 수 있습니다:

    - 메모리
    - CPU
    - I/O
    - 네트워크
    - device 노드(/dev/)
    ```
    
    > union filesystem
    
    [출처] https://devaom.tistory.com/5  
    ```
    Union Mount

    복수의 파일시스템을 하나의 파일시스템으로 마운트하는 기능으로, 두 파일 시스템에서 동일한 파일이 있다면 나중에 마운트된 파일 시스템의 파일을 오버레이한다. 하위 파일시스템에 대한 쓰기 작업은 CoW(Copy On Write) 전략에 따라 복사본을 생성하여 수행하므로 원본 파일 시스템은 변하지 않는 것이 특징이다.
    
    일종의 상속inheritance와 비슷한 개념이기도 하다. 실제로 이 기능의 초기 구현은 상속 파일 시스템이라고 불리기도 했다.
  
    Union File System 종류

    Union Mount를 지원하는 파일 시스템

    UnionFS : Linux, FreeBSD, NetBSD를 위해 초기에 구현된 유니언 파일 시스템
    AUFS(Advanced Union File System) : UnionFS를 완전히 재작성하여 신뢰성과 성능을 개선하면서도 Writable Branch Balancing과 같은 새로운 개념을 도입했다.
    코드 가독성이 리눅스 오픈소스 커뮤니티에서 문제되어 주류 리눅스 커널에 통합되지 못했다.
    Overlay : 주류 리눅스 커널에 통합된 버전이다.

    최신 도커는 overlay2가 기본 값!
    ```
  
  ### **Typically said .. Container is..**
    가장 많이 보는 그림 , 정의
    ```
    가상 머신은 하드웨어 스택을 가상화 한다. 컨테이너는 이와 달리 운영체제 수준에서 가상화를 실시하여 다수의 컨테이너를 OS 커널에서 직접 구동된다. 하지만, 컨테이너는 훨씬 가볍고 운영체제 커널을 공유하며, 시동이 훨씬 빠르고 운영체제 전체 부팅보다 메모리를 훨씬 적게 차지한다. 그렇기 때문에 컨테이너는 애플리케이션을 실제 구동 환경으로부터 추상화할 수 있는 논리 패키징 메커니즘을 제공할 수 있다. 이러한 분리를 통해 사설 데이터 센터나 공용 클라우드 개발자, 심지어 개인 노트북 컴퓨터에서까지 대상 환경을 막론하고 컨테이너 기반 애플리케이션을 간단하고 균일하게 배포할 수 있다. 또한 컨테이너화를 통해 업무 영역을 깔끔하게 분리할 수 있다. 즉, 개발자는 애플리케이션의 로직과 종속 항목에 집중할 수 있고, IT 부서는 특정 소프트웨어 버전과 개별 앱 구성과 관련한 세부 업무에 시간을 낭비하지 않고 배포 및 관리에 집중할 수 있다.
    ```
    chroot 기능에서 출발, BSD 계열을 통해 발전 LXC 의 container 기술들  반짝 > docker 대부분 정복 but...
    https://www.datadoghq.com/container-report/  docker vs containerd vs CRI-O
    because of OCI
    http://www.opennaru.com/kubernetes/open-container-initiative/ 
    https://www.samsungsds.com/kr/insights/docker.html 
    https://blog.hyojun.me/5

 ### **그중에 도커를 알아 보자**

  ```
  Docker는 LXC(LinuX Container)에서 사용하는 리눅스 커널 컨테이너 기술을 이용해 만든 컨테이터 관리 유틸리티이며, Build, Ship, Run Docker는 서비스 운영환경을 묶어서 손 쉽게 배포하고 실행하는 경량컨테이너 기술이다.

  - Docker는 2013년 docker.inc에서 출시한 오픈소스 컨테이너 프로젝트
  - AWS, Google Cloud Platform, Azure 에서 공식 지원중
  ```

 ### **Docker 주요 개념 (이미지, 컨테이너 등)**
  https://velog.io/@yanghl98/%EB%8F%84%EC%BB%A4Docker 
    
  ### **도커 네트워크 (netns 기반 docker0 + veth)**
    https://bluese05.tistory.com/15
    https://bluese05.tistory.com/38 

  ### **도커 스토리지 (이미지 & 컨테이너 & 레이어)**
    https://devaom.tistory.com/5 

  ### **도커 data volume (영구 볼륨)**
    https://kin3303.tistory.com/23


## 나머지 Reference

도커에 여러 리눅스 디스트로 실행 가능 이유
https://bluese05.tistory.com/10?category=559611 

CNI?
https://captcha.tistory.com/78

container commit
https://nomad-programmer.tistory.com/305


Install docker on ubuntu
https://docs.docker.com/engine/install/ubuntu/

Docker compose & swarm
https://code-machina.github.io/2019/08/06/Difference-between-Docker-Composer-N-Swarm.html 

Pull image from dockerhub
http://aispiration.com/r-docker/04-Dockerhub.html 


- [Markdown guide](https://docs.github.com/en/get-started/writing-on-github/getting-started-with-writing-and-formatting-on-github/basic-writing-and-formatting-syntax)
- [About Git](https://docs.github.com/en/get-started/using-git/about-git)
- [Download VS code](https://code.visualstudio.com/download)

