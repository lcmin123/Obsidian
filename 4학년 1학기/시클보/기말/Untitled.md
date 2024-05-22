$ sudo sysctl -w kernel.randomize_va_space=0

$ ls

$ gcc -m32 -fno-stack-protector -z noexecstack -o ./libcAttack ./libcAttack.c

$ gcc -fno-stack-protector -z noexecstack -o ./libcAttack ./libcAttack.c

$ ls

$ sudo ln -sf /bin/zsh /bin/sh
$ sudo chown root ./libcAttack

$ sudo chown 4755 ./libcAttack

$ touch badfile
$ gdb breakpoint main, run
