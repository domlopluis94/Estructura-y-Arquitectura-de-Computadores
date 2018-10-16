
## Problema 4
#### Programe en ensamblador del 88110 los fragmentos de la subrutina SUBRUTINA que se detallan a continuacion:
	SUBRUTINA (int p1, int p2, int p3)	{		int vl_a = 0; /* vl_a = 0 */		char vl_b = 0; /* vl_b = null 8 bits */		char vl_c = 45; /* vl_c = 'A' 8 bits */	/* Fragmento 1 */	....	/* Fragmento 2 */	funcion (0, p2, vl_a);	/* Fragmento 3 */	....	/* Fin */}#### a) Marco de pila: Creacion del marco de pila incluyendo la inicializacion de las variables locales.

	Subrutina: Push (R1) /*Salvar la dir de retorno al entrar en una subrutina siempre se hace esto porque sino luego no puedes regresar*/		Push(R31)		Or r31,r30,r0 ; copia r30 en r31		/*hasta aquí tengo en pila FP1,r1, p1,p2,p3 */		/*char bite , 8 puesto que son 2 palabras*/		Sub r30,r30,8		St r0,r31,-4 ; vla =0		St.b r0,r31,-5 ; vl_b=0		Add r3,r0, 45 ; copia 45 en r3		St.b r3,r31,-6		/*recordamos que store es registro que queremos guardar , dir donde lo guardamos, desplazamiento*/
#### b) Fragmento 2: Llamada a la subrutina funcion incluyendo el paso de los tres parametros por valor y en la pila. Suponga que en el Fragmento 1 se hace uso de los registros r5, r6, r7, r8, r9 y r10; que r9 y r10 contienen informacion relevante para el Fragmento 3; y que la subrutina llamante, SUBRUTINA, deja disponibles todos los registros excepto r1, r30 (SP) y r31 (FP).

	Función: Push (r9)	Push (r10)	Ld r3, r31,-4	Push (r3)	Ld r3, r31,12	Push (r3)	Push (r0)	Bsr Función;	Add r30, r30,12	POP (r10)	POP (r9)

#### c) Fin: Retorno al programa llamante, incluyendo la destruccion del marco de pila.

	Or r30,r31,0 ; destinar espacio de VL	POP(r31)	POP(r1)	Jmp r1 ; salto al retorno de subrutina


## Problema 1
#### Sea la siguiente subrutina expresada en lenguaje de alto nivel:	/* El parametro n se pasa por valor y vector por direccion */	funcion (int n, int vector[])	{	int aux[n]; /* Vector de enteros de una palabra */	int i = 0; /* Entero de una palabra inicializado a 0 */	/* Copia el vector que se pasa como parametro en	el vector de la variable local aux */	while (i < n)	{	aux [i] = vector[i];	i = i + 1;	}
	#### Programe en ensamblador del 88110 el fragmento de la subrutina funcion expuesto anteriormente.

		Push (r1)		Push (r31)		Or r31,r30,r0		Ld r3,r31,8 ; leo n		; n+1 palabras * 4 en r4		Add r4,r3,1		Mul r4,r4,4		Sub r30,r30,r4 ; guardamos espacio para los vectores locales		St r0,r31,-4 ; i = 0		Or r6,r0,r0 ; r6 -> i	While: cmp r5,r6,r3		Bb1 eq,r5,FiN		Mul r8,r6,4		Ld r20,r31,12 ; vect		Or r21, r30,r0 ; aux		Ld r2,r20,r8		St r3,r21,r8		Add r6,r6,1		St r6,r31,-4		Br While	Fin: or r30,r31,0		POP (r31)		POP(r1)		Jmp r1
		
## EJEMPLO SUB .R1,.R1,.R2 

-> 010101…1/0001/0010
C.O / R1 / R2
R1<-R1-R2

**FETCH - DECODIFICACION - FASE DE EJECUCION**
* AR<- PC -					- R1<-R1-R2* LEER MP - 
* DR <-M(AR)
* IR<-DR
* PC <-PC+1
C.O = CODIGO DE OPERACIÓN


## EJEMPLO LOAD .R1/1000
**FETCH - DECODIFICACION - FASE DE EJECUCION*** AR<-PC - - AR<-PC* LEER MP LEER MP* DR<-M(AR) DR<-M(AR)* IR<-DR LEER MP* PC<-PC+N DR<-M(AR)OJO EL PC APUNTARA A LA 2 PALABRA R1<-DR ESA SEGUNDA PALABRA HABRA QUE CARGARLA PC<-PC+N


## Ejercicio 10 de la hoja de problemasPal y Dir = 32bits (la instrucción se adapta a 32 bits), 30 registros generales (para codificar se necesitan 5 bits) , 500 instrucciones (bit que se necesitan para el c.o)¿Mínimo numero de formatos de instrucción que contenga?
	 BNZ #10[R4]	 BNZ $10 [C.O 9| DESP 23]	 ADD .R1,.R1,#4 [C.O 9| REG 5 |REG 5|INM 13]	 LD .R4, #4[.R1] [C.O 9| REG 5 |REG 5|INM 13]	 ST .R1, /2000 [C.O 9| REG 5 | XXXX 18] [DIR 32]
¿Min de formatos?3 [C.O 9 |MD 1|REG 5|DESP 17] y los de ADD y STProblema 13: Palabra y direcciones: 32 bits, modos de direccionamiento inmediato,directo a reg, indirecto a reg . Directo a byte, modelo de ejecución: reg-reg
	BR #3[++R1]	ADD .R1,#4
	MOV .R1,.RT1	ADD .RT1,#3	BR[RT1]	MOV [.R1],#7[R2--]	MOV .R2,.RT1	ADD .RT1,#7	MOV [R1],[RT1]	SUB .R2,#4	ADD .R1[.R2],#7[--R4]	SUB .R4,#4	MOV .R4,RT1	ADD .RT1,#7	LD .RT2,[.RT2]	LD .R1,[.R2]	ADD .R1,RT2	DBNZ .R1,#7[R4++]	MOV .R4,.RT2 
	ADD .RT2,#7 
	ADD .R4,#4 
	DBNZ .R1,[.RT2]

## 1 Ejercicio ”Descomponer”:  LD R1 ./1000

	Fetch : PC -> AR; (MEMRQ,RD) Mem -> DR; PC ++
	MEM(AR) -> DR; 
	DR -> RI;
	Pc +1-> PC
	Decod			1 pal				2 pal
		[ C.O 	|	R1 ]	...…. 	[	/dir	]
	PC -> AR, (MEMRQ,RD) Mem -> DR ; PC ++….. Cargo dir (2pal)
	Ejec	DR -> AR, (MEMRQ,RD) Mem -> DR; DR -> BR(R1)…… Cargo el contenido de DIR




## 2 Ejer SUB r1 r2
	Fetch : PC -> AR; (MEMRQ,RD) Mem -> DR;
	MEM(AR) -> DR; 
	DR -> RI;
	Pc +1-> PC
	Decod		
	Ejec	Br(R1)-Br(R2) -> Br(R1)
	DR -> AR, (MEMRQ,RD) Mem -> DR; DR -> BR(R1)…… Cargo el contenido de DIR

## 3 Ejercicio ST r1 , /1200 “2 palabras ocupa”
	Fetch : PC -> AR; (MEMRQ,RD) Mem -> DR; PC ++
	MEM(AR) -> DR; 
	DR -> RI;
	Pc +1-> PC
	Decod			1 pal				2 pal
		[ C.O 	|	R1 ]	...…. 	[	/dir	]
	PC -> AR, (MEMRQ,RD) Mem -> DR ; PC ++….. Cargo dir (2pal)
	Ejec	DR -> AR; r1 -> DR ; ((MEMRQ,WR)) DR -> MEM(AR)


## 4 Ejercicio jmpz /50 
	Si z  PC <- 50
	Fetch 
	Si no Z fetch 


## 5 Sea un computador con modelo de ejecución registro-registro cuyos únicos modos de direccionamiento son: inmediato, directo a registro e indirecto a registro. Programe el código equivalente a las instrucciones que se indican: 
DBNZ .R1, #7[.R4] ; Decrementa el primer operando y si el resultado no es cero salta al segundo operando. 
SWAP .R1, #7[++.R2] ; Intercambia el contenido de R1 y de la posici´on de memoria indicada por el segundo operando.

R-R 
### 1.DBNZ .R1, #7[.R4]

	R1<-R1-1; 
	act RE; 
	Si NZ PC <- R4 + 7
	SUB .R1,#1
	BZ $Fin
	NZ op1
	Mov .r4 , Rt1
	Add Rt1,#7
	BR [Rt1]
	NZ op2
	Mov R4, Rt1
	ADD RT1,#7
	DBNZ R1, [Rt1]


### 2.SWAP .R1, #7[++.R2] 
	R2 + 1 -> R2
	R1 -> M(7 + R2)
	Add .R2, #1
	Mov .r2 , Rt2
	ADD RT2 , #7
	Swap .R1 [RT2]		→ MOV .R1, .RT1
	→ LD .R1, [ RT2 ]
	ST .RT1, [ .RT2 ]



9 Dado un computador con palabras y direcciones de 32 bits, 30 registros de propósito general y 500 instrucciones. Indique el formato de las siguientes instrucciones: 

a) BR #8[.R7] 

b) ADD [.R4], #4 

c) ST .R4, /2000 

d) LD.R4,#4[.R7]

Co 9 R 5 codigo 18			dirm 32 

Co 9 R 5 R/ 5 Desp 13

CO 9 desp 23

##Ejer 1  En un computador con palabras y direcciones de 16 bits, con direccionamiento a nivel de palabra, se ejecuta el siguiente fragmento de código en el que el tamaño de cada instrucción se indica como comentario. Considere que a partir de la dirección de memoria H’1008 se encuentran almacenados respectivamente los siguientes datos: H’0001, H’0002, H’0003, H’0004 y H’0005. Indique los valores sucesivos que toman los registros y posiciones de memoria afectados por la ejecución. 

	LD .R1, #100C ; 2 palabras 			R1 -> 100C      
	LD .R2, #1008 ; 2 palabras 			R2 -> 1008
	SUB .R1, [.R2--] ; 1 palabra 			R1 -> 100B, R2 -> 1007 (Dir a palabra -1)(Si fuera a byte serian -2)
	DEC .R1 ; 1 palabra 				R1 -> 100A 
	ST .R1, #4[++.R2] ; 2 palabras 		R2 -> 1008, (H´100C	    |	H´100A)
	CMP .R1, .R2 ; 1 palabra 			ACT RE
	BNZ $-6 ; 1 palabra  salta 			salta a 2* 
	MOVE [++.R2], [.R1++] ; 1 palabra 		(H´1008     |	H´0002) |R1 → 1009
	HALT ; 1 palabra *2 loop del 7
	LD .R2, #1008 ; 2 palabras 			R2 -> 1008
	SUB .R1, [.R2--] ; 1 palabra 			R1 -> 1009, R2 -> 1007 (Dir a palabra -1)(Si fuera a byte serian -2)
	DEC .R1 ; 1 palabra 				R1 -> 1008 
	ST .R1, #4[++.R2] ; 2 palabras 		R2 -> 1008, (H´100C	    |	H´1008)
	CMP .R1, .R2 ; 1 palabra 			ACT RE
	BNZ $-6 ; 1 palabra  salta 			salta a 2* 
	
*memoria* 	

	INI
	H´1008		H´0001	
	H´1009		H´0002
	H´100A		H´0003
	H´100B		H´0004
	H´100C		H´0005
	FIN
	H´1008		H´0002	
	H´1009		H´0002
	H´100A		H´0003
	H´100B		H´0004
	H´100C		H´1008


## 2 Sea un computador con palabras y direcciones de 32 bits, direccionamiento a nivel de byte y ordenamiento little-endian que ejecuta el siguiente fragmento de código en el que todas las instrucciones ocupan una palabra. Las direcciones de memoria 0 a 7 contienen los siguientes valores hexadecimales respectivamente: EF, FE, 00, 00, DF, FD, 00, 00. 

	LD .R2, #H’0008 
	LD .R4, [--.R2] 
	CMP .R4, #0 
	BZ $8 
	CMP .R2, #0 
	BNZ $-20 
	ADD [.R2--], .R4 
	HALT 

Indique razonadamente los valores sucesivos que toman los registros y posiciones de memoria que se modifican durante la ejecución de las instrucciones.

* Memoria 

0     |	EF

1     |	FE

2     |	00

3     |	00

4     |	DF

5     |	FD

6     |	00

7     |	00

Del 0 al 3 primera palabra , del 4 al 7 la segunda
M(0000) -> 00 00 FE EF

	LD .R2, #H’0008 
	LD .R4, [--.R2] 	R2→ 0004, 	R4 → 0000FDDF | R2→ 0004, 	R4 → 0000FEEF	
	CMP .R4, #0 		
	BZ $8 
	CMP .R2, #0 
	BNZ $-20		Salto a 2 
	ADD [.R2--], .R4 	R2→ 0000FEEF + R4 → 0000FEEI  = que dios nos pille confesados 0001 FDDE
	HALT 
