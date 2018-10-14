
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

