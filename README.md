# AilabGitTutorial

# Git 이란

### 버전관리

관리 시스템(*VCS* - Version Control System)을 사용하면 파일을 이전 상태로 되돌릴 수 있고, 프로젝트를 통째로 이전 상태로 되돌릴 수 있고, 시간에 따라 수정 내용을 비교해 볼 수 있고, 누가 문제를 일으켰는지도 추적할 수 있고, 누가 언제 만들어낸 이슈인지도 알 수 있다. **VCS를 사용하면 파일을 잃어버리거나 잘못 고쳤을 때도 쉽게 복구할 수 있다.**


### 로컬 버전 관리

많은 사람은 버전을 관리하기 위해 디렉토리로 파일을 복사하는 방법을 사용한다. 하지만 작업하던 디렉토리를 지워버리거나, 실수로 파일을 잘못 고칠 수도 있고, 잘못 복사할 될 가능성이 있다.

이런 이유로 프로그래머들은 오래전에 로컬 VCS라는 걸 만들었다. 이 VCS는 아주 간단한 데이터베이스를 사용해서 파일의 변경 정보를 관리한다.
![local](https://user-images.githubusercontent.com/51704629/76032702-ce9a7080-5f7d-11ea-93fe-fe8ac4053b7b.png)

Figure1. 로컬 버전 관리.
- 컴퓨터 시스템 내 전자 방식으로 저장된 구조화된 정보 또는 데이터의 조직적 집합


### 중앙집중식 버전 관리(CVCS)

 위 문제와 프로젝트를 진행하다 보면 다른 개발자와 함께 작업해야 하는 경우 생기는 문제를 해결하기 위해 *CVCS*(중앙집중식 VCS)가 개발되었다. 이 시스템은 **파일을 관리하는 서버가 별도로 있고 클라이언트가 중앙 서버에서 파일을 받아서 사용(Checkout).**
![centeral](https://user-images.githubusercontent.com/51704629/76033107-c68f0080-5f7e-11ea-87b0-050567ba89d9.png)

Figure 2. 중앙집중식 버전 관리(CVCS).

* CVCS 환경은 로컬 VCS에 비해 장점
  * 모두 누가 무엇을 하고 있는지 알 수 있음
  * 관리자는 누가 무엇을 할지 꼼꼼하게 관리할 수 있음
  * 모든 클라이언트의 로컬 데이터베이스를 관리하는 것보다 VCS 하나를 관리하기가 훨씬 쉬움

* CVCS 환경 단점
  * 중앙 서버에 발생한 문제
    * 서버가 한 시간 동안 다운되면 그동안 아무도 다른 사람과 협업할 수 없고 사람들이 하는 일을 백업할 방법도 없음. 그리고 중앙 데이터베이스가 있는 하드디스크에 문제가 생기면 프로젝트의 모든 히스토리를 잃음.


### 분산 버전 관리 시스템
*DVCS*(분산 버전 관리 시스템)에서 **클라이언트는 단순히 파일의 마지막 스냅샷을 Checkout 하지 않고, 그냥 저장소를 히스토리와 더불어 전부 복제**. 서버에 문제가 생기면 이 복제물로 다시 작업을 시작할 수 있다. 클라이언트 중에서 아무거나 골라도 서버를 복원할 수 있다. **Clone은 모든 데이터를 가진 진정한 백업.**
![distributed](https://user-images.githubusercontent.com/51704629/76033716-3baf0580-5f80-11ea-9d9d-8329a9207051.png)

Figure 3. 분산 버전 관리 시스템(DVCS).

# Git Repository 구성


### Git 공간

Git 디렉토리는 Git이 프로젝트의 메타데이터와 객체 데이터베이스를 저장하는 곳

* Local: 컴퓨터의 저장소(ex. 내 컴퓨터)
 Local에서 작업한 내용은 서버로 push 하지 않으면 서버 저장소에 변경 내용이 반영되지 않는다.

* Remote: 서버의 저장소(ex. Github)
 Local에서 push 받아서 작업한 내용을 저장하는 공간이다.

* working directory: 우리가 지금 사용하고 있는 컴퓨터에 있는 작업 디렉토리
 각자 컴퓨터에서 어떤 파일들을 만들거나, 파일의 내용을 수정하였거나 등등 각자 어떠한 작업을 한 디렉토리

* staging area: 변경 내용을 local repo 공간에 올리기 전 임시 저장소
 깃은 파일이 추가되고, 삭제되고, 수정되는 등 변경사항을 전부 파악할 수 있다. working directory 변경사항이 있는 파일들 중 최종적으로 다음 단계인 local repo에 저장할 파일들을 설정할 수 있고, 우리가 설정한 파일들이 staging area에 임시적으로 저장한다.

* local repo: 컴퓨터의 저장소
 전 단계인 staging area의 파일들을 최종적으로 우리 컴퓨터에 저장하는 저장소(working directory와 다르다). git은 저장소를 버전으로 관리하는데 working directory에서 변경된 내용은 저장(commit. commit에 대한 자세한 내용은 뒤에서)되지 않으면 변경사항이 저장소에 반영되지 않는다.
 
![image10](https://user-images.githubusercontent.com/51704629/76034501-73b74800-5f82-11ea-902c-3a25491be604.png)


#### 세 가지 상태

Git은 파일을 Committed, Modified, Staged 이렇게 세 가지 상태로 관리
* Committed란 데이터가 로컬 데이터베이스에 안전하게 저장됐다는 것을 의미
 
* Modified는 수정한 파일을 아직 로컬 데이터베이스에 커밋하지 않은 것
 staging area, repo에 올라가지도 않은 상태
 
* Staged란 현재 수정한 파일을 곧 커밋할 것이라고 표시한 상태
 staging area에 파일이 올라간 상태

![areas](https://user-images.githubusercontent.com/51704629/76039076-b1ba6900-5f8e-11ea-8326-9402d0335dfb.png)

-------- version 사이에 편집내용 있고 이 내용이 앞선 version에 포함되지 않는다는 image 만들어서 넣을 것 --------

# 사전 작업

### Git 회원 가입
link: https://github.com/

(_Remote repository push 권한을 부여하기 위한 username 교육자에게 알려주세요_)
![collaborator](https://user-images.githubusercontent.com/51704629/76037503-92214180-5f8a-11ea-9631-ae514f0e3167.png)


### Git 설치 되어있는지 확인
```
$ cd ~
$ git --version
git version 2.25.0
```

만약 git version이 표시되지 않는다면 ```sudo apt-get install git``` 명령어를 실행하여 gitd을 설치한다.


### Clone YeongsooKim repository
link: https://github.com/YeongsooKim/AilabGitTutorial

![ys_repo](https://user-images.githubusercontent.com/51704629/76038092-145e3580-5f8c-11ea-8cf4-ead12586a17b.png)

Click the button, *Clone or download*, copy the link

```
$ cd ~
$ git clone "paste link"
```


### 사용자 정보 입력
Git을 설치하고 나면 Git의 사용 환경을 적절하게 설정해 주어야 한다. 환경 설정은 한 컴퓨터에서 한 번만 하면 된다. 설정한 내용은 Git을 업그레이드해도 유지된다. 언제든지 다시 바꿀 수 있는 명령어도 있다.

$ git config --global user.name "yeongsookim"
$ git config --global user.email "kimkys768@gmail.com"
$ git config --list
user.email=kimkys768@gmail.com
user.name=yeongsookim
 ...

