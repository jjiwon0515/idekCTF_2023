https://liveyourit.tistory.com/122
https://hg2lee.tistory.com/65
https://rninche01.tistory.com/18
https://j4guar.tistory.com/12
https://bnzn2426.tistory.com/28


RTL(Return to Libc) 이란? : 사용자가 작성한 코드에 없는 함수를 호출하고자 메모리에 이미 적재된 공유 라이브러리의 원하는 함수를 
사용할 수 있는 기법이다. 또한, 리눅스의 메모리 보호 기법 중 리눅스의 Nx bit나 윈도우의 DEP를 우회하여 공격이 가능하다.


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
