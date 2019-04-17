# MIPS

## Os 32 Registradores de uso geral do MIPS 32 bits.
### Tabela 1
|Nome do Registrador|Número|Binário|Uso|
|--|--|--|--|
|$zero|0|000 000  |constante zero|
|$at|1|000 001|reservado para o montador|
|$v0|2|000 010|avaliação de expressão e resultados de uma função|
|$v1|3|000 011|avaliação de expressão e resultados de uma função
|$a0|4|000 100|argumento 1 (passam argumentos para as rotinas)|
|$a1|5|000 101|argumento 2|
|$a2|6|000 110|argumento 3|
|$a3|7|000 111|argumento 4|
|$t0|8|001 000|temporário (valores que não precisam ser preservados entre chamadas)|
|$t1|9|001 001|temporário|
|$t2|10|001 010|temporário|
|$t3|11|001 011|temporário|
|$t4|12|001 100|temporário|
|$t5|13|001 101|temporário|
|$t6|14|001 110|temporário|
|$t7|15|001 111|temporário|
|$s0|16|010 000|temporário salvo (valores de longa duração e devem ser preservados entre chamadas)|
|$s1|17|010 001|temporário salvo|
|$s2|18|010 010|temporário salvo|
|$s3|19|010 011|temporário salvo|
|$s4|20|010 100|temporário salvo|
|$s5|21|010 101|temporário salvo|
|$s6|22|010 110|temporário salvo|
|$s7|23|010 111|temporário salvo|
|$t8|24|011 000|temporário|
|$t9|25|011 001|temporário|
|$k0|26|011 010|reservado para o Kernel do sistema operacional|
|$k1|27|011 011|reservado para o Kernel do sistema operacional|
|$gp|28|011 100|ponteiro para área global
|$sp|29|011 101|stack pointer (aponta para o último local da pilha)|
|$fp|30|011 110|frame pointer (aponta para a primeira palavra do frame de pilha do procedimento)|
|$ra|31|011 111|endereço de retorno de uma chamada de procedimento|

## Formato de Instrução do Tipo R
### Tabela 2
|op|rs|rt|rd|shamt|funct|
|--|--|--|--|--|--|
|6 bits|5 bits|5 bits|5 bits|5 bits|6 bits|
|opcode ou código da operação|registrador do primeiro operando fonte|registrador do segundo operando fonte|registrador do operando destino	|deslocamento|função de operação ou código de função|

## Formato de Instrução do Tipo l
### Tabela 3
|opcode|rs|rt|endereço|
|--|--|--|--|
|6 bits|5 bits|5 bits|16 bits|
|código de operação|registrador destino|registrador fonte|endereço de memória|

## Opcodes
### Tabela 4

|OPCODE|DECIMAL|BINÁRIO|
|--|--|--|
|ADD|32|100 000|
|SUB|34|100 010|
|OR|36|100 100|
|AND|37|100 101|
|SLT|42|101 010|
|BNE|4|000 100|
|BEQ|5|000 101|
|J|2|000 010|
|JR|8|001 000|
|LW|35|100 011|
|SW|43|101 011|

---

# ADIÇÃO - Tipo R

**Em C** => ``a = b + c`` 

**Em MIPS** => ``ADD $t0,$s0,$s1``

* **Conversão da instrução de Linguagem de Montagem para Linguagem de Máquina**
``ADD $8,$16,$17`` 

|op|rs|rt|rd|shamt|funct|
|--|--|--|--|--|--|
0|$16|$17|$8|0|32|

* **Após a definição das posições dos registradores, basta converter cada número para binário**

|op|rs|rt|rd|shamt|funct|
|--|--|--|--|--|--|
|000000|10000|10001|01000|00000|100000|

``O código binário 00000010000100010100000000100000 é a instrução a = b + c em MIPS 32 bits``

# INSTRUÇÃO COM PARÊNTESES - Tipo R

**Em C** => ``a = b - (c - d) + e`` 

**Em MIPS** => 
*Suponha: a = $s0, b = $s1, c = $s2, d = $s3 e e = $s4*
```
SUB $t1,$s2,$s3
SUB $t1,$s1,$t1
ADD $s0,$t1,$s4
```

* **Conversão da instrução de Linguagem de Montagem para Linguagem de Máquina**

 
```
SUB $9,$18,$19
SUB $9,$17,$9
ADD $16,$9,$20
``` 

|op|rs|rt|rd|shamt|funct|
|--|--|--|--|--|--|
0|$18|$19|$9|0|32|
0|$17|$9|$9|0|32|
0|$9|$20|$16|0|32|


* **Após a definição das posições dos registradores, basta converter cada número para binário**

|op|rs|rt|rd|shamt|funct|
|--|--|--|--|--|--|
|000000|10010|10011|01001|00000|100010
|000000|10001|01001|01001|00000|100010
|000000|01001|10100|10000|00000|100000

``Código de máquina final:
00000010010100110100100000100010
00000010001010010100100000100010
00000001001101001000000000100000``

# INSTRUÇÃO COM ARRAY (Carregar Palavra) - Tipo I
Carrega em um registrador uma posição de um array

## Load Word  
``LW registrador_destino, valor (registrador_fonte)``

**Em C** => ``a = b + c[10]`` 

**Em MIPS** => *Suponha: a = $s0, b = $s1, c = $s2*
```
LW $t0, 10 ($s2)     # $t0 = memória [ $s2 + 10 ]
ADD $s0, $s1, $t0    # $s0 = $s1 + $t0
```

* **Conversão da instrução de Linguagem de Montagem para Linguagem de Máquina**

|op|rs|rt|rd|shamt|funct|
|--|--|--|--|--|--|
35|$8|$18|10|
0|$17|$8|$16|0|32|

* **Após a definição das posições dos registradores, basta converter cada número para binário**

|op|rs|rt|rd|shamt|funct|
|--|--|--|--|--|--|
|100 011|01000|10010|0000 0000 0000 1010|
|000  000|10001|01000|10000|00000|100 000|

``Código de máquina final:
10001101000100100000000000001010
00000010001010001000000000100000``

# INSTRUÇÃO COM ARRAY (Armazenar Palavra) - Tipo I
Armazena um valor em uma posição de um array

## Store Word  
``SW registrador_fonte, valor (registrador_destino)``

**Em C** => ``a[15] = b + c`` 

**Em MIPS** => *Suponha: a = $s0, b = $s1, c = $s2*
```
ADD $t0, $s1, $s2     # $t0 = $s1 + $s2
SW $t0, 15 ($s0)      # memória [ 15 + $s0 ] =  $t0
```

* **Após a definição das posições dos registradores, basta converter cada número para binário**

|op|rs|rt|rd|shamt|funct|
|--|--|--|--|--|--|
|000 000|10001|10010|01000|00000|100 000|
|101 011|01000|10000|0000 0000 0000 1111|

``Código de máquina final:
00000010001100100100000000100000
10101101000100000000000000001111``

# INSTRUÇÃO COM ARRAY (Carregar/Armazenar Palavra) - Tipo I

**Em C** => ``a[22] = b[1] - c`` 

**Em MIPS** => *Suponha: a = $s0, b = $s1, c = $s2*
```
LW $t0, 1 ($s1)       # $t0 = memória [ 1 + $s1 ]
SUB $t1,$t0,$s2       # $t1 = $t0 - $s2
SW $t1, 22 ($s0)      # memória [ 22 + $s0 ] =  $t1
```

# CONDICIONAL IF - Tipo I

**Em C** => 
```
if ( x == y ) go to L2;
a = b + c;
L2 : a = b - c;
``` 

**Em MIPS** => *Suponha: x = $s0, y = $s1, a = $s2, b = $s3, c = $s4*
```
BEQ $s0, $s1, L2              # desvia para L2 se x = y
ADD $s2, $s3, $s4             # Executa se x <> y
L2 : SUB $s2, $s3, $s4        # Executa se x = y
```
