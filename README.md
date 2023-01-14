# idek-CTF-pwn

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
