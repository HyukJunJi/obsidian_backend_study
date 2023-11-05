![도커](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F9981E6375B8CF0802A)
Docker란 Go언어로 작성된 리눅스 **컨테이너 기반**으로하는 **오픈소스 가상화 플랫폼**이다.
현재 Docker 0.9버전 부터는 직접 개발한 libcontainer 컨테이너를 사용하고 있다.

### **가상화를 사용하는 이유는?**

향상된 컴퓨터의 성능을 더 효율적으로 사용하기 위해 가상화 기술이 많이 등장하였습니다.
서버 관리자 입장에서 CPU 사용률을이 10%대 밖에 되지 않는 활용도 낮은 서버들의 리소스 낭비일 수 밖에 없습니다. 그렇다고 모든 서비스를 한 서버에 안에 올린다면 안정성에 문제가 생길수도 있습니다. 그래서 안정성을 높이며 리소스도 최대한 활용할 수 있는 방법으로 나타난게 서버 가상화 입니다. 모두가 아는 대표적인 가상화 플랫폼으로는 VM이 있습니다. VM은 누구나 아는 OS 가상화입니다. 그렇다면 컨테이너란?


### 컨테이너
![사진](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F9939BC355B8D39F616)

컨테이너는 가상화 기술 중 하나로 대표적으로 LXC(Linux Container)가 있습니다. 기존 OS를 가상화 시키던 것과 달리 컨테이너는 **OS레벨의 가상화로 프로세스를 격리시켜 동작하는 방식**으로 이루어집니다.

한 서버의 여러 OS를 가상화 하여 사용하는 것과 컨테이너 방식으로 프로세스를 격리시켜 동작하는 방법은 어떠한 차이점이 있을까요?


도커는 리눅스 컨테이너 기술이므로 macOS나 windows에 설치할 경우 가상 머신이 설치가 됩니다. 
[출처](https://khj93.tistory.com/entry/Docker-Docker-%EA%B0%9C%EB%85%90)
[초보를 위한 도커 안내서 - 도커란 무엇인가? (subicura.com)](https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html)