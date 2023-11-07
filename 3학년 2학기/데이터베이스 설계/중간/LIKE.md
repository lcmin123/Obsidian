**LIKE 연산**

Wildcard를이용한문자열부분매칭

Wildcard
–% : 임의의길이의문자열(공백문자가능)
–_ : 한글자

Escape
–**ESCAPE**뒤의문자열로시작하는문자는Wildcard가아닌것으로해석

예)
–ename **LIKE ‘KOR%’** : ‘KOR’로시작하는모든문자열(KOR가능)
–ename **LIKE ‘KOR_’** : ‘KOR’다음에하나의문자가오는모든문자열
–ename **LIKE ‘KOR/%%’ ESCAPE ‘/’** :'KOR/%’로시작하는모든문자열, /시작은 제외