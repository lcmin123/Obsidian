$ sudo sysctl -w kernel.randomize_va_space=0

$ ls

$ gcc -m32 -fno-stack-protector -z noexecstack -o ./libcAttack ./libcAttack.c

$ gcc -fno-stack-protector -z noexecstack -o ./libcAttack ./libcAttack.c
./libcAttack.c: In function ‘vul_func’:
./libcAttack.c:13:66: warning: cast from pointer to integer of different size [-Wpointer-to-int-cast]
   13 | f("Address of buffer[] inside vul_func(): 0x%.8x\n",(unsigned)buffer);
      |                                                     ^

./libcAttack.c:14:60: warning: cast from pointer to integer of different size [-Wpointer-to-int-cast]
   14 |  printf("Frame pointer inside vul_func(): 0x%.8x\n",(unsigned)framep);
      |                                                     ^

./libcAttack.c: In function ‘main’:
./libcAttack.c:28:60: warning: cast from pointer to integer of different size [-Wpointer-to-int-cast]
   28 |  printf("Address of input[] inside main(): 0x%x\n", (unsigned)input);
      |                                                     ^

/tmp/ccx5gaQ5.s: Assembler messages:
/tmp/ccx5gaQ5.s:26: Error: unknown mnemonic `movl' -- `movl %ebp,x0'
seed@seed-virtual-machine:~/SEED_LABS$ ls
Linking.c     c_leak     hello_dynamic  mycat2  mytest
bof           c_leak.c   hello_static   myid    mytest.c
buffOvFlow.c  environ.c  libcAttack.c   myid2   sleep.c
seed@seed-virtual-machine:~/SEED_LABS$ sudo ln -sf /bin/zsh /bin/sh
seed@seed-virtual-machine:~/SEED_LABS$ sudo chown root ./libcAttack
chown: './libcAttack'에 접근할 수 없음: 그런 파일이나 디렉터리가 없습니다
seed@seed-virtual-machine:~/SEED_LABS$ sudo chown 4755 ./libcAttack
chown: './libcAttack'에 접근할 수 없음: 그런 파일이나 디렉터리가 없습니다
seed@seed-virtual-machine:~/SEED_LABS$ touch badfile
seed@seed-virtual-machine:~/SEED_LABS$ gdb breakpoint main, run
