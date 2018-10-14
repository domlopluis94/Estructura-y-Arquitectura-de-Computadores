# Conocimientos básicos de un computador 

Primero tener claras las unidades con las que vamos a trabajar, sera imprescindible tambien saber trabajar en la transformacion de decimal a binario (base 2) o hexadeciaml (base 16)
 
![ Lo primero recordar bien las unidades que vamos a emplear ](http://revista.jovenclub.cu/wp-content/uploads/2016/03/Tabla-2.jpg)

![ Lo primero recordar bien las unidades que vamos a emplear ](https://wiki.brazilfw.com.br/lib/exe/fetch.php?media=wiki:bit-byte-01.png)

[Aprender a pasar de Decimal a Hexadecimal ](https://es.wikihow.com/convertir-de-decimal-a-hexadecimal)

![ Lo primero recordar bien las unidades que vamos a emplear ](http://image.slidesharecdn.com/presentacionconceptosbasicoscomunicaciones-130306103755-phpapp02/95/presentacion-conceptos-basicos-comunicaciones-12-638.jpg?cb=1362566644)

![ Lo primero recordar bien las unidades que vamos a emplear ]( https://image.slidesharecdn.com/benchmark-osdctw-2014-140411002848-phpapp02/95/understanding-android-benchmarks-15-638.jpg?cb=1397176443 )

![ Lo primero recordar bien las unidades que vamos a emplear ]( https://vignette.wikia.nocookie.net/fisicaingindustrial/images/3/35/Prefijos_SI.jpg/revision/latest?cb=20120219165651&path-prefix=es )

## Partes del computador 
![ Lo primero recordar bien las unidades que vamos a emplear ](https://upload.wikimedia.org/wikipedia/commons/5/50/Arquitecturaneumann.jpg)

Como podemos ver el computador se compone principalmente de la CPU que se compone de una ALU "unidad aritmética lógica",La unidad de control y los registros.  Luego tenemos la MP "memoria principal" y el sistema de E/S.


### ALU: 

Realiza las operaciones tanto aritméticas como lógicas. Para facilitar caso con acarreo o similar contara con el RE. 

RE Registro de estado (Registro transparente el usuario no lo puede tocar , dice si ha pasado algo en la última operación Over flow...Z, C...). (Propósito específico )

* Registro - Registro (viene de los registros) más rápido
* Registro - Memoria (viene de la memoria )
* Memoria - Memoria  más lento

##### U. CONTROL:
 
Extrae de MP la instrucción, la analiza
Activa las señales, es el que reparte las órdenes. Siempre se mira si estas en el nivel adecuado ( super user (1), user (0) ) 

* PC - Contador de programa : guarda la instrucción que vamos a ejecutar (Registro transparente)(Propósito específico). 

* RI - Registro instrucción : información sobre la instrucción, la primera parte me da los datos de la operación y el resto 

* DR - Registro de datos: Bus de datos

* AR - Registro de direcciones: Bus de direcciones


#### REGISTROS:

##### BR: Banco de registro 

##### MEMORIA PRINCIPAL:  
Conjunto de celdas,contiene los datos e instrucciones, señales : RD , WR , MEMRQ. Las operaciones de memoria tardan siempre más que los de CPU.  la memoria se separa en código datos ya que de primeras no hay una diferencia entre ellos, por eso es importante separar las instrucciones de los datos “00…….000001100000”. Puede ejecutar un dato pensando que es una instrucción. 
Todas las instrucciones usan la memoria TODAS “fase de fetch”.

![ Lo primero recordar bien las unidades que vamos a emplear ](http://www.angelfire.com/darkside/thc/espanol/neumann2.jpg)

##### Osea porque se usan registros ? 
porque son más rápidos y por ello al hacer un programa lo más eficiente, es guardar todos los datos que vayas a necesitar en registros y a partir de ahí realizar las operaciones que necesites. De este modo ahorras tiempo de lectura durante la ejecución de las instrucciones. 
Latencia: desde que empieza hasta que acaba  

#### UNIDAD E/S: 

Instrucciones 
Fases:
 
1. Fetch
2. Decodificación
3. Ejecución
4. Prepara siguiente instrucción
5. Vuelta al 1
6. Finalización 

Las instrucciones manejan 3 tipos de datos, palabras (4 bytes), medias palabras (2 bytes) y Bytes. 

##### E/S Programada
* Iniciar programa
* Comprobar estado
* Dato listo avanzamos, sino volvemos a comprobar estado
* enviamos dato 
* fin de operación , si, terminamos, sino volvemos a comprobar estado 

Osea como el nombre dice los pasos por el bus de E/S estan programadas y siguen un procedimiento establecido. 

##### E/S Interrupciones
La CPU inicia la operación E/S, Ejecuta el programa , mientras el periferico cuando tenga los datos listos realizara la interrupción, en ese momento la CPU inicia la rutina de tratamiento de interrupción RTI realizandose la transferencia de los datos. La CPU tiene que salvar y restaurar el estado.  
 
##### E/S DMA
La CPU inicia la operación E/S,el periferico mientras recibe la transferencua de datos a MP, la CPU Ejecuta el programa, y cuando se termina la transferencia de datos a MP se realiza una INT y la CPU realiza la rutina de tratamiento de interrupción.   


#Tema 2 Programación en ensamblador 
	Antes de empezar a programar --!Pensar!--

### Tipos de datos:
 
* Palabra: Tamaño privilegiado del computador 4 bytes
* Medias Palabras 2 bytes
* Bytes : cadena de carateres 

### Tipos de Direccionamiento

* A nivel de palabra: Cada palabra tiene una dirección 
* A nivel de Bit: Cada byte tiene una dirección, dos palabras estan separadas por el tamaño en bytes de la palabra.


### Alineamiento a palabra 

* Little-Endian - Byte menos significativo de una palabra en la dirección menos significativa. 
		
		0x10 20 30 40 
		1000 |	40 30 20 10
		1004 |

* Big-Endian :Byte menos significativo de una palabra en la dirección más significativa.

		0x10 20 30 40 
		1000 |	10 20 30 40
		1004 |

### Modos de Direccionamiento
El concepto de direccionamiento lo que hace es decir donde esta el objeto, de este modo tenemos inmediato que el objeto es en la propia instrucción. Directo , absoluto -> la instrucción con la dirección completa del objeto, 

* Inmediato: El objeto está contenido en la propia instrucción. ADD .R1,#4
* Relativo: Si la instrucción contiene la dirección del objeto de forma parcial. 
* Absoluto a registro: El objeto del direccionamiento está contenido en un registro. La instrucción contiene el registro que contiene el objeto del direccionamiento. 
	
		ADD .R4,.R5			R4<- R4+R5
		
* Absoluto a memoria: El objeto del direccionamiento está contenido en una dirección de memoria. La instrucción contiene la dirección completa de memoria que contiene el objeto del direccionamiento.	
	
		LD .R4,/1000			R4 <- M(1000)
				
* Relativo a registro base: El registro base se carga una dirección de memoria que contiene un conjunto de datos a los que se accede conociendo su posición relativa frente al comienzo de dicha zona: Estructuras de datos. El rango de direcciones al que se puede acceder está limitado por el tamaño del desplazamiento.

		DIR EFECTIVA = REGISTRO BASE(Registro de propósito específico o general que contiene una dirección a memoria.) + DESPLAZAMIENTO (Valor entero con signo.)
		
			LD .R1,#4[.R7]		R1<-MEM(R7+4)
		
*Relativo Registro Base: Registro de propósito específico o general que contiene una dirección a memoria + Desplazamiento: Valor entero con signo.

* Relativo a PC: Es un direccionamiento relativo a registro base en el que el registro base es el PC. “saltos”

			BR $10 		PC <- PC + 10
			
* Registro Índice: Es un direccionamiento relativo a registro base en el que el registro base se modifica. Incrementos y decrementos.

		Preincremento		LD .R1,#8[++.R7] R7 <- R7+4 R1 <- MEM(R7+8)		Predecremento		LD .R1,#8[--.R7] R7 <- R7-4 R1 <- MEM(R7+8)		Postincremento		LD .R1,#8[.R7++] R1 <- MEM(R7+8) R7 <- R7+4		Postdecremento		LD .R1,#8[.R7--] R1 <- MEM(R7+8) R7 <- R7-4 

* Indirecto A Registro: La instrucción contiene la especificación del registro que contiene la dirección de memoria donde está almacenado el objeto.

		LD .R1,[.R4]		R1<-MEM(R4)
		
* Indirecto A Memoria: La instrucción contiene una dirección de memoria donde está contenida la dirección donde se almacena el objeto.

		LD .R1, [/1000]		R1<- MEM(MEM(1000))

## Juego de instrucciones
###IEEE

#### Transferencia de datos

	LD			LD .R2,#4[.R4]		R2 <- MEM(R4+4)
	
	ST			ST .R2, #4[.R4]		MEM(R4+4) <- R2
		
	MOVE		MOVE .R2,.R4 			R4 <- R2				MOVE [.R2],[.R4]		MEM(R4) <- MEM(R2)
				
	PUSH 		PUSH .R1 				SP <- SP-4;MEM(SP) <- R1
	POP			POP .R1 				R1 <- MEM(SP); SP <- SP + 4
	BR			BR /1000 PC <- 1000
				BR #4[.R4] PC <- R4+4
				BR $10 PC <- PC + 10 ¡¡ El PC apunta a la dirección de la siguiente instrucción
				
	BZ /1000 			Si Z = 1 PC <- 1000	BNC #4[.R4] 		Si C = 0 PC <- R4+4	BP $10 			Si S = 0 PC <- PC + 10
	
	CALL /1000 R1 <- PC ; PC <- 1000	RET PC <- R1		
	
	ADD .R1, .R2 		R1 <- R1 + R2; mod. flags
		SUB .R1, .R2 		R1 <- R1 – R2; mod. flags
		MUL .R1, .R2 		R1 <- R1 · R2; mod. flags
		DIV .R1, .R2 		R1 <- R1 / R2; mod. flags
		ADDC .R1, .R2 	R1 <- R1 + R2 + c; mod. flags
		SUBC .R1, .R2 	R1 <- R1 - R2 - c; mod. flags
		CMP .R1, .R2 R1 – R2 ; mod. flags	
	
	•Modelo Registro-Registro.	ADD .R1,.R2 R1 <- R1 + R2; mod. Flags	ADD .R1,#4 R1 <- R1 + 4; mod. Flags	Modelo Registro-Memoria.	ADD .R1,[.R2] R1 <- R1 + MEM(R2); mod. Flags	Modelo Memoria-Memoria.	ADD [.R1],#4[.R2] MEM(R1) <- MEM(R1) + MEM(R2+4);


	AND .R1,.R2 R1 <- R1 AND R2; mod. Flags	XOR .R1,#4 R1 <- R1 XOR 4; mod. Flags	OR .R1,.R2 R1 <- R1 OR R2; mod. Flags	NOT .R1 R1 <- NOT R1; mod. Flags	Se utilizan para trabajar con máscaras	AND .R1, #1 ; Si Z = 1 el número es par
	
### Arquitectura 88110 

Usa 3 direcciones, modelo de ejecución registro-registro, palabras de 32 bits, ALU en complemento a 2.

* R0 Cableado a 0
* R1 Guarda dirección de retorno de subrutina accesibles por pares en operaciones de 64 bits.
* R30 Puntero de pila
* R31 Puntero de marco de pila

Tiene direccionamiento :

* Directo a registro: .Ri 
		
		add r1, r2, r3 ; r1 <- r2 + r3
* Inmediato: #aaaa

		add r1, r2, 0xFFF3
		add r1, r2, -13
* Relativo a registro base: #desp[.Ri]

		ld r1, r4, 13 r1 <- MEM(r4+13)		st r1, r4, 13 r1 <- MEM(r4+13)
* Relativo a PC: $xx

		ADD .7, .5		BR $ desp ; PC <- et1+desp		et1: LD .1, [.7]
		
* Indirecto a registro: [.Ri]

		JMP (.R10) ; PC <- R10

##### Juego de instrucciones

* Lógicas (or, and, xor, mask)* Aritméticas (add, sub, addu, subu, muls, mulu, divs, divu, cmp)* Bifurcaciones (bb0, bb1, br, bsr, jmp, jsr)* Transferencia (ld, st, ldcr, stcr, xmem)* Campos de bit (clr, set, ext, extu, mak, rot)* Coma flotante (fadd, fsub, fmul, fdiv, fcvt, flt, int, fcmp) 