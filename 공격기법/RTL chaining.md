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


















RTL chaining : RTL 공격에서 RET 주소를 변조하여 하나의 공유라이브를 호출했다면, RTL chaining 공격 기법은 RTL 공격 기법을 응용하여 여러 개의 공유 라이브러리 함수를 호출할 수 있다. RTL과 다른점은 가젯을 이용해야 여러 개의 공유 라이브러리 함수들을 사용할 수 있다는 것이다.


```c
-소스코드



```




```
-디버깅 작업



```


오프셋이란? : 기준점에서 얼만큼 떨어져있는 메모리의 값을 지칭할 때 쓰는 단어이다.
디버깅할 때 main의 주소가 있고 main+9의 주소가 있는데 이때 주솟값으로 표기하지 않고 
함수명 + 숫자로 표기하는 것이 오프셋이다. 

https://blog.naver.com/PostView.naver?blogId=shackerz&logNo=220473532035&redirect=Dlog&widgetTypeCall=true&directAccess=false
call 어셈블리어 의미 : push eip --> call 다음에 있는 명령줄의 주소를 Stack 영역에 저장한다는 의미이다.
                      jmp [접근할 함수 주소] --> 되돌아올 eip 주소값을 Stack에 저장했다면 접근할 함수로 이동
                      
ret 어셈블리어 의미 : pop eip --> ESP가 가르키는 값을 EIP에 저장 후 ESP가 가르키는 공간을 제거한다.
                     jmp eip --> EIP의 값을 가르키는 주소로 점프한다.
                     
                    
                    
