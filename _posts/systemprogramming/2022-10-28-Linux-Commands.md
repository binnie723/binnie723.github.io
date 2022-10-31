---
title:  "따로 정리하는 Linux Commands" 

categories:
  - System Programming
tags:
  - [Linux, Unix, commands]

toc: true
toc_sticky: true

date: 2022-10-28
last_modified_at: 2022-10-30
---
  

<br/>   
### Basic Commands

`pwd` : 현재 디렉토리 경로

`man command` : 명령어 설명 

`whoami` :  로그인한 사용자 이름

`date` / `cal` : 지금 날짜와 시간 / 오늘 달력 

`history` : 내가 사용했던 명령어 보기 (`!줄번호`  치면 지난 명령어 다시 실행)

`clear` : 터미널 화면 지우기 

`exit` : 현재 프로세스 나가기 

  
<br/>    
### Directory commands

`mkdir dir` : 디렉토리 생성

`rmdir dir` : 디렉토리 제거 (파일 안에 없는 경우)

`rm[-rf] dir` : 디렉토리 제거 (파일 안에 있는 경우) (`rf`: recursive flag)

`cd dir`  : 디렉토리 변경

`cd .` : 현재 디렉토리로 가기       `cd ..` : 부모 디렉토리로 가기

`ls` : 디렉토리 파일 목록 보기 

`ls [-l]` : 파일형식 포함   `ls [-a]` : 숨긴파일 포함   `ls [-la]` : 둘 다 포함

  
<br/>   
### File commands

`touch file` : 파일 생성 

`vi` / `vim file` :  text editor로 파일 열기

`cat file [file2]`  : 파일 열기 (파일 합치기)

`rm file` : 파일 제거

`cp file path` : 파일 복사

`mv file path` : 파일 이동

`wc file` : 파일의 line수, word수, byte수 출력 

  
<br/>   
### File 응용 commands

`sort [-r] file` : 파일을 줄 단위로 오름차순 정렬  (-r : 내림차순)(파일을 바꾸지는 않음!)

`uniq [-c] file` : 중복되는 항목을 제거 (-c : 중복 갯수도 출력)

`cut [-d]“delimiter” [-f]num *file*` : 파일을 tokenize (-d: 자르는 기준)(-f : 몇번째 대상)

`grep [-r] "word" file` : 해당 단어를 포함하는 모든 라인 출력 (-r: recursive, 디렉토리 전체 찾기)

`find [path] -name “name”` : string을 포함하는 이름의 파일 찾기 (string에 wildcard를 지정해서 사용)

  
<br/>   
### Change permission, owner

`chmod` : permission 변경 (‘r’ = read(4)  ‘w’ = write(2)  ‘x’ = execute(1))

   -> `chmod u=rwx,g=rx,o=r file1.txt`  (= or + 모두 사용 가능, 공백 불가능)

`chown` : file owner 또는 group 변경  

   -> `sudo chown root:root file2.txt` (root로 변경하는 경우 sudo 사용!)

  
<br/>   
### Wildcard : Pattern matching

‘`*`’ : 없을 수도 있고 있을 수도 있고

   -> `ls *.txt` : 모든 txt 파일     `ls n*` : n으로 시작하는 모든 파일* 

‘?’ : 1 글자

   -> `ls *.???` *3글자 확장자를 가진 모든 파일*    `ls ?.*`  파일이름이 1글자인 모든 파일

`[ ]` : 안에 있는 문자 중 하나 (or)

   -> `ls [M-P]` : M부터 P사이 문자 중 하나로 시작하는 모든 파일

`{ }` : 안에 있는 문자 모두 포함 (and)

   -> `ls pic1.{txt, jpg, bmp}` : pic1.txt, pic1.jpg, pic1.bmp

`\` : special character를 보호하는 역할 

   -> `ls \*.txt` = `ls *.txt`


“exec” system 함수는 wildcards 인식 불가 
{: .notice--primary} 


<br/>   
### Vim commands (mode)

![image](https://user-images.githubusercontent.com/86834982/198948309-3885db44-5002-4376-be19-465272d423db.png){: width="80%" height="80%"}

![image](https://user-images.githubusercontent.com/86834982/198948305-d9303a2d-d25d-42a1-86eb-6806c5c3ee8b.png){: width="80%" height="80%"}

<br/>   
<br/>   
