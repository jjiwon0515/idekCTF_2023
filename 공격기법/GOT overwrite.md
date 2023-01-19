https://ii4gsp.tistory.com/73
https://velog.io/@silvergun8291/GOT-Overwrite


GOT overwrite 공격기법이란 : 다이나믹 링크방식으로 컴파일된 바이너리가 공유 라이브러리를 호출할 때 사용되는 PLT & GOT를 이용하는 공격 기법이다.
PLT는 GOT를 참조하고, GOT에는 함수의 실제 주소가 들어있는데, 이 GOT의 값을 원하는 함수의 실제 주소로 변조시킨다면, 
원래의 함수가 아닌 변조한 함수가 호출된다. 
Ex) printf("/bin/sh"); -> "/bin/sh"라는 문자열을 출력하는 함수가 있다면 여기서 printf함수를 system함수로 변조시켜
system("/bin/sh");로 하여 쉘을 실행시킬 수 있다.
