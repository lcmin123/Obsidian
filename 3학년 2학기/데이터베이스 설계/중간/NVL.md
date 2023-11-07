
NVL(expr1, expr2)

–expr1이NULL이면expr2를출력한다

–데이터타입이호환가능하여야함

–e.g., SELECT sal, comm, **(sal*12)+NVL(comm,0)** FROM emp;