https://d4m0n.tistory.com/84

ROP chaining이란 : ROP(Return Oriented Programming) chaining


*Exploit 실습

예시 코드


Exploit 과정 
1) write()함수로 read()함수의 실제 주소얻기
2) read()함수 - system()함수의 오프셋을 연산하여 system()함수의 실제 주소얻기
3) read()함수로 BSS영역에 "/bin/sh"문자열 저장
4) read()함수로 write()함수의 GOT에 system()함수의 실제 주소 저장
5) BSS영역의 주소를 인자, write()함수의 PLT를 호출하여 실질적으로는 system()함수 호출


https://hg2lee.tistory.com/entry/%EC%8B%9C%EC%8A%A4%ED%85%9C-Gadget-%EA%B0%80%EC%A0%AF
가젯이란? : 본래의 개념은 프로그램 상에서의 코드 조각을 지칭, 또한 프로그램 내에 존재하는 명령어 조각이다.libc파일에도 존재한다.
포너블에서의 가젯이란? : 주로 바이너리 해킹에서 가젯을 사용하는 경우 ret로 끝나는 연속된 명령어들을 의미하기도 한다.
  pop은 호출된 함수에서 인자를 정리해주는 용도이다.
pop pop ret 가젯이란 ? : 


https://kangsecu.tistory.com/147
find Gadget명령어 : 
ropsearch "pop rdi"

https://liveyourit.tistory.com/121










https://dokhakdubini.tistory.com/51
rop를 할때 중요한 과정 : gadget을 찾는 것이다. 
명령어 : ROPgadget --binary (파일명) | grep "(찾을 가젯)"
**64비트에서 인자받는 순서는 rdi,rsi,rdx 순서이다** <- 그냥 암기

만약 두 가지 상황일 경우
1. 진짜로 pop rdx가 없다 --> libc를 직접 찾아 주어야한다
2. ROPgadget으로는 조회되지 않는다. 
--> ropsearch 명령어 이용하기 

https://hg2lee.tistory.com/entry/%EC%8B%9C%EC%8A%A4%ED%85%9C-Return-Oriented-Programming-ROP-x86-32bit

