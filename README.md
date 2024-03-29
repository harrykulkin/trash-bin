# vscode-kweb-dockerfile
java / Spring 환경의 KWEB 개발을 하려고 할 때,
   기본적으로 삽질을 줄일 수 있는 매뉴얼

## 주요 내용
- 한국 로케일 설정
- 서울 타임존 / 한글 설정
- jdk 내장 / maven3 설치 (gradle은 주석 처리 / 테스트 필요)
- npm 사용 시스템을 위한 node 세팅 (IT자원신청 전용)
- tomcat8.5 기반 로컬 디버깅 (IT자원신청 제외)

## 현재는 매뉴얼만 참조할 것. 파일은 클론할 필요 없음

## 선행 사항
- 기준 환경 : Windows 10 64bit 및 __WSL2__
  - 윈도우에서 리눅스 개발환경 세팅 가능
### 가. 윈도우 최신버전 업데이트
- 윈도우 설정 -> 업데이트 확인 -> 설치 -> (요구시) 재부팅 -> 반복
  - 업데이트 더이상 안 뜰 때까지 __반복__
  - 완료 시에도 Windows10 Version __2004__ 이상이 될 때까지 반복
### 나. WSL2 및 docker for windows 설치
- 아래 블로그 참조하여 nginx 설치 직전까지 진행
  - <https://www.44bits.io/ko/post/wsl2-install-and-basic-usage>
  - 검증을 위해 nginx를 실습해봐도 좋다
  - docker run hello-world 로 검증하는 방법도 있음
### 다. Windows Terminal 편의 설정
- 설정창에서 기본 탭 WSL로 변경
  - 프로파일 순서를 바꿔서 Ubuntu를 첫번째로 해주는 것도 편하다
- 기본 디렉토리를 WSL 홈으로 변경
  - setting.json 열기 클릭
  - Ubuntu 프로파일에 다음사항 추가
  ```"startingDirectory": "\\\\wsl.localhost\\Ubuntu\\home\\유저명"```
  - <https://github.com/microsoft/terminal/issues/2849> 참조
- setting.json 예시
```
{
  ...
  "defaultProfile": "{2c4de342-38b7-51cf-b940-2309a097f518}", // 하단 Ubuntu 프로파일과 guid 일치 시켜 줌
  ...
  "profiles":
  {
      ...
      "list":
      [
          {
              "guid": "{2c4de342-38b7-51cf-b940-2309a097f518}",
              "hidden": false,
              "name": "Ubuntu",
              "source": "Windows.Terminal.Wsl",
              "startingDirectory": "//wsl$/Ubuntu/home/kang" // 기본 디렉토리 설정
          },
          {
              "guid": "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}",
              "name": "Windows PowerShell",
              ...
```
### 라. VS Code 설치 및 초기 설정
- <https://code.visualstudio.com/>

### 권장 사항 
- 한국 로케일 설정 <https://beomi.github.io/2017/07/10/Ubuntu-Locale-to-ko_KR/>
- 카카오 미러 설정
```
sudo sed -i 's/archive.ubuntu.com/mirror.kakao.com/g' /etc/apt/sources.list \
sudo sed -i 's/security.ubuntu.com/mirror.kakao.com/g' /etc/apt/sources.list \
sudo apt-get update
```
- nodejs 설치
```
sudo apt-get install nodejs
```

## 개발 환경 세팅 매뉴얼
### 가. WSL 원격 개발 환경 구동
1. 선행 사항 완료 후 Windows terminal 로 WSL 쉘(Ubuntu) 접속
2. workspace 디렉토리 생성하여 해당 위치로 이동
    - ~/workspace 추천
3. 해당 디렉토리에서 VS Code 실행 ```code .```
4. 좌측 하단 __WSL: Ubuntu__ 배너 확인되면 성공

### 나. Git 설정 및 리포지토리 클론
1. 터미널에서 아래 명령어로 유저명 및 이메일 설정 / git 비밀번호 캐싱 시간 1시간으로 설정
```
git config --global user.name "{gitea 유저명}"
git config --global user.email {gitea 가입 시 입력한 이메일}
git config --global credential.helper cache 
git config --global credential.helper 'cache --timeout=3600'
```
2. F1 커맨드 파렛트 에서 git clone 실시
  - 현재 경로는 생성한 workspace 디렉토리로
  - 터미널에서 git 쳐보고 git 안 깔려 있으면 설치 ```sudo apt-get install git```
  - URL 입력 ```https://gilab.kbs.co.kr/kwebGroup/{리포지토리명}.git```

### 다. 소스 수정 및 git 리포지토리 반영
1. 수정이 일어나면 VS Code 좌측 source control 메뉴에 뱃지가 활성화됨
2. 해당 메뉴에 들어가서 changes 목록 확인 후, 상단에 commit 메시지 입력 및 Ctrl + 엔터로 커밋
    - 경고 메시지에서 __yes__ 또는 __always__ 선택
3. 터미널에서 최종적으로 깃에 반영하기 source control 사이드바 우측 상단 확장메뉴 내 __Push__ 또는 F1 커맨드 파렛트에서 git push 입력

## 유의 사항
- Git Commit을 VS Code 로 했으면 VS Code에서 Push로 마무리 / 터미널에서 했으면 터미널에서 명령어로 마무리할 것
  - 꼬일 수 있음. 따라서 불가피한 경우가 아니면 VS Code로만 한다

## 관련 문서
- VS Code Remote WSL 매뉴얼
  - <https://code.visualstudio.com/docs/remote/wsl>
- github 매뉴얼
  - <https://docs.github.com/en>
