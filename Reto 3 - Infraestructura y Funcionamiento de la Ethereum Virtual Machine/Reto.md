# **Ethereum program developer**

<br/>

## **Reto 3 - Infraestructura y Funcionamiento de la Ethereum Virtual Machine**

---

<br/>

### **Actividad 1:** Ejecutar el siguiente código en un cliente de la EVM y explicar el paso a paso que realiza en su ejecución.

<br/>

```
evm --code 0x363d3d37363df3 --debug run
```

<br/>

La línea de comando previa, es una ejecución que nos mostrara en pantalla el paso a paso de una serie de instrucciones que le enviamos a nuestro **cliente de la EVM**.

<br/>

- **evm:** Comando para ejecutar instrucciones en nuestro cliente de la EVM.

- **--code:** Comando para ingresar una serie de instrucciones a nuestro cliente de la EVM, en este caso las instrucciones son **"0x363d3d37363df3"**.

    <br/>

    1. **0x:** Protocolo de código abierto basado en Ethereum.
    
    2. **36:** CALLDATASIZE.

    3. **3d:** RETURNDATASIZE.

    4. **37:** CALLDATACOPY.

    5. **f3:** RETURN.

    <br/>
    
- **--debug:** Comando para que las instrucciones que escribimos, previamente, como valor en el comando **"--code"** y su información, sean pintadas en pantalla.

- **run:** Comando para ejecutar la serie de comando que escribimos.

<br/>

### **Output:**

<br/>


```
0x
#### TRACE ####
CALLDATASIZE    pc=00000000 gas=10000000000 cost=2

RETURNDATASIZE  pc=00000001 gas=9999999998 cost=2
Stack:
00000000 0x0

RETURNDATASIZE  pc=00000002 gas=9999999996 cost=2
Stack:
00000000 0x0
00000001 0x0

CALLDATACOPY    pc=00000003 gas=9999999994 cost=3
stack:
00000000 0x0
00000001 0x0
00000002 0x0

CALLDATASIZE    pc~00000004 gas=9999999991 cost=2

RETURNDATASIZE  pc=00000005 gas=9999999989 cost=2
Stack:
00000000 0x0

RETURN          pc=00000006 gas=9999999987 cost=0
```


---

<br/>

#### 1. **Primer grupo de líneas:**

<br/>


```
0x
#### TRACE ####
CALLDATASIZE    pc=00000000 gas=10000000000 cost=2
```

<br/>

**Línea 1:** Nos dice que se está utilizando el protocolo de código abierto basado en Ethereum "0x".

**Línea 2:** Nos dice que iniciará un proceso de rastreo, que en este caso rastrea el proceso de esta transacción.

**Línea 3:** En esta línea se ejecuta nuestra primera instrucción: **CALLDATASIZE**. Como su nombre dice, lo que hace es obtener el tamaño de los datos de la entrada en el entorno actual. 

A su vez, en la tercera línea nos muestra otros datos importantes:

<br/>

- **pc o program counter:** Registro del procesado, de un computador, que indica la posición donde está en su secuencia de instrucciones. Contiene la posición de la instrucción a ejecutar o la posición de la siguiente instrucción.

- **gas:** El gas que tenemos actualmente para la ejecución de nuestras instrucciones.

- **cost:** El costo en **gas** para realizar esta instrucción.

<br/>

En este caso el **pc** es "00000000", el **gas total** que tenemos es de "10000000000" y el **costo en gas**, para esta instrucción, fue de 2.

<br/>

---

<br/>
<br/>

#### **Segundo grupo de líneas:**

<br/>

```
RETURNDATASIZE  pc=00000001 gas=9999999998 cost=2
Stack:
00000000 0x0
```

<br/>

**Línea 1:** Nos entrega los mismos datos del anterior grupo de líneas, el pc, el gas y el cost. Pero también nos muestra una nueva instrucción llamada **"RETURNDATASIZE"**, que corresponde al **"3d"** en el grupo de instrucciones que introducimos en el **comando --code** en la consola. 

Lo que hace esta instrucción es obtener el tamaño de los datos de salida de la llamada anterior del entorno y empuja el tamaño del búfer de datos en la **Stack (pila)**.

**Línea 2 y 3:** En la línea dos nos enseña el texto **"Stack"** para indicarnos que lo que sigue en la línea 3 es el valor actualmente que contiene nuestra stack en la ejecución.

<br/>

---

<br/>
<br/>

#### **Tercer grupo de líneas:**

<br/>

```
RETURNDATASIZE  pc=00000002 gas=9999999996 cost=2
Stack:
00000000 0x0
00000001 0x0
```

<br/>

Repetimos la misma instrucción del grupo de líneas anterior, con la diferencia que el contenido de nuestra stack ha incrementado.

<br/>

---

<br/>
<br/>

#### **Cuarto grupo de líneas:**

<br/>

```
CALLDATACOPY    pc=00000003 gas=9999999994 cost=3
stack:
00000000 0x0
00000001 0x0
00000002 0x0
```

<br/>

**Línea 1:** En esta línea la instrucción ahora es **"CALLDATACOPY" (37)**, lo que hace esta instrucción es copiar los datos de entrada actual del entorno a la memoria.

En las siguientes 4 líneas, nuevamente, lo que hace es mostrarnos la stack y el contenido actual que tiene. Que ha ido incrementando tras cada instrucción.

<br/>

---

<br/>
<br/>


#### **Quinto grupo de líneas:**

<br/>

```
CALLDATASIZE    pc=00000004 gas=9999999991 cost=2
```

<br/>

No es un grupo, sino una única línea con la instrucción **"CALLDATASIZE" (36)**. Qué como al inicio, lo que hace es obtener el valor de los datos de entrada en el entorno.

<br/>

---

<br/>
<br/>

#### **Sexto grupo de líneas:**

<br/>

```
RETURNDATASIZE  pc=00000005 gas=9999999989 cost=2
Stack:
00000000 0x0
```

<br/>

Como hizo en el grupo de líneas 2, lo que hace es obtener el tamaño de los datos de salida de la llamada anterior del entorno. Y nuevamente nos muestra la stack, pero el contenido se ha reseteado, esto es porque cada vez que una instrucción retorna, lo que hace es sacar su valor del stack.

<br/>

---

<br/>
<br/>

#### **Septimo grupo de líneas:**

<br/>

```
RETURN    pc=00000006 gas=9999999987 cost=0
```

<br/>

Nuevamente, una única línea con la instrucción **"RETURN" (f3)**. Lo que hace es detener la ejecución, finalizar el rastreo y devolvernos los datos de salida.

<br/>

---

<br/>
<br/>

### **Actividad 2:** Ejecutar el siguiente código en un cliente de la EVM y explicar el por qué del valor de retorno.

<br/>

```
evm --code 0x363d3d37363df3 --input 0x12 run
```

<br/>

Como en la primera actividad, ingresamos una serie de comandos en la terminal, con la diferencia que esta vez colocamos un comando nuevo, **--input** con su valor **0x12** y removimos el comando **"--debug"**.

<br/>

- **--input:** Valor de entrada para la ejecución de las instrucciones del comando **"--code"**.

<br/>

Esta línea de comando lo que hará es realizar las diferentes instrucciones que colocamos como valor en el comando **"--code"**, **tomando como entrada** el valor que colocamos en el comando **"--input"**. Con la diferencia que esta vez no nos enseñara por pantalla el paso a paso (ya que eliminamos el comando **--debug**), sino que nos devolverá directamente los datos de salida.

Al ser instrucciones que solo leen, copian y retornan los datos, nos devolverá, como valor de salida, el mismo valor de entrada.

### **Output:**

<br/>

```
0x12
```

<br/>

---

<br/>
<br/>

## **Recursos utilizados para completar este reto:**

<br/>

- [**Platzi** - Ethereum developer program | Live class 3 (Infraestructura y Funcionamiento de la Ethereum Virtual Machine
)]()

- [**Coin Market Cap** - glossary (0x protocol)](https://coinmarketcap.com/alexandria/glossary/0x-protocol)

- [**Go Ethereum** - Documentation](https://geth.ethereum.org/docs/)

- [**EVM Codes**](https://www.evm.codes/)

- [**GitHub** - evm-opcodes repository](https://github.com/crytic/evm-opcodes)

- [**Hyperledger Besu** - EVM tool reference](https://besu.hyperledger.org/en/stable/Reference/Evm-Tool/#nomemory)

- [**Wikipedia** - ¿Qué es un contador de programa?](https://es.wikipedia.org/wiki/Contador_de_programa)