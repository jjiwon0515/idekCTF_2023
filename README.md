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
   
   ──(root㉿kali)-[/home/kali/Downloads/attachments]
└─# ./chall
Do you want to complete a survey?
yes
Do you like ctf?
yes
You said: yes

That's great! Can you provide some extra feedback?
yes
Do you want to complete a survey?


--> getchar 함수때문에 yes에서 y만 읽힌다. 

0x7fffffffe1fe 메모리에서 read함수에 넣은 입력값들을 확인할 수 있었다.


info func를 해보니 win함수라는게 있었다. 그래서 getchar 함수를 이용해서 getchar 함수 ret 부분에 win함수의 주소를 넣어서 전체 흐름을 변조해보려고 한다.
이것을 하기 위해서는 getchar 함수의 어셈블리어를 보면서 getchar에 넣은 값이 어느 주소값 들어가는지, ret 위치를 알아야 할 것 같다.

명령어 과정
b *getchar -> x/20i(어셈블리어 보기) 0x7ffff7e4cc92 -> 0x00007ffff7e51fff에서 입력값을 받는다(getchar함수에서 입력값 받는 역할하는 함수를 불러옴) : IO_file_jumps+20 =>IO_validate_vtable=> (x/20i 0x07ffff7e5100b 조사)여기서 si해서 함수 메모리 할당 크기보기 -> 0x7ffff7e5100b에서 다른 함수를 호출한다
 
* 0x00007ffff7e51fff에서 si ->  0x7ffff7e5100b에서 si -> 0x7ffff7ec702b에서 si -> _IO_new_file_underflow에서의 0x07ffff7e51032를 실행 후 x/x $rax를 조사하면 내가 입력한 값들이 있다(123456을 볼 수있다.) (ret영역 0x07ffff7e51043)



/jail # ./nsjail
 Usage: ./nsjail [options] -- path_to_command [args]
 Options:
  --help|-h 
        Help plz..
  --mode|-M VALUE
        Execution mode (default: 'o' [MODE_STANDALONE_ONCE]):
        l: Wait for connections on a TCP port (specified with --port) [MODE_LISTEN_TCP]
        o: Launch a single process on the console using clone/execve [MODE_STANDALONE_ONCE]
        e: Launch a single process on the console using execve [MODE_STANDALONE_EXECVE]
        r: Launch a single process on the console with clone/execve, keep doing it forever [MODE_STANDALONE_RERUN]
  --config|-C VALUE
        Configuration file in the config.proto ProtoBuf format (see configs/ directory for examples)
  --exec_file|-x VALUE
        File to exec (default: argv[0])
  --execute_fd 
        Use execveat() to execute a file-descriptor instead of executing the binary path. In such case argv[0]/exec_file denotes a file path before mount namespacing
  --chroot|-c VALUE
        Directory containing / of the jail (default: none)
  --no_pivotroot 
        When creating a mount namespace, use mount(MS_MOVE) and chroot rather than pivot_root. Usefull when pivot_root is disallowed (e.g. initramfs). Note: escapable is some configuration
  --rw 
        Mount chroot dir (/) R/W (default: R/O)
  --user|-u VALUE
        Username/uid of processes inside the jail (default: your current uid). You can also use inside_ns_uid:outside_ns_uid:count convention here. Can be specified multiple times
  --group|-g VALUE
        Groupname/gid of processes inside the jail (default: your current gid). You can also use inside_ns_gid:global_ns_gid:count convention here. Can be specified multiple times
  --hostname|-H VALUE
        UTS name (hostname) of the jail (default: 'NSJAIL')
  --cwd|-D VALUE
        Directory in the namespace the process will run (default: '/')
  --port|-p VALUE
        TCP port to bind to (enables MODE_LISTEN_TCP) (default: 0)
  --bindhost VALUE
        IP address to bind the port to (only in [MODE_LISTEN_TCP]), (default: '::')
  --max_conns VALUE
        Maximum number of connections across all IPs (only in [MODE_LISTEN_TCP]), (default: 0 (unlimited))
  --max_conns_per_ip|-i VALUE
        Maximum number of connections per one IP (only in [MODE_LISTEN_TCP]), (default: 0 (unlimited))
  --log|-l VALUE
        Log file (default: use log_fd)
  --log_fd|-L VALUE
        Log FD (default: 2)
  --time_limit|-t VALUE
        Maximum time that a jail can exist, in seconds (default: 600)
  --max_cpus VALUE
        Maximum number of CPUs a single jailed process can use (default: 0 'no limit')
  --daemon|-d 
        Daemonize after start
  --verbose|-v 
        Verbose output
  --quiet|-q 
        Log warning and more important messages only
  --really_quiet|-Q 
        Log fatal messages only
  --keep_env|-e 
        Pass all environment variables to the child process (default: all envars are cleared)
  --env|-E VALUE
        Additional environment variable (can be used multiple times). If the envar doesn't contain '=' (e.g. just the 'DISPLAY' string), the current envar value will be used
  --keep_caps 
        Don't drop any capabilities
  --cap VALUE
        Retain this capability, e.g. CAP_PTRACE (can be specified multiple times)
  --silent 
        Redirect child process' fd:0/1/2 to /dev/null
  --stderr_to_null 
        Redirect child process' fd:2 (STDERR_FILENO) to /dev/null
  --skip_setsid 
        Don't call setsid(), allows for terminal signal handling in the sandboxed process. Dangerous
  --pass_fd VALUE
        Don't close this FD before executing the child process (can be specified multiple times), by default: 0/1/2 are kept open
  --disable_no_new_privs 
        Don't set the prctl(NO_NEW_PRIVS, 1) (DANGEROUS)
  --rlimit_as VALUE
        RLIMIT_AS in MB, 'max' or 'hard' for the current hard limit, 'def' or 'soft' for the current soft limit, 'inf' for RLIM64_INFINITY (default: 4096)
  --rlimit_core VALUE
        RLIMIT_CORE in MB, 'max' or 'hard' for the current hard limit, 'def' or 'soft' for the current soft limit, 'inf' for RLIM64_INFINITY (default: 0)
  --rlimit_cpu VALUE
        RLIMIT_CPU, 'max' or 'hard' for the current hard limit, 'def' or 'soft' for the current soft limit, 'inf' for RLIM64_INFINITY (default: 600)
  --rlimit_fsize VALUE
        RLIMIT_FSIZE in MB, 'max' or 'hard' for the current hard limit, 'def' or 'soft' for the current soft limit, 'inf' for RLIM64_INFINITY (default: 1)
  --rlimit_nofile VALUE
        RLIMIT_NOFILE, 'max' or 'hard' for the current hard limit, 'def' or 'soft' for the current soft limit, 'inf' for RLIM64_INFINITY (default: 32)
  --rlimit_nproc VALUE
        RLIMIT_NPROC, 'max' or 'hard' for the current hard limit, 'def' or 'soft' for the current soft limit, 'inf' for RLIM64_INFINITY (default: 'soft')
  --rlimit_stack VALUE
        RLIMIT_STACK in MB, 'max' or 'hard' for the current hard limit, 'def' or 'soft' for the current soft limit, 'inf' for RLIM64_INFINITY (default: 'soft')
  --rlimit_memlock VALUE
        RLIMIT_MEMLOCK in KB, 'max' or 'hard' for the current hard limit, 'def' or 'soft' for the current soft limit, 'inf' for RLIM64_INFINITY (default: 'soft')
  --rlimit_rtprio VALUE
        RLIMIT_RTPRIO, 'max' or 'hard' for the current hard limit, 'def' or 'soft' for the current soft limit, 'inf' for RLIM64_INFINITY (default: 'soft')
  --rlimit_msgqueue VALUE
        RLIMIT_MSGQUEUE in bytes, 'max' or 'hard' for the current hard limit, 'def' or 'soft' for the current soft limit, 'inf' for RLIM64_INFINITY (default: 'soft')
  --disable_rlimits 
        Disable all rlimits, default to limits set by parent
  --persona_addr_compat_layout 
        personality(ADDR_COMPAT_LAYOUT)
  --persona_mmap_page_zero 
        personality(MMAP_PAGE_ZERO)
  --persona_read_implies_exec 
        personality(READ_IMPLIES_EXEC)
  --persona_addr_limit_3gb 
        personality(ADDR_LIMIT_3GB)
  --persona_addr_no_randomize 
        personality(ADDR_NO_RANDOMIZE)
  --disable_clone_newnet|-N 
        Don't use CLONE_NEWNET. Enable global networking inside the jail
  --disable_clone_newuser 
        Don't use CLONE_NEWUSER. Requires euid==0
  --disable_clone_newns 
        Don't use CLONE_NEWNS
  --disable_clone_newpid 
        Don't use CLONE_NEWPID
  --disable_clone_newipc 
        Don't use CLONE_NEWIPC
  --disable_clone_newuts 
        Don't use CLONE_NEWUTS
  --disable_clone_newcgroup 
        Don't use CLONE_NEWCGROUP. Might be required for kernel versions < 4.6
  --enable_clone_newtime 
        Use CLONE_NEWTIME. Supported with kernel versions >= 5.3
  --uid_mapping|-U VALUE
        Add a custom uid mapping of the form inside_uid:outside_uid:count. Setting this requires newuidmap (set-uid) to be present
  --gid_mapping|-G VALUE
        Add a custom gid mapping of the form inside_gid:outside_gid:count. Setting this requires newgidmap (set-uid) to be present
  --bindmount_ro|-R VALUE
        List of mountpoints to be mounted --bind (ro) inside the container. Can be specified multiple times. Supports 'source' syntax, or 'source:dest'
  --bindmount|-B VALUE
        List of mountpoints to be mounted --bind (rw) inside the container. Can be specified multiple times. Supports 'source' syntax, or 'source:dest'
  --tmpfsmount|-T VALUE
        List of mountpoints to be mounted as tmpfs (R/W) inside the container. Can be specified multiple times. Supports 'dest' syntax. Alternatively, use '-m none:dest:tmpfs:size=8388608'
  --mount|-m VALUE
        Arbitrary mount, format src:dst:fs_type:options
  --symlink|-s VALUE
        Symlink, format src:dst
  --disable_proc 
        Disable mounting procfs in the jail
  --proc_path VALUE
        Path used to mount procfs (default: '/proc')
  --proc_rw 
        Is procfs mounted as R/W (default: R/O)
  --nice_level VALUE
        Set jailed process niceness (-20 is highest -priority, 19 is lowest). By default, set to 19
  --cgroup_mem_max VALUE
        Maximum number of bytes to use in the group (default: '0' - disabled)
  --cgroup_mem_memsw_max VALUE
        Maximum number of memory+swap bytes to use (default: '0' - disabled)
  --cgroup_mem_swap_max VALUE
        Maximum number of swap bytes to use (default: '-1' - disabled)
  --cgroup_mem_mount VALUE
        Location of memory cgroup FS (default: '/sys/fs/cgroup/memory')
  --cgroup_mem_parent VALUE
        Which pre-existing memory cgroup to use as a parent (default: 'NSJAIL')
  --cgroup_pids_max VALUE
        Maximum number of pids in a cgroup (default: '0' - disabled)
  --cgroup_pids_mount VALUE
        Location of pids cgroup FS (default: '/sys/fs/cgroup/pids')
  --cgroup_pids_parent VALUE
        Which pre-existing pids cgroup to use as a parent (default: 'NSJAIL')
  --cgroup_net_cls_classid VALUE
        Class identifier of network packets in the group (default: '0' - disabled)
  --cgroup_net_cls_mount VALUE
        Location of net_cls cgroup FS (default: '/sys/fs/cgroup/net_cls')
  --cgroup_net_cls_parent VALUE
        Which pre-existing net_cls cgroup to use as a parent (default: 'NSJAIL')
  --cgroup_cpu_ms_per_sec VALUE
        Number of milliseconds of CPU time per second that the process group can use (default: '0' - no limit)
  --cgroup_cpu_mount VALUE
        Location of cpu cgroup FS (default: '/sys/fs/cgroup/cpu')
  --cgroup_cpu_parent VALUE
        Which pre-existing cpu cgroup to use as a parent (default: 'NSJAIL')
  --cgroupv2_mount VALUE
        Location of cgroupv2 directory (default: '/sys/fs/cgroup')
  --use_cgroupv2 
        Use cgroup v2
  --iface_no_lo 
        Don't bring the 'lo' interface up
  --iface_own VALUE
        Move this existing network interface into the new NET namespace. Can be specified multiple times
  --macvlan_iface|-I VALUE
        Interface which will be cloned (MACVLAN) and put inside the subprocess' namespace as 'vs'
  --macvlan_vs_ip VALUE
        IP of the 'vs' interface (e.g. "192.168.0.1")
  --macvlan_vs_nm VALUE
        Netmask of the 'vs' interface (e.g. "255.255.255.0")
  --macvlan_vs_gw VALUE
        Default GW for the 'vs' interface (e.g. "192.168.0.1")
  --macvlan_vs_ma VALUE
        MAC-address of the 'vs' interface (e.g. "ba:ad:ba:be:45:00")
  --macvlan_vs_mo VALUE
        Mode of the 'vs' interface. Can be either 'private', 'vepa', 'bridge' or 'passthru' (default: 'private')
  --disable_tsc 
        Disable rdtsc and rdtscp instructions. WARNING: To make it effective, you also need to forbid `prctl(PR_SET_TSC, PR_TSC_ENABLE, ...)` in seccomp rules! (x86 and x86_64 only). Dynamic binaries produced by GCC seem to rely on RDTSC, but static ones should work.
  --forward_signals 
        Forward fatal signals to the child process instead of always using SIKGILL.
 
 Examples: 
  Wait on a port 31337 for connections, and run /bin/sh
   nsjail -Ml --port 31337 --chroot / -- /bin/sh -i
  Re-run echo command as a sub-process
   nsjail -Mr --chroot / -- /bin/echo "ABC"
  Run echo command once only, as a sub-process
   nsjail -Mo --chroot / -- /bin/echo "ABC"
  Execute echo command directly, without a supervising process
   nsjail -Me --chroot / --disable_proc -- /bin/echo "ABC"
[E][2023-01-15T14:39:14+0000][51] setupArgv():330 No command-line provided
[F][2023-01-15T14:39:14+0000][51] main():333 Couldn't parse cmdline options
<--- nsjail을 열었을때

생각했던 솔루션 : 디버깅할 때 win이라는 함수가 있었는데 변조해서 win 함수를 보는 것이였다. (win의 주소가 0x00000000000001249였는데 0x12를 입력값에 넣지 못하고 
ret의 위치에 대한 확신이 없어서 이 방법으로는 할 수 없었다)
아마 0x12 <-> DC2라는 특수기호? (장치제어2를 의미) 이것을 할 수 없었다... 솔직히 이게 맞는 방법인지도 모르겠다

이번 CTF문제에서 여러 명령어나 도커에 대해서도 새롭게 알 수 있었다. 문제를 풀지는 못함...

해야할 것:
1) 도커에 접속을 하긴했지만 정확한 개념은 다시 정리해야겠다.
2) cgroup이란?
3) gdb로 주소 변조하는 거 연습(직접 프로그래밍 후 진행)
