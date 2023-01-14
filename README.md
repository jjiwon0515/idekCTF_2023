# idek-CTF-pwn

https://ctf.idek.team/login?token=UwaWxM9tCbdQvWbITImXLU34R4nIfZxYWbt8D8U%2BP7g%2Bv3loyhg3EY62E%2FRbOuTo%2Fecvq2InuS3DhqNSpxoY6KwcnWLTWMMTuaXtkByDa7QhchOFwQcYqcbZ16dI


1) Typop
dockerfile이란? : 이미지를 생성하기 위한 용도로 작성하는 파일이다.
지시어 : 각 라인 맨앞에 있는 대문자들
양식 : FROM   pwn.red/jail
       COPY   --from=unbuntu:20.04/ /srv
       RUN    mkdir /srv/app                  --> /srv/app 폴더를 만든다.
       ADD    chall /srv/app/run              --> 호스트에 있는 chall파일을 /srv/app/run 폴더 안에 추가      
       ADD    flag.txt  /srv/app/flag.txt     --> 호스트에 있는 flag.txt파일을 /srv/app/flag.txt 폴더 안에 추가
       RUN    chmod +x /srv/app/run           --> 
       
       1) FROM : 베이스 이미지와 태그를 지정하면 레지스트리(이미지를 저장하는 저장소)에서 해당 이미지를 땡겨온다.
       2) COPY : 파일과 디렉토리에 대한 소유 권한을 지정한다. 또는 파일 또는 디렉토리를 컨테이너의 파일 시스템으로 복사
       3) RUN : 명령어를 실행하여 새 이미지에 포함시키는 역할을 한다.
       4) ADD : COPY와 거의 비슷하다.
       
       
 *Dockerfile 빌드 명령어 : docker build -t mybuild:0.0 ./
 *Dockerfile 실행 명령어 : docker run -d -p 80:80 --name myserver mybuild:0.0
 
 
 ```asm
 => 0x0000555555555410 <+0>:     endbr64 
   0x0000555555555414 <+4>:     push   rbp
   0x0000555555555415 <+5>:     mov    rbp,rsp
   0x0000555555555418 <+8>:     mov    rax,QWORD PTR [rip+0x2bf1]        # 0x555555558010 <stdout@@GLIBC_2.2.5>
   0x000055555555541f <+15>:    mov    ecx,0x0
   0x0000555555555424 <+20>:    mov    edx,0x2
   0x0000555555555429 <+25>:    mov    esi,0x0
   0x000055555555542e <+30>:    mov    rdi,rax
   0x0000555555555431 <+33>:    call   0x555555555130 <setvbuf@plt>
   0x0000555555555436 <+38>:    jmp    0x555555555447 <main+55>
   0x0000555555555438 <+40>:    call   0x555555555120 <getchar@plt>
   0x000055555555543d <+45>:    mov    eax,0x0
   0x0000555555555442 <+50>:    call   0x555555555348 <getFeedback>
   0x0000555555555447 <+55>:    lea    rdi,[rip+0xc3a]        # 0x555555556088
   0x000055555555544e <+62>:    call   0x5555555550d0 <puts@plt>
   0x0000555555555453 <+67>:    test   eax,eax
   0x0000555555555455 <+69>:    je     0x555555555461 <main+81>
   0x0000555555555457 <+71>:    call   0x555555555120 <getchar@plt>
   0x000055555555545c <+76>:    cmp    eax,0x79
   0x000055555555545f <+79>:    je     0x555555555438 <main+40>
   0x0000555555555461 <+81>:    mov    eax,0x0
   0x0000555555555466 <+86>:    pop    rbp
   0x0000555555555467 <+87>:    ret    

실행파일을 디버깅했을 때는 이상한 점은 없었다....
아마 도커파일로 서버에 접속해야하는 것 같다

docker build -t fin:latest ./ 

docker images

docker run -it --privileged fin

docker ps -a : 실행중인 도커를 볼 수 있다.

docker exec 이름 fin /bin/bash or /bin/sh --> *bash 쉘이 아닐 경우 sh쉘로 생각하고 명령어를 넣어보자

sh쉘이였고 쉘 환경에 들어올 수 있었다. 그리고 /jail 폴더안의 run 파일을 실행하니까 아래와 같은 에러 메시지가 뜬다.
자꾸 mount cgroup2 to /jail/cgroup/unified : device or resource busy 오류가 뜬다

무엇으로 flag를 구해야할지 감이 잘 오지않는다. 우선 쉘 안에 들어왔지만 


```c
   0x0000555555555348 <+0>:     endbr64 
   0x000055555555534c <+4>:     push   rbp
   0x000055555555534d <+5>:     mov    rbp,rsp
   0x0000555555555350 <+8>:     sub    rsp,0x20
   0x0000555555555354 <+12>:    mov    rax,QWORD PTR fs:0x28
   0x000055555555535d <+21>:    mov    QWORD PTR [rbp-0x8],rax
   0x0000555555555361 <+25>:    xor    eax,eax
   0x0000555555555363 <+27>:    mov    QWORD PTR [rbp-0x12],0x0
   0x000055555555536b <+35>:    mov    WORD PTR [rbp-0xa],0x0
   0x0000555555555371 <+41>:    lea    rdi,[rip+0xcab]        # 0x555555556023
   0x0000555555555378 <+48>:    call   0x5555555550d0 <puts@plt>
   0x000055555555537d <+53>:    lea    rax,[rbp-0x12]
   0x0000555555555381 <+57>:    mov    edx,0x1e
   0x0000555555555386 <+62>:    mov    rsi,rax
   0x0000555555555389 <+65>:    mov    edi,0x0
   0x000055555555538e <+70>:    call   0x555555555100 <read@plt>
   0x0000555555555393 <+75>:    lea    rax,[rbp-0x12]
   0x0000555555555397 <+79>:    mov    rsi,rax
   0x000055555555539a <+82>:    lea    rdi,[rip+0xc93]        # 0x555555556034
   0x00005555555553a1 <+89>:    mov    eax,0x0
   0x00005555555553a6 <+94>:    call   0x5555555550f0 <printf@plt>
   0x00005555555553ab <+99>:    movzx  eax,BYTE PTR [rbp-0x12]
   0x00005555555553af <+103>:   cmp    al,0x79
   0x00005555555553b1 <+105>:   jne    0x5555555553c6 <getFeedback+126>
   0x00005555555553b3 <+107>:   lea    rdi,[rip+0xc88]        # 0x555555556042
   0x00005555555553ba <+114>:   mov    eax,0x0
   0x00005555555553bf <+119>:   call   0x5555555550f0 <printf@plt>
   0x00005555555553c4 <+124>:   jmp    0x5555555553d7 <getFeedback+143>
   0x00005555555553c6 <+126>:   lea    rdi,[rip+0xc84]        # 0x555555556051
   0x00005555555553cd <+133>:   mov    eax,0x0
   0x00005555555553d2 <+138>:   call   0x5555555550f0 <printf@plt>
   0x00005555555553d7 <+143>:   lea    rdi,[rip+0xc82]        # 0x555555556060
   0x00005555555553de <+150>:   call   0x5555555550d0 <puts@plt>
   0x00005555555553e3 <+155>:   lea    rax,[rbp-0x12]
   0x00005555555553e7 <+159>:   mov    edx,0x5a
   0x00005555555553ec <+164>:   mov    rsi,rax
   0x00005555555553ef <+167>:   mov    edi,0x0
   0x00005555555553f4 <+172>:   call   0x555555555100 <read@plt>
   0x00005555555553f9 <+177>:   nop
   0x00005555555553fa <+178>:   mov    rax,QWORD PTR [rbp-0x8]
   0x00005555555553fe <+182>:   xor    rax,QWORD PTR fs:0x28
   0x0000555555555407 <+191>:   je     0x55555555540e <getFeedback+198>
   0x0000555555555409 <+193>:   call   0x5555555550e0 <__stack_chk_fail@plt>
   0x000055555555540e <+198>:   leave  
   0x000055555555540f <+199>:   ret    
   
   문제에서 JW가 피드백을 쓰는 도중 오타를 발생했고 그래도 정상적으로 진행됐다고 한다. 이 어셈블리어 영역을 봐야할 것 같다.
