# Alu
#### `Arquitectura de Alu en VHDL`

**Índice**
- [Introduccion](#id1)
- [Objetivos](#id2)
- [Planteamiento](#id3)
- [Especificaciones del Sistema](#id4)
- [Marco Teórico](#id5)
- [Desarrollo Experimental](#id6)
- [Análisis y Resultados](#id7)
- [Conclusiones](#id8)
- [Referencias](#id9)


## INTRODUCCIÓN<a name="id1"></a>
  
Los sistemas secuenciales están formados por una parte combinacional y un elemento de memoria, por ello este sistema no solo depende de las entradas sino también de sus salidas, pueden ser síncronos (sincronizados por un pulso de reloj) y asíncronos (dependen más del momento y orden de sus señales de entrada) como los fipflop y latch.
  
Existen también los registros, los cuales están compuestos de flip flop y con ello pueden almacenar más de un bit, existen varios tipos de registro dependiendo de cómo son sus salidas (paralelo o en serie). En esta práctica se pretende conocer mejor el funcionamiento de un registro y utilizarlo como almacenamiento para poder realizar operaciones entre dos números de 4 bits. Al igual que conocer cómo influye un flanco en el circuito. 

## OBJETIVOS<a name="id2"></a>
  
-	Determinar experimentalmente el correcto funcionamiento del registro PIPO 
-	Diseñar un registro PIPO de 4 bits 
-	Elaborar e implementar un reloj astable con el circuito integrado lm555
-	Diseñar una ALU de 4 bits
-	Implementar una tarjeta de desarrollo (FPGA) 
-	Interconectar nuestro registro con la unidad lógica aritmética (ALU)

## PLANTEAMIENTO<a name="id3"></a>

Para llevar a cabo la práctica es necesario realizar un programa en VHDL el cual deberá tener las operaciones:
|**ARITMÉTICA**|**LÓGICA**|
|:---:|:---:|
|    A    |    A'    |
|    B    |    B'    |
|   A+1	  |  A and B |
|   B+1	  |  A or B  |
|   A-1	  |  A nand B |
|   B-1	  |  A nor B |
|   A+B	  |  A xor B |
|   A+B+C	|    A xnor B |

De igual manera se deberá escoger y construir un registro talque al introducir A, B (4 bits) y la combinación de una operación los almacene en cada flanco del reloj.

Para ello las salidas del registro serán conectadas (en este caso) a una FPGA en la cual ya tendrá el código y las salidas de esta deberán conectarse a dos display para reflejar el resultado.

Para la elaboración de la práctica se requieren los siguientes materiales:

-	3 registros pipo.
-	Un circuito integrado lm555.
-	Potenciómetro de 50 k ohms.
-	Capacitador (47µF).
-	Resistencias (1 k y 330 ohm). 
-	Protoboard.
-	FPGA.
-	2 display cátodos común 

## ESPECIFICACIONES DEL SISTEMA<a name="id4"></a>

Se desea programar un datapath con sistema combinacional y secuencial, el cual contiene los siguientes bloques:

-	Multiplexores
-	ALU
-	Banco de registros
-	Memoria RAM
-	Corrimiento a la derecha o izquierda
-	Extensión de signo de 16 a 32 bits
-	Unidad de control 

El datapath se controla con instrucciones tipo R e I las cuales son:

1. Instrucciones R
   - Add : suma
   - Sub : resta
   - And : operación and
   - Or : operación or
   - Nor : operación nor
   - Slt : compara y pone ‘1’ si es verdad
   - Sll : corrimiento a la izquierda
   - Srl : corrimiento a la derecha

2. Instrucciones I
   - Addi : suma con constante
   - Andi : operación and con constante
   - Ori : operación or con constante
   - Alti : compara con constante y pone ‘1’ si es verdad
   - Beq : salta a la instrucción tipo J si son iguales
   - Bne salta a la instrucción tipo J si son diferentes
   - Lw : lee elemento guardado en la memoria
   - Sw : escribe un elemento en la memoria

![Arquitectura ALU](/Img/Imagen1.png)

## MARCO TEÓRICO<a name="id5"></a>

#### Registro

Un registro es un grupo de flip-flop, cada uno de los cuales comparten un reloj común y es capaz de guardar más de un bit de información. Un registro de n bits consiste en un grupo de n flip-flop capaces de guardar bits de información binaria, un registro puede tener compuertas combinacionales que realizan ciertas tareas de procesamiento de datos. Los flip-flop retienen la información binaria, y las compuertas determinan como se transfiere al registro.

![Arquitectura ALU](/Img/Imagen2.png)

En la imagen a) se muestra sistema de memoria que este caso sería un registro y nos daremos cuenta de que hay una retroalimentación, por lo tanto los valores de las salidas, en un momento dado, no dependen exclusivamente de los valores de las entradas en dicho momento, sino también dependen del estado anterior o estado interno. 

#### Registro de entrada y salida en paralelo (PIPO)

![Arquitectura ALU](/Img/Imagen3.png)

La entrada de datos es cada una de las entradas D del biestable; la entrada de habilitación se une a una entrada de habilitación global, de manera que cuando se activa, permite que se lean los datos. Hay otra entrada (control de salida) que al activarse permite que se lean las salidas.

#### Circuito integrado 555
Se utiliza en la generación de temporizadores, pulsos y oscilaciones. El 555 puede ser utilizado para proporcionar retardos de tiempo, como un oscilador, y como un circuito integrado flip flop.

![Arquitectura ALU](/Img/Imagen4.png)

#### Potenciómetro

El potenciómetro es una resistencia variable, limitan el paso de la corriente eléctrica (Intensidad) provocando una caída de tensión en ellos al igual que en una resistencia, pero en este caso el valor de la corriente y  la tensión en el potenciómetro las podemos variar solo con cambiar el valor de su resistencia. El potenciómetro más sencillo es una resistencia variable mecánicamente. Los primeros potenciómetros y más sencillos son los reóstatos.

![Arquitectura ALU](/Img/Imagen5.png)
 
Tenemos 3 terminales A, B y C. Si conectáramos los terminales A y B al circuito sería una resistencia Fija del valor igual al máximo de la resistencia que podría tener el reóstato. Ahora bien, si conectamos los terminales A y C el valor de la resistencia dependería de la posición donde estuviera el terminal C, que se puede mover hacia un lado o el otro. Hemos conseguido un Potenciómetro, ya que es una resistencia variable. Este potenciómetro es variable mecánicamente, ya que para que varíe la resistencia lo hacemos manualmente, moviendo el terminal C. Este tipo de potenciómetros se llaman reóstatos, suelen tener resistencia grande y se suelen utilizar en circuitos eléctricos por los que circula mucha intensidad.

#### Display de 7 segmento

Es un componente que se utiliza para la representación de números en muchos dispositivos electrónicos.
Cada elemento del display tiene asignado una letra que identifica su posición en el arreglo del display.

![Arquitectura ALU](/Img/Imagen6.gif)
 
#### Arreglo de leds en un display de 7 segmentos cátodo común. 

El display cátodo común tiene todos los ánodos de los diodos LED unidos y conectados a tierra. Para activar un segmento de estos hay que poner el ánodo del segmento a encender a Vcc (tensión de la fuente) a través de una resistencia para limitar el paso de la corriente.

![Arquitectura ALU](/Img/Imagen7.gif)
 
#### Capacitor

Un capacitor es un componente eléctrico pasivo que guarda energía y tiene la propiedad de capacitancia. En estado neutro, las dos placas de un capacitor tienen el mismo número de electrones libres. Cuando el capacitor se conecta a una fuente de voltaje mediante una resistencia, se liberan electrones de la placa A, los cuales se depositan en la placa B en un número igual deliberado. A medida que la placa A pierde electrones y la placa B los gana, la placa A se vuelve positiva con respecto a la placa B. Durante este proceso de carga, los electrones fluyen solo a través de los contactos. Por el dieléctrico del capacitor no fluyen electrones porque es un aislante. El movimiento de electrones cesa cuando el voltaje presente en el capacitor es igual al voltaje de la fuente, retiene la carga almacenada durante un largo periodo y aún tiene voltaje de un lado a otro de él. Un capacitor cargado es capaz de actuar como batería temporal.

![Arquitectura ALU](/Img/Imagen8.png)

#### FPGA
Una FPGA (del inglés Field Programmable Gate Array) es un dispositivo programable que contiene bloques de lógica cuya interconexión y funcionalidad puede ser configurada 'in situ' mediante un lenguaje de descripción especializado. La lógica programable puede reproducir desde funciones tan sencillas como las llevadas a cabo por una puerta lógica o un sistema combinacional hasta complejos sistemas en un chip.

![Arquitectura ALU](/Img/Imagen9.png)

Las FPGAs se utilizan en aplicaciones similares a los ASICs sin embargo son más lentas, tienen un mayor consumo de potencia y no pueden abarcar sistemas tan complejos como ellos. A pesar de esto, las FPGAs tienen las ventajas de ser reprogramables (lo que añade una enorme flexibilidad al flujo de diseño), sus costes de desarrollo y adquisición son mucho menores para pequeñas cantidades de dispositivos y el tiempo de desarrollo es también menor.

## DESARROLLO EXPERIMENTAL<a name="id6"></a>

#### Sumador medio

Se  realizó un código para un sumador medio por medio del diseño del circuito conectando compuertas lógicas.

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.all;

entity SUM_MED is
	 port(
		 A : in STD_LOGIC;
		 B : in STD_LOGIC;
		 S : out STD_LOGIC;
		 COUT : out STD_LOGIC
	     );
end SUM_MED;
architecture SUM_MED of SUM_MED is
begin
    S <= A XOR B;
	COUT <= A AND B;
end SUM_MED;
```
#### Sumador completo

Se  realizó un código para un sumador completo por medio del diseño del circuito conectando compuertas lógicas.

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.all;

entity SUM_COM is
	 port(
		 A : in STD_LOGIC;
		 B : in STD_LOGIC;
		 CIN : in STD_LOGIC;
		 S : out STD_LOGIC;
		 COUT : out STD_LOGIC
	     );
end SUM_COM;

architecture SUM_COM of SUM_COM is
begin
    S <= A XOR B XOR CIN;
	COUT <= (A AND B) OR ((A XOR B) AND CIN);
end SUM_COM;
```

#### Operaciones lógicas y aritméticas

Se realizó otro código en el cual se usó la función de `if – elsif` para hacer un tipo decodificador para que al dar una combinación realizaría una acción, como mostrar el valor de una de las entradas para reflejarla en la salida o usarla para una operación aritmética.

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.all;

entity COMPL is
	 port(
		 A : in STD_LOGIC_VECTOR(3 downto 0);
		 B : in STD_LOGIC_VECTOR(3 downto 0);
		 C : IN STD_LOGIC;
		 E : in STD_LOGIC_VECTOR(3 downto 0);
		 S : out STD_LOGIC_VECTOR(3 downto 0);
		 W : OUT STD_LOGIC_VECTOR(4 DOWNTO 0);
		 Y : OUT STD_LOGIC_VECTOR (3 DOWNTO 0);
		 R : OUT STD_LOGIC
	     );
end COMPL;

architecture COMPL of COMPL is
begin
	PROCESS (A,B,E,C)
	BEGIN
		IF (E="0000") THEN
			W(3 DOWNTO 0) <= A;
			W(4) <= '0';
		ELSIF (E="0001") THEN
			W(3 DOWNTO 0) <= B;
			W(4) <= '0';
	    ELSIF (E="0010") THEN
			Y <= A;
			S <= "0001";
		ELSIF (E="0011") THEN
			Y <= "0001";
			S <= B;
		ELSIF (E="0100") THEN
			IF (A="0000")THEN
			Y <= A;
			S <= "0001";
			ELSE
			Y <= A;
			S <= "1111";
			END IF;
		ELSIF (E="0101") THEN
			IF (B="0000")THEN
				Y<=B;
				S<="0001";
			ELSE
			Y <= B;
			S <= "1111";
			END IF;
		ELSIF (E="0110") THEN
			Y <= A;
			S <= B;
		ELSIF (E="0111") THEN
			Y <= A;
			S <= B;
			R <= C;
		ELSIF (E="1000") THEN
			W(3 DOWNTO 0) <= NOT A;
			W(4) <= '0';
		ELSIF (E="1001") THEN
			W(3 DOWNTO 0) <= NOT B;
			W(4) <= '0';
		ELSIF (E="1010") THEN
			W(3 DOWNTO 0) <= A AND B;
			W(4) <= '0';
		ELSIF (E="1011") THEN
			W(3 DOWNTO 0) <= A OR B;
			W(4) <= '0';
		ELSIF (E="1100") THEN
			W(3 DOWNTO 0) <= A NAND B;
			W(4) <= '0';
		ELSIF (E="1101") THEN
			W(3 DOWNTO 0) <= A NOR B;
			W(4) <= '0';
		ELSIF (E="1110") THEN
			W(0)<=A(0) XOR B(0);
			W(1)<=A(1) XOR B(1);
			W(2)<=A(2) XOR B(2);
			W(3)<=A(3) XOR B(3);
			W(4)<= '0';
		ELSE
			W(0)<=A(0) XNOR B(0);
			W(1)<=A(1) XNOR B(1);
			W(2)<=A(2) XNOR B(2);
			W(3)<=A(3) XNOR B(3);
			W(4)<= '0';
		END IF;
	END PROCESS;
end COMPL;
```

#### ALU

Para juntar todos estos bloques y otros que se explicaran a continuación se realizó un código el cual conectaba las salidas del anterior código para realizar la acción de suma de dos bits y otra para la acción de la suma de dos bits con acarreo.

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.all;

entity ALU is
	 port(
		 A : in STD_LOGIC_VECTOR(3 downto 0);
		 B : in STD_LOGIC_VECTOR(3 downto 0);
		 C : IN STD_LOGIC;
		 E : in STD_LOGIC_VECTOR(3 downto 0);
		 O : OUT STD_LOGIC_VECTOR (6 DOWNTO 0);
		 P : OUT STD_LOGIC_VECTOR (6 DOWNTO 0)
	     );
end ALU;

architecture ALU of ALU is
SIGNAL COUT,COU : STD_LOGIC_VECTOR (2 DOWNTO 0);
SIGNAL N,M : STD_LOGIC_VECTOR (3 DOWNTO 0);
SIGNAL T,X,Q,S : STD_LOGIC_VECTOR (4 DOWNTO 0);
SIGNAL L : STD_logic;
COMPONENT SUM_MED is
	 port(
		 A : in STD_LOGIC;
		 B : in STD_LOGIC;
		 S : out STD_LOGIC;
		 COUT : out STD_LOGIC
	     );
end COMPONENT;
COMPONENT SUM_COM is
	 port(
		 A : in STD_LOGIC;
		 B : in STD_LOGIC;
		 CIN : in STD_LOGIC;
		 S : out STD_LOGIC;
		 COUT : out STD_LOGIC
	     );
end COMPONENT;
COMPONENT COMPL is
	 port(
		 A : in STD_LOGIC_VECTOR(3 downto 0);
		 B : in STD_LOGIC_VECTOR(3 downto 0);
		 C : IN STD_LOGIC;
		 E : in STD_LOGIC_VECTOR(3 downto 0);
		 S : out STD_LOGIC_VECTOR(3 downto 0);
		 W : OUT STD_LOGIC_VECTOR(4 DOWNTO 0);
		 Y : OUT STD_LOGIC_VECTOR (3 DOWNTO 0);
		 R : OUT STD_LOGIC
	     );
end COMPONENT;
COMPONENT SALIDA is
	 port(
		 A : in STD_LOGIC_VECTOR(4 downto 0);
		 B : in STD_LOGIC_VECTOR(4 downto 0); 
		 C : in STD_LOGIC_VECTOR(4 downto 0);
		 E : in STD_LOGIC_VECTOR(3 DOWNTO 0);
		 S : out STD_LOGIC_VECTOR(4 downto 0)
	     );
end COMPONENT;
COMPONENT BCD_1 is
	 port(
		 A : in STD_LOGIC_VECTOR(4 downto 0);
		 S : out STD_LOGIC_VECTOR(6 downto 0)
	     );
end COMPONENT;
COMPONENT BCD_2 is
	 port(
		 A : in STD_LOGIC_VECTOR(4 downto 0);
		 S : out STD_LOGIC_VECTOR(6 downto 0)
	     );
end COMPONENT;
BEGIN
	C0: COMPL PORT MAP (A,B,C,E,N,T,M,L);
	
	U0: SUM_MED PORT MAP (M(0),N(0),X(0),COUT(0));						   
	U1: SUM_COM PORT MAP (M(1),N(1),COUT(0),X(1),COUT(1));
	U2: SUM_COM PORT MAP (M(2),N(2),COUT(1),X(2),COUT(2));
	U3: SUM_COM PORT MAP (M(3),N(3),COUT(2),X(3),X(4));
	
	K0: SUM_COM PORT MAP (M(0),N(0),L,Q(0),COU(0));
	K1: SUM_COM PORT MAP (M(1),N(1),COU(0),Q(1),COU(1));
	K2: SUM_COM PORT MAP (M(2),N(2),COU(1),Q(2),COU(2));
	K3: SUM_COM PORT MAP (M(3),N(3),COU(2),Q(3),Q(4));
	
	S0: SALIDA PORT MAP (T,X,Q,E,S);
	
	E0: BCD_1 PORT MAP (S,O);
	
	Z0:	BCD_2 PORT MAP (S,P);
	
end ALU;
```

#### Multiplexor

Ya que en el código de ALU se realizaban tres operaciones (suma de dos bits sin y con acarreo y reflejar el número y operación lógica de las variables A y B) se realizó un código el cual dependiendo de la misma combinación que se usó en el código de operaciones para mostrar solo una salida de las tres operaciones de la ALU que se quería reflejar, el cual también está en el código de la ALU como: `S0: SALIDA`.

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.all;

entity SALIDA is
	 port(
		 A : in STD_LOGIC_VECTOR(4 downto 0);
		 B : in STD_LOGIC_VECTOR(4 downto 0);
		 C : IN STD_LOGIC_VECTOR (4 DOWNTO 0);
		 E : in STD_LOGIC_VECTOR(3 DOWNTO 0);
		 S : out STD_LOGIC_VECTOR(4 downto 0)
	     );
end SALIDA;

architecture SALIDA of SALIDA is
begin
	PROCESS (A,B,E,C)
	BEGIN
		IF(E="0000")THEN
			S<=A;
		ELSIF(E="0001")THEN
			S<=A;
		ELSIF (E="0010")THEN
			S<=B;
		ELSIF (E="0011")THEN
			S<=B;
		ELSIF (E="0100")THEN
			S<=B;
			S(4) <= '0';
		ELSIF (E="0101")THEN
			S<=B;
			S(4) <= '0';
		ELSIF (E="0110")THEN
			S<=B;
		ELSIF (E="0111")THEN
			S<=C;
		ELSIF (E="1000")THEN
			S<=A;
		ELSIF (E="1001")THEN
			S<=A;
		ELSIF (E="1010")THEN
			S<=A;
		ELSIF (E="1011")THEN
			S<=A;
		ELSIF (E="1100")THEN
			S<=A;
		ELSIF (E="1101")THEN
			S<=A;
		ELSIF (E="1110")THEN
			S<=A;
		ELSE
			S<=A;
		END IF;
	END PROCESS;
end SALIDA;
```

#### Decodificadores

Ya que el resultado se mostró en display de 7 segmentos se realizaron dos códigos de decodificadores para los display los cuales se conectaron del código del MULTIPLEXOR a la salida del código de la ALU.     

##### `1° código`
```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.all;

entity BCD_1 is
	 port(
		 A : in STD_LOGIC_VECTOR(4 downto 0);
		 S : out STD_LOGIC_VECTOR(6 downto 0)
	     );
end BCD_1;

--}} End of automatically maintained section

architecture BCD_1 of BCD_1 is
begin
PROCESS (A) BEGIN
		CASE A IS 
			WHEN "00000" => S <= "1111110";
			WHEN "00001" => S <= "0110000";
			WHEN "00010" => S <= "1101101";
			WHEN "00011" => S <= "1111001";
			WHEN "00100" => S <= "0110011";
			WHEN "00101" => S <= "1011011";
			WHEN "00110" => S <= "1011111";
			WHEN "00111" => S <= "1110001";
			WHEN "01000" => S <= "1111111";
			WHEN "01001" => S <= "1111011";
			WHEN "01010" => S <= "1111110";
			WHEN "01011" => S <= "0110000";
			WHEN "01100" => S <= "1101101";
			WHEN "01101" => S <= "1111001";
			WHEN "01110" => S <= "0110011";
			WHEN "01111" => S <= "1011011";
			WHEN "10000" => S <= "1011111";
			WHEN "10001" => S <= "1110001";
			WHEN "10010" => S <= "1111111";
			WHEN "10011" => S <= "1111011";
			WHEN "10100" => S <= "1111110";
			WHEN "10101" => S <= "0110000";
			WHEN "10110" => S <= "1101101";
			WHEN "10111" => S <= "1111001";
			WHEN "11000" => S <= "0110011";
			WHEN "11001" => S <= "1011011";
			WHEN "11010" => S <= "1011111";
			WHEN "11011" => S <= "1110001";
			WHEN "11100" => S <= "1111111";
			WHEN "11101" => S <= "1111011";
			WHEN "11110" => S <= "1111110";
			WHEN OTHERS => S <= "0110000";
			
		END CASE;
	END PROCESS;
end BCD_1;
```

##### `2° código`

```vhdl
library IEEE;
use IEEE.STD_LOGIC_1164.all;

entity BCD_2 is
	 port(
		 A : in STD_LOGIC_VECTOR(4 downto 0);
		 S : out STD_LOGIC_VECTOR(6 downto 0)
	     );
end BCD_2;

--}} End of automatically maintained section

architecture BCD_2 of BCD_2 is
begin
PROCESS (A) BEGIN
		CASE A IS 
			WHEN "00000" => S <= "0000000";
			WHEN "00001" => S <= "0000000";
			WHEN "00010" => S <= "0000000";
			WHEN "00011" => S <= "0000000";
			WHEN "00100" => S <= "0000000";
			WHEN "00101" => S <= "0000000";
			WHEN "00110" => S <= "0000000";
			WHEN "00111" => S <= "0000000";
			WHEN "01000" => S <= "0000000";
			WHEN "01001" => S <= "0000000";
			WHEN "01010" => S <= "0110000";
			WHEN "01011" => S <= "0110000";
			WHEN "01100" => S <= "0110000";
			WHEN "01101" => S <= "0110000";
			WHEN "01110" => S <= "0110000";
			WHEN "01111" => S <= "0110000";
			WHEN "10000" => S <= "0110000";
			WHEN "10001" => S <= "0110000";
			WHEN "10010" => S <= "0110000";
			WHEN "10011" => S <= "0110000";
			WHEN "10100" => S <= "1101101";
			WHEN "10101" => S <= "1101101";
			WHEN "10110" => S <= "1101101";
			WHEN "10111" => S <= "1101101";
			WHEN "11000" => S <= "1101101";
			WHEN "11001" => S <= "1101101";
			WHEN "11010" => S <= "1101101";
			WHEN "11011" => S <= "1101101";
			WHEN "11100" => S <= "1101101";
			WHEN "11101" => S <= "1101101";
			WHEN OTHERS => S <= "1111001";
			
		END CASE;
	END PROCESS;
end BCD_2;
```

#### Reloj

Para realizar el reloj se usó un circuito integrado 555 y un potenciómetro de 50 kilos para regular la frecuencia, regulados con un capacitor de 47 microfaradios y una resistencia de 1 kilo como se muestra en el diagrama. 
   
![Arquitectura ALU](/Img/Imagen10.jpg)

#### Registro 

Para hacer el registro de cuatro bits se realizó un registro PIPO utilizando tres registros 74HC194.

![Arquitectura ALU](/Img/Imagen11.png)

## ANÁLISIS Y RESULTADOS<a name="id7"></a>

El diseño de los registros y la ALU, ambos de 4 bits, se llevó a cabo de forma exitosa, así como la elaboración del reloj astable con el circuito integrado LM555.

Al hacer funcionar la ALU con los registros, pudimos comprobar que funcionó como se esperaba:
Los registros guardaron tanto los valores de  A y B,  como el del flanco selectivo C; en cada flanco de subida se actualizaban los valores, y si el flanco se mantenía en cero, los valores dados a las variables no cambiaban.

Las combinaciones que se utilizaron para realizar las operaciones deseadas se muestran en la tabla que aparece a continuación, cabe mencionar que cada una de las combinaciones fue comprobada:

<table>
<thead>
  <tr>
    <th colspan="3">&nbsp;&nbsp;&nbsp;<br>Operación&nbsp;&nbsp;&nbsp;aritmética&nbsp;&nbsp;&nbsp;</th>
    <th colspan="2">&nbsp;&nbsp;&nbsp;<br>Operación&nbsp;&nbsp;&nbsp;lógica&nbsp;&nbsp;&nbsp;</th>
  </tr>
</thead>
<tbody>
  <tr>
    <td>&nbsp;&nbsp;&nbsp;<br>0000&nbsp;&nbsp;&nbsp;</td>
    <td colspan="2">&nbsp;&nbsp;&nbsp;<br>A&nbsp;&nbsp;&nbsp;</td>
    <td>&nbsp;&nbsp;&nbsp;<br>1000&nbsp;&nbsp;&nbsp;</td>
    <td>&nbsp;&nbsp;&nbsp;<br>A’&nbsp;&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td>&nbsp;&nbsp;&nbsp;<br>0001&nbsp;&nbsp;&nbsp;</td>
    <td colspan="2">&nbsp;&nbsp;&nbsp;<br>B&nbsp;&nbsp;&nbsp;</td>
    <td>&nbsp;&nbsp;&nbsp;<br>1001&nbsp;&nbsp;&nbsp;</td>
    <td>&nbsp;&nbsp;&nbsp;<br>B’&nbsp;&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td>&nbsp;&nbsp;&nbsp;<br>0010&nbsp;&nbsp;&nbsp;</td>
    <td colspan="2">&nbsp;&nbsp;&nbsp;<br>A+1&nbsp;&nbsp;&nbsp;</td>
    <td>&nbsp;&nbsp;&nbsp;<br>1010&nbsp;&nbsp;&nbsp;</td>
    <td>&nbsp;&nbsp;&nbsp;<br>A+B&nbsp;&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td>&nbsp;&nbsp;&nbsp;<br>0011&nbsp;&nbsp;&nbsp;</td>
    <td colspan="2">&nbsp;&nbsp;&nbsp;<br>B+1&nbsp;&nbsp;&nbsp;</td>
    <td>&nbsp;&nbsp;&nbsp;<br>1011&nbsp;&nbsp;&nbsp;</td>
    <td>&nbsp;&nbsp;&nbsp;<br>A•B&nbsp;&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td>&nbsp;&nbsp;&nbsp;<br>0100&nbsp;&nbsp;&nbsp;</td>
    <td colspan="2">&nbsp;&nbsp;&nbsp;<br>A-1&nbsp;&nbsp;&nbsp;</td>
    <td>&nbsp;&nbsp;&nbsp;<br>1100&nbsp;&nbsp;&nbsp;</td>
    <td>&nbsp;&nbsp;&nbsp;<br>(A+B)’&nbsp;&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td>&nbsp;&nbsp;&nbsp;<br>0101&nbsp;&nbsp;&nbsp;</td>
    <td colspan="2">&nbsp;&nbsp;&nbsp;<br>B-1&nbsp;&nbsp;&nbsp;</td>
    <td>&nbsp;&nbsp;&nbsp;<br>1101&nbsp;&nbsp;&nbsp;</td>
    <td>&nbsp;&nbsp;&nbsp;<br>(A•B)’&nbsp;&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td>&nbsp;&nbsp;&nbsp;<br>0110&nbsp;&nbsp;&nbsp;</td>
    <td colspan="2">&nbsp;&nbsp;&nbsp;<br>A+B&nbsp;&nbsp;&nbsp;</td>
    <td>&nbsp;&nbsp;&nbsp;<br>1110&nbsp;&nbsp;&nbsp;</td>
    <td>&nbsp;&nbsp;&nbsp;<br>A xor B&nbsp;&nbsp;&nbsp;</td>
  </tr>
  <tr>
    <td colspan="2">&nbsp;&nbsp;&nbsp;<br>0111&nbsp;&nbsp;&nbsp;</td>
    <td>&nbsp;&nbsp;&nbsp;<br>A+B+Cin&nbsp;&nbsp;&nbsp;</td>
    <td>&nbsp;&nbsp;&nbsp;<br>1111&nbsp;&nbsp;&nbsp;</td>
    <td>&nbsp;&nbsp;&nbsp;<br>A®B&nbsp;&nbsp;&nbsp;</td>
  </tr>
</tbody>
</table>

###### _TABLA 1. Combinaciones para operaciones_

Se puede ver que cuando el bit más significativo de la combinación es cero, la operación que realiza la ALU es aritmética, y si el valor es 1, entonces se lleva a cabo una operación lógica.

![Arquitectura ALU](/Img/Imagen12.png)

## CONCLUSIONES<a name="id8"></a>

En esta práctica se comprendió el funcionamiento de los registros PIPO, gracias a la aplicación que le dimos en la Unidad Lógica Aritmética; Se pudo observar cómo, cada uno de ellos, guardaba el valor de los números de 4 bits deseado.

También se entendió cómo los sistemas combinacionales, es decir, la ALU, y secuenciales, los registros, se complementan. Así, en proyectos futuros, podemos aplicar los conocimientos obtenidos en esta práctica de forma exitosa.

Gracias a los conocimientos previos de sistemas combinacionales y a los adquiridos recientemente, pudimos realizar esta práctica con éxito.

## REFERENCIAS<a name="id9"></a>

>	Diseño digital, principios y prácticas, tercera edición. John F. Wakerly

>	Diseño digital, quinta edición. M. Morris Mano, Michael D. Ciletti

>	Principios de circuitos eléctricos, octava edición. Thomas L. Floyd
