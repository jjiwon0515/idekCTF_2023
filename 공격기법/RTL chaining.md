https://liveyourit.tistory.com/122 <br> 
기존의 스택구조 : buf + SFP + RET <br>
RTL을 적용한 스택구조 : buf + SFP + func1 address + dummy(func1 ret) + func1 argv[0]<br>
-> 원래 RET 자리에 system함수의 주소값이 왔으므로 eip가 func1함수의 주소가 되어 func1함수를 실행한다.<br>
-> ret 영역에 주소 변조를 하기위해서는 원하는 공유 라이브러리 함수의 주소가 필요하다 <br>
->우선 실행을 돌려야 공유 라이브러리가 실행되기때문에 실행 후 <br>
p [함수이름] 명령어를 이용하여 해당 함수의 주소를 찾아내야한다. + find "string"을 해서 원하는 문자열의 주솟값도 알아낼 수 있다.<br>
==> 위 설명이  기본적인 RTL을 이용한 취약점이다. 이것을 chaining하여 연속적으로 흐름을 제어활 수 있는 방법이 있다.<br>

RTL 실습 환경
```c
-소스코드
#include<stdio.h>+

int main(){
        char buf[100];
        read(0,buf,200);
        printf("%s\n",buf);
}

```

```asm
-디버깅 작업







```


















RTL chaining : RTL 공격에서 RET 주소를 변조하여 하나의 공유라이브를 호출했다면, RTL chaining 공격 기법은 RTL 공격 기법을 응용하여 여러 개의 공유 라이브러리 함수를 호출할 수 있다.


```c
-소스코드



```




```
-디버깅 작업



```

*RTL chaining에서 가젯(gadget)을 사용하는 이유와 사용법 : 
이유 : 위의 예시처럼 익스폴로잇하면 call_me1 함수는 잘 실행되겠지만 dummy값때문에 call_me2는 제대로 실행되지 

 * func1은 인자가 1개인 함수
 * func2는 인자가 2개인 함수
https://hg2lee.tistory.com/65 <br> 
https://rninche01.tistory.com/18 <br>
https://j4guar.tistory.com/12 <br>
https://bnzn2426.tistory.com/28 <br>


RTL(Return to Libc) 이란? : 사용자가 작성한 코드에 없는 함수를 호출하고자 메모리에 이미 적재된 공유 라이브러리의 원하는 함수를 
사용할 수 있는 기법이다. 또한, 리눅스의 메모리 보호 기법 중 리눅스의 Nx bit나 윈도우의 DEP를 우회하여 공격이 가능하다.

라이브러리 : 다른 프로그램들과 링크되기 위하여 존재하는, 하나 이상의 서브루틴이나 함수들의 집합 파일을 말하는데 함께 링크될 수 있도록 보통 미리 컴파일된 형태인 오브젝트 코드 형태로 존재한다.
-> 코드의 재사용 및 부품화 실현, 소스를 제공하지 않음으로서 중요 기술의 유출을 방지할 수 있고, 라이브러리는 사용하는 개발자들로서는 대형 어플리케이션 개발 시간을 단축시킬 수 있다는 장점들이 주어지기 때문이다. 

공유 라이브러리 : 많이 쓰는 라이브러리 종류이다. 공유 라이브러리를 사용하여 컴파일을 하면 링커가 실행파일에다가 단지 "실행될 때 우선 이 라이브러리를 로딩시킬 것" 이라는 표시를 해둔다. 그러면 실행할 때 라이브러리에 있는 컴파일된 코드를 가져와 사용한다.
정적 라이브러리 : 정적 라이브러리를 사용하여 컴파일을 하면 링커가 프로그램이 필요로 하는 부분을 라이브러리에서 찾아 실행파일 에다가 바로 복사한다. 실행파일에 다 들어가기 때문에 실행할 때 라이브러리가 필요없다. <-> 단점은 크기가 그만큼 커지고 , 같은 코드를 가진 여러 프로그램을 실행할 경우 코드가 중복이 되니 그만큼 메모리를 낭비하게 된다.


https://ii4gsp.tistory.com/71
buffer [40byte] + SFP[4byte] + system() 주소 [4byte] + dummy [4byte] + "/bin/sh"주소 [4byte]
system()함수의 주소를 넣고 dummy를 넣어주는 이유는 system()함수의 RET를 채우고 system()함수의 인자로 EBP+8의 위치에 "/bin/sh"를 넣어주기 위함이다.


https://1962-1966.tistory.com/20
앞에 코드를 cdecl 형태의 어셈블리어로 변환하면

다음과 같은 규칙을 유의합니다.

 -4개의 인자값을 push 명령어를 통해 스택에 저장합니다.

push어셈블리어를 이용해서 function 함수의 ret이전에 해당 함수의 인자값이 먼저 오도록 한다.(스택프레임 기본)
push    d
push    c
push    b
push    a
call    function
mov     ret,eax

오프셋이란? : 기준점에서 얼만큼 떨어져있는 메모리의 값을 지칭할 때 쓰는 단어이다.
디버깅할 때 main의 주소가 있고 main+9의 주소가 있는데 이때 주솟값으로 표기하지 않고 
함수명 + 숫자로 표기하는 것이 오프셋이다. 

https://blog.naver.com/PostView.naver?blogId=shackerz&logNo=220473532035&redirect=Dlog&widgetTypeCall=true&directAccess=false
call 어셈블리어 의미 : push eip --> call 다음에 있는 명령줄의 주소를 Stack 영역에 저장한다는 의미이다.
                      jmp [접근할 함수 주소] --> 되돌아올 eip 주소값을 Stack에 저장했다면 접근할 함수로 이동
                      
ret 어셈블리어 의미 : pop eip --> ESP가 가르키는 값을 EIP에 저장 후 ESP가 가르키는 공간을 제거한다.
                     jmp eip --> EIP의 값을 가르키는 주소로 점프한다.
                     
                    
                    
                    
https://blog.naver.com/PostView.naver?blogId=shackerz&logNo=220474846731&categoryNo=25&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=postView&userTopListOpen=true&userTopListCount=5&userTopListManageOpen=false&userTopListCurrentPage=1 : 함수 시작(프레임에 관한 것) --> 예시를 암기하자                    

                     
공부순서 : RTL -> RTL chaining -> GOT overwrite -> ROP -> ROP chaining
