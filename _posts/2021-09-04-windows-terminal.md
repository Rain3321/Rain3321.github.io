---
layout: post
title: 윈도우에 터미널 설정하기
comments: true 
categories: etc
---

회사에서는 Mac을 사용하고 집에서는 윈도우를 사용하다보니 윈도우의 커맨드가 익숙치 않아 작업이 헷갈릴 때가 있다.
그래서 이를 해결하고자 윈도우 터미널에 Git Bash를 적용하여 리눅스 명령어를 이용할 수 있도록 해보자.

# 들어가기에 앞서

---

MS에서 드디어(!) 터미널을 정식으로 출시하였다. 이를 먼저 설치해주자. 
윈도우 터미널은 Microsoft Store 에서 설치할 수 있다.

![image](https://user-images.githubusercontent.com/39397110/132098644-bea880a6-76ef-4831-acc4-d821057f9c09.png)

이를 설치하면 아래와 같은 UI의 터미널을 볼 수 있다. 이 터미널은 PowerShell과 cmd, Azure Cloud Shell 3가지가 기본적으로 들어있다.

![image](https://user-images.githubusercontent.com/39397110/132098702-3b224b3b-6151-4ae6-abe8-111cd217fe20.png)

그리고 만약 Git Bash가 아직 설치되어 있지 않다면 설치해주도록 하자 ([링크](https://gitforwindows.org/))

# Git Bash 적용하기

---

터미널을 실행해서 위에 화살표를 클릭하면 아래 사진과 같은 탭이 뜬다. 해당 탭에서 설정 탭으로 진입한다.
![image](https://user-images.githubusercontent.com/39397110/132098920-29a5893f-3483-4672-8193-b56e9cc70e68.png)

설정 탭에서 왼쪽 `새 프로필 추가 > 새 빈 프로필` 눌러 새로운 쉘을 추가한다. 이때 기입해야할 값들은 아래 사진과 같다.

![image](https://user-images.githubusercontent.com/39397110/132099625-b9a638f4-ee3a-4bdf-9dad-bc5f45963e68.png)
```
 /* 명령줄 - git이 설치되어있는 경로로 입력해주어야 한다.*/
"D:\Program Files\git\usr\bin\bash.exe" -i -l

/* 디렉터리를 시작하는 중 */
%USERPROFILE%

/* 아이콘 */
D:\Program Files\git\mingw64\share\git\git-for-windows.ico
```

이때 사진에서 `D:\Program Files` 의 경우 설정하는 PC의 git 설치 경로로 입력을 해주어야 한다.
그러면 설정 끝!

![image](https://user-images.githubusercontent.com/39397110/132099752-ab58602a-8717-4a63-b126-019e00e9284c.png)

이제 Mac 부럽지 않게 `.bash_profile` 파일을 수정하여 터미널을 취향에 맞게 설정해주면 된다.

관련된 설정은 [참고자료1](https://violetboralee.medium.com/intellij-idea%EC%99%80-git-bash-%EC%97%B0%EB%8F%99%ED%95%98%EA%B8%B0-63e8216aa7de), [참고자료2](https://violetboralee.medium.com/windows-%EC%82%AC%EC%9A%A9%EC%9E%90%EB%A5%BC-%EC%9C%84%ED%95%9C-git-bash-%EC%84%A4%EC%A0%95-ac50acb34c46)
을 확인해주시면 된다.
