---
title:  "Shell Programming" 

categories:
  - System Programming
tags:
  - [Linux, Unix, shell, programming]

toc: true
toc_sticky: true

date: 2022-10-31
last_modified_at: 2022-10-31
---

<br/>   
### What is Shell, Shell Script

- **Shell** : kernel 과 user 사이의 인터페이스
- **Shell Script** : shell command들의 집합, collection
    
    -> task 자동화, 사용하기 쉬움, portable, transparency 
    
<br/> 
### Running Shell Script

 **Hashbang #!** : 맨 위쪽 상단에 **어떤 쉘을 사용할 것인지** 정의
    
    -> bash shell 사용 : `#!/bin/bash`,  sh shell 사용 : `#!/bin/sh` 
    
 Shell Script 실행 방법
  - **권한 부여**하고 실행 :  `chmod a+x scriptname` 후 `./scriptname`
  - **새로운 shell**에서 실행 : `bash scriptname`
  - **현재 shell**에서 실행 : `source scriptname` , `. scriptname`
        
    -> shell에서 정의한 변수 값들이 그대로 남아있게 된다
        
    
    ex. bash, source, ./ 명령어의 차이점
    

![image](https://user-images.githubusercontent.com/86834982/198943718-e0720717-08dc-42ae-8aea-15a5e801e5fd.png){: width="80%" height="80%"}

- 간단한 Shell Script 실행 : Hello World! 출력하기
    
    ```bash
    #!/bin/bash          
    echo "Hello World!"  # echo : 입력 문자열을 터미널에 출력하는 명령어
    ```
      
<br/> 
### Shell Variables

- **Environment variable** : 모든 shell에 값을 유지, export 명령어로 생성
    
    -> 주로 프로그램에 대한 정보를 제공할 때 사용, 프로그램 실행시 환경 변수 설정 
    
    -> **`env`, `printenv`** 명령어로 현재 저장된 환경변수 목록 확인
    
- **Shell variable** : 해당 shell 안에서만 값을 유지
    - Global variable : shell script 내에 모든 줄에서 값을 유지
    - Local variable : shell script 내에 선언된 함수 안에서만 값을 유지

- **Set variable** 변수 지정하기
    
    ```bash
    # set environment(global) variable 
    export global_var=2
    global_var=2; export global_var
    
    # set shell(local) variable
    shell_var=3
    local shell_local_var=4
    
    # print variables in terminal
    echo "global_var : $global_var"             # global_var : 2       
    echo "local_var : $local_var"               # local_var : 3
    echo "shell_local_var : $shell_local_var"   # local_var : 4
    ```
    
- **Unset variable** 변수 삭제하기 : `unset variablename`
- **Variable naming** : letter나 _로 시작, 대소문자 구분, 숫자 사용 가능(맨 앞 제외), special character 사용 불가
- `$PATH` : 명령어를 실행할 때, shell은 PATH에 등록된 경로에 있는 디렉토리에 해당 프로그램이 있는지 검색
    
    -> 자주 실행하는 프로그램을 저장해놓는 편이 좋음, ex. `PATH=$PATH:/home/binnie/myfolder`
    
<br/> 
### Arrays

Bash 쉘은 배열 자료구조를 지원하고 있다.

**Array 사용 방법** 

```bash
# Create list
A=(abc 1 123 xyz)   # ,가 아닌 띄어쓰기로 구분

# Set element 
A[3]=zyx

# View element of array
echo $A          # just first element 
echo ${A[3]}     # third index element
echo ${A[@]}     # all elements 
echo ${#A[@]}    # number of elements

# Get input in array
echo -n "Insert number: "  # -n : no newline!
read -a arr     
```

<br/> 
### Quotation

- Single Qutataton **`‘‘`** : **literally** 문자열을 출력  → ‘$myvar’
- Double quotation **`“”`** : **semantically** 문자열을 출력  → “hello”
- Back Quotation **````** : **command substitution**, `$( )`와 같은 기능
    
    -> `type -a`, `command -V` : 사용할 수 있는 built-in commands 확인
    

  ex. 각 quotation별 결과 확인 

![image](https://user-images.githubusercontent.com/86834982/198943733-fc8f9d28-a261-4dec-96b3-2892dddb441a.png){: width="80%" height="80%"}

<br/> 
### Boolean, Arithmetic Operations

- Boolean 연산 : **0 (True), 1 (False)**
    
    -> other language : 1 (True), 0 (False) 헷갈리지 말기 !
    
- Arithmetic 연산
    
    -> integer expansion : `$[ ]`, `$(( ))` , `$(expr)`, ``expr`` 
    

```bash
# 수학 연산할 때 사용 가능한 syntax들 정리  

$((num3=num1+num2))   # 3 (앞에 $ 생략 가능)
$((num4=$num1+$num2)) # 3 (앞에 $ 생략 가능)
num5=$((num1+num2))   # 3
num6=$(($num1+$num2)) # 3

num7=$[num1 + num2]   # 3
num8=$[$num1 + $num2] # 3
$[num9=num1+num2]    # 3
$[num10=$num1+$num2]   # 3

let num11=$num1+$num2  # 3
num12=`expr $num1 + $num2` # 3 (띄어쓰기 중요)
num13=$(expr $num1 + $num2) # 3 (띄어쓰기 중요)
```

<br/> 
### Redirection and Here document

- **Redirection** > : standard input이나 standard output을 바꿔준다
    
    `sort array.txt > array2.txt` : 출력 스트림을 파일로 설정
    
- **Here document** << : tag가 들어올 때까지 넣은 입력이 입력 스트림에 들어간다
    
    ```bash
    cat << EOF
    > multi-line text
    > can now be inserted here.
    > EOF   # 여기까지 입력으로 같이 들어가게 됨
    
    # multi-line text
    # can now be inserted here.
    ```
    
    - EOF : End of File의 약자로 **파일 종료 코드**를 의미, 리눅스에서는 ctrl+D

---

<br/> 
### Flow Control : **if condition**

- **if - elif - else 조건문** 사용하기
    
    ```bash
    if [ condition ]    # [ ] 안에 띄어쓰기 중요
    # if [ condition ]; then도 가능
    then
        command-list
    
    elif [ condition2 ] # [[ ]]도 사용 가능 
    then
        command-list
    
    else
        command-list
    fi
    ```

    

<br/> 
### Operations for Conditions

- **test** Expression : 리턴 값이 **true인지 false인지** 판별
    
    = `[ Expression ]` = `[[ Expression ]]`  같은 의미지만, [[ ]] 가 더 범용적
    

- Special arguments
    
    
    | $# | argument 갯수 |  |
    | --- | --- | --- |
    | $0 | 현재 shell script 이름 |  |
    | $1 ,$2 | positional parameters
    직접 입력 받는 parameter들 | C의 argv[0], argv[1]와 유사 |
    | $*  | 모든 parameters 전부         | 하나의 문자열로 출력 |
    | $@ | 모든 parameters 전부               | 각각 문자열로 출력 |
    | $? | 최근에 실행한 명령어 값 |  |
    | $$ | 현재 프로세스 pid |  |

- Logical Operations : **!  not**,  [[  **&&  and**,  **||  or**  ]],   **-a  and**,  **-o  or**
- Relational Operations
    
    
    | Meaning | Numeric  | String |
    | --- | --- | --- |
    | greater than (str1) | -gt | str1 > str2 |
    | greater than + equal | -ge |  |
    | less than (str1) | -lt | str1 < str2 |
    | less than + equal | -le |  |
    | equal | -eq | = or == |
    | not equal | -ne | != |
    | strlen > 0 |  | -n str |
    | strlen = 0 |  | -z str |
    
    ex. 입력 받은 2개의 argument를 출력하는 shell script
    

![image](https://user-images.githubusercontent.com/86834982/198943753-671fbb18-0a4a-4459-9b54-8b56dc0ee61d.png){: width="80%" height="80%"}

- File testing : 입력 받은 **파일의 종류** 확인 (True 또는 False 반환)
    - ***-e** file* : 파일이 존재하는지
    - ***-r -w -x** file*  : 파일 읽기, 쓰기, 실행 가능한지
    - ***-l** file* : 파일이 존재하고 symbolic link인지
    - ***-o** file* : user가 파일을 own하고 있는지
    - ***-f -d** file* : 파일이 regular file, directory file인지
    - ***-s** **-z** file* : 파일의 크기가 0보다 큰지, 0인지

  
  <br/> 
### Flow Control : For, While, Until loop

- **for 반복문** 사용하기
    
    ```bash
    for [ condition ]
    # for [ condition ]; do 도 가능 
    do
        command-list
    done 
    ```
    
    ex. for 문으로 array와 list의 element를 하나씩 불러와서 출력 
    
    ![image](https://user-images.githubusercontent.com/86834982/198944440-0094da46-b807-48cd-befc-9f224efb25e1.png){: width="80%" height="80%"}
    

- **while 반복문** 사용하기
    
    ```bash
    while [ condition ]
    # while [ condition ]; do도 가능 
    do
        command-list
    done 
    ```
    
    ex. 입력받은 파일을 while read line로 한 줄씩 불러와서 출력 
    
    ![image](https://user-images.githubusercontent.com/86834982/198944443-b7e2a112-f243-419f-af79-f7dab0b7c847.png){: width="80%" height="80%"}
    

- **until 반복문** 사용하기
    
    ```bash
    until [ condition ]
    # until [ condition ]; do도 가능 
    do
        command-list
    done 
    ```
    
<br/> 
### Operations for loop

- **`shift** n` : n만큼 뒤로 이동시키는 명령어, default n은 1로 설정됨
    
    → 작동 방식 : $2 → $1, 기존의 $1 argument는 삭제
    
    ex.  4, 5, 6을 입력하고 shift 1을 실행시킨 결과 
    
    ![image](https://user-images.githubusercontent.com/86834982/198944450-cce4798f-0735-4e05-b023-6dbcbd38c1b6.png){: width="80%" height="80%"}
    

- **break, continue** : iteration을 멈추고 나가거나, 멈추고 다음으로 넘어가는 구문

![image](https://user-images.githubusercontent.com/86834982/198944529-c93cd415-0a99-43df-b70b-cc209f674d8c.png){: width="80%" height="80%"}

    continue문 : 다음 iteration으로 이동

![image](https://user-images.githubusercontent.com/86834982/198944529-c93cd415-0a99-43df-b70b-cc209f674d8c.png){: width="80%" height="80%"}

    break문 : 조건문 완전히 탈출 

    
<br/> 
### Flow Control : case, select

- **case 조건문** 사용하기
    
    str과 일치하는 블럭이 있을 때, 해당 command list를 수행한다
    
    ```bash
    case str in  
        pattern_1)  
            commandlist1 
            ;;  
        pattern_2)  
            commandlist2 
            ;;  
        pattern_3|pattern_4|pattern_5) # 다수의 조건을 하나의 패턴에 적용
            commandlist3 
            ;;  
        *)  
            command-list4    # default, 조건에 일치하지 않는 항목  
            ;;
    esac
    ```
    
    ex.  case문 : 사용자로부터 입력 받은 이름이 올바른지 확인하는 예제
    
    ![image](https://user-images.githubusercontent.com/86834982/198944649-8f3efd6b-0cbb-4279-81f5-a8eaec0f9f4d.png){: width="80%" height="80%"}
    

- **select 조건문** 사용하기
    
    wordlist 하나를 선택할 수 있는 옵션을 주고, 선택한 것을 적용한 command 수행
    
    → 조건문을 나갈 때까지 **loop가 반복**된다 (^C or ^D)
    
    ```bash
    PS3="input message: "  #select 명령어에서 제공하는 프롬프트 메시지
    select word in wordlist
    do
    	commands 
    done
    ```
    
    ex.  select 문 : 사용자가 선택한 파일을 private 파일로 권한 변경 
    
    ![image](https://user-images.githubusercontent.com/86834982/198944656-c059b324-eaab-4274-89cf-b9716ee110a7.png){: width="80%" height="80%"}
    

<br/> 
### Shell Function

shell script 내에서 **함수 선언**하기

→ 참조되기 전에 선언되어야 함, 주로 shell script 시작 부분에 위치

```bash
function function_name {  # function keyword 사용하는 경우
		statements
}

function_name () {         # keyword 대신 bracket 사용하는 경우 
		statements
}
```

- **함수 실행하기** : `function_name arg1 arg2`
- **함수 삭제하기** : `unset -f function_name`
    
    ex. 함수에 multiple argument를 입력해서 실행하는 예제
    
    ![image](https://user-images.githubusercontent.com/86834982/198944761-da0fbc37-4029-463a-b73e-ededd0ab16eb.png){: width="80%" height="80%"}
    

- **함수 return 값 반환하기** : `**return** [n]` (n : 0-255)
    - 정수 외에 다른 값 리턴 : **standard output(echo)** 또는 **global variable** 사용
    - 리턴 값을 받아오기 : `**$?**` 또는 `var=$(function)` 사용
    
    ex.  함수에서 **$?** 를 사용해서 multiple return을 받아올 때 
    
    ![image](https://user-images.githubusercontent.com/86834982/198944683-9ad135ee-1f8a-439b-b019-31b33fcbe935.png){: width="80%" height="80%"}
    

- **Recursion** **재귀 함수** : **자기 자신을 호출**하는 경우 (no limit)
    
    최대한 재귀 함수를 사용하지 않는 편이 좋음 
    
    → 속도가 매우 느리고, call stack을 많이 잡아먹음 → stack overflow도 발생 가능 
    
    ex. recursion 종료 조건의 필요성 
    
    ![image](https://user-images.githubusercontent.com/86834982/198944822-98707b79-4e0f-40a0-9e20-bbe79bbd933e.png){: width="80%" height="80%"}
    
<br/> 
### Handling Signals : Trap

UNIX에서 특정 signal을 통해 프로세스를 멈추게 하는 것이 가능하다

`kill -signal pid` : 프로세스에 특정 signal을 보내기

- -1 : hangup
- -2 : interrupt with ^C
- -9 : kill
- no argument : terminate

- **Trap 설정**하기 : `trap ‘handler commands’ signals`
    
    1, 2 등의 signal을 받아도 terminate 하지 않고 특정 명령어를 대신 수행 
    
- **Trap 리셋**하기 : `trap signals`
    
    수행할 command 지정하지 않으면 원래의 default handler로 돌아감 
    

ex.  trap을 설정하고 signal을 부여했을 때 나타나는 결과 

![image](https://user-images.githubusercontent.com/86834982/198944845-fd7cb543-ab96-4878-a941-49a7f3080431.png){: width="80%" height="80%"}

→  다른 터미널에서 signal을 발생시킨 결과 : **terminate 하지 않음** 

![image](https://user-images.githubusercontent.com/86834982/198944847-6581dacc-0506-4393-98fb-a3752a5284ac.png){: width="80%" height="80%"}

<br/> 
### Debugging Shell Script

shell script를 디버깅하는데 사용되는 명령어

- `echo` 를 사용한 debugging : *printf* 와 유사하게 값을 직접 출력해보면서 확인
- `set` 을 사용한 debugging
    - *-v* : read line할 때마다 값을 출력
    - *+x* : command랑 argument 출력
    - *-x* : statement 출력하는 것 중단
    - *-n* : syntax error 찾아줌
    
    ex : `set -xv` 또는 
    
    ```bash
    #!/bin/bash -xv  
    # -> shell script 전체를 디버깅 모드로 설정 
    ```
    
<br/> 
@Advanced Programming in the UNIX environment, Third edition 내용을 참고함
  

<br/>   
