# Sistemas en tiempo real

## Biblioteca HAL

Permite gestionar el periférico GPIO. Se usan de la siguiente forma:
* Para activar o desactivar los pines de algún puerto en específico: 

~~~
HAL_GPIO_WritePin(GPIO_TypeDef *GPIOx, uint16_t GPIO_Pin,GPIO_PinState PinState)
~~~

**Ejemplo:**

~~~
HAL_GPIO_WritePin(LD2_GPIO_Port, LD2_Pin,GPIO_PIN_SET)
~~~
LD2 corresponde al LED de la tarjeta, el cual su estado será encendido.

Ahora, si :

HAL_GPIO_WritePin(LD2_GPIO_Port, LD2_Pin,GPIO_PIN_RESET)

LD2 corresponde al LED de la tarjeta, pero su estado será apagado.

* Para conmutar de 1 a 0 y de 0 a 1 en los pines especificados.

~~~
HAL_GPIO_TogglePin(GPIO_TypeDef *GPIOx, uint16_t GPIO_Pin)
~~~

**Ejemplo:**

~~~
HAL_GPIO_TogglePin(LD2_GPIO_Port,LD2_Pin)
~~~
 
Para que se haga la conmutación de 1 a 0 y de 0 a 1 se debe pone dentro de un while.	

•	Para leer el pin del puesto especificado.
~~~
HAL_GPIO_ReadPin(GPIO_TypeDef *GPIOx, uint16_t GPIO_Pin)
~~~

**Ejemplo:**

~~~
HAL_GPIO_ReadPin(B1_GPIO_Port,B1_Pin)
~~~
B1 corresponde al pulsador de la tarjeta y esta función se encarga de determinar su estado.

**Nota:** Con Ctrl+espacio el programa ayuda a completar la función.

## Ejemplo 1:

Pasos para la creación de un proyecto [aquí](https://github.com/MarianaEstrada/Pasos-para-crear-un-proyecto/blob/master/README.md "Pasos para crear un proyecto")

Hacer titilar un LED. Para realizar este ejemplo se usará el LED de la tarjeta ubicado en LD2.
Se van a exponer dos formas de hacer titilar el LED:

1.	Con la función de HAL_GPIO_WritePin.

En la parte del programa while(1), se empieza de la siguiente manera:

~~~
While(1)
{
//Se enciende el LED
HAL_GPIO_WritePin(LD2_GPIO_Port, LD2_Pin,GPIO_PIN_SET);
//Por 10000 ms
HAL_Delay(1000);
//Ahora se apaga el LED
HAL_GPIO_WritePin(LD2_GPIO_Port, LD2_Pin,GPIO_PIN_RESET);
//Por 10000 ms
HAL_Delay(1000);
}
~~~

2.	Con la función HAL_GPIO_TogglePin.

En la parte del programa while(1), se empieza de la siguiente manera:

~~~
While(1)
{
//Se realiza un toggle que realiza una conmutación de estado de 1 a 0 y de 0 a 1
HAL_GPIO_TogglePin(LD2_GPIO_Port,LD2_Pin)
//Por 10000 ms
HAL_Delay(1000);
}
~~~
Pasos para correr el programa [Aquí](https://github.com/MarianaEstrada/Pasos-para-correr-un-proyecto/blob/master/README.md)


## Ejemplo 2:

Pasos para la creación de un proyecto [aquí](https://github.com/MarianaEstrada/Pasos-para-crear-un-proyecto/blob/master/README.md "Pasos para crear un proyecto")

Utilizando el botón ubicado en B1, cada vez que se realice una pulsación debe realizar:
1.	Primera pulsación, encender el LED.
2.	Segunda pulsación, apagar el LED.
3.	Tercera pulsación, hacer titilar el LED cada 1000ms.
4.	Cuarta pulsación, hacer la primera letra del nombre en código Morse, donde una línea son 300ms, un punto 100ms y el intervalo de espacio entre símbolos es de 100ms.

* Primero se va a crear tres variables en el main(void), de la siguiente forma:
~~~
Int main (void)
{
Int a;
Int b;
Int c;
a=0;
b=0;
c=0
~~~

* Segundo se va a empezar el programa en el while(1), de la siguiente forma:

~~~
While(1) 
{
//Dado que dependido el número de pulsaciones se realiza una acción diferente, se usará una variable “a” para determinar cuantas pulsaciones se han dado:
//Se usará la función HAL_GPIO_ReadPin para determinar el estado de B1 donde se encuentra el pulsador.

//Primero se hará un if con el fin de determinar que a=0 y que el pulsador lo hayan oprimido
//Se utiliza el ! antes del HAL para que solo cuente cuando se haya oprimido el botón
If ((!HAL_GPIO_ReadPin(B1_GPIO_Port,B1_Pin)) && (a==0)){
//Se enciende el LED;
HAL_GPIO_WritePin(LD2_GPIO_Port, LD2_Pin,GPIO_PIN_SET);
a= a+1; 
//Se realiza un retraso de 500 ms
HAL_Delay(500);
}

//Segundo se hará un if con el fin de determinar que a=1 y que el pulsador lo hayan oprimido
//Se utiliza el ! antes del HAL para que solo cuente cuando se haya oprimido el botón
If ((!HAL_GPIO_ReadPin(B1_GPIO_Port,B1_Pin)) && (a==1)){
//Se apaga el LED;
HAL_GPIO_WritePin(LD2_GPIO_Port, LD2_Pin,GPIO_PIN_RESET);
a= a+1; 
//Se realiza un retraso de 500 ms
HAL_Delay(500);
}

//Tercero se hará un if con el fin de determinar que a=2 y que el pulsador lo hallan oprimido
//Se utiliza el ! antes del HAL para que solo cuente cuando se haya oprimido el botón
If ((!HAL_GPIO_ReadPin(B1_GPIO_Port,B1_Pin)) && (a==2)){
//Como se mencionó en el ejemplo anterior hay dos formar de hacer titilar un LED, en este caso se usara la función Toggle este debe ser usado dentro de un while para que se pueda realizar la conmutación, por esta razón se usara la variable “b” para que se pueda salir del while cuando se vuelva a oprimir el botón.
While (b==0){
 HAL_GPIO_TogglePin(LD2_GPIO_Port, LD2_Pin);
//Por 1000 ms
HAL_Delay(1000);
//Se mira si el botón a cambiado de estado.
If (!HAL_GPIO_ReadPin(B1_GPIO_Port,B1_Pin)){
b= b+1;
}
a= a+1;
}

//Finalmente se hará la letra M en código morse que equivale a dos líneas, por esto
If ((!HAL_GPIO_ReadPin(B1_GPIO_Port,B1_Pin)) && (a==3)){
While(c==0){
//Se enciende el LED
HAL_GPIO_WritePin(LD2_GPIO_Port, LD2_Pin,GPIO_PIN_SET);
//Por 300 ms
HAL_Delay(300);
//Ahora se apaga el LED
HAL_GPIO_WritePin(LD2_GPIO_Port, LD2_Pin,GPIO_PIN_RESET);
//Por 100 ms
HAL_Delay(100);
//Se enciende el LED
HAL_GPIO_WritePin(LD2_GPIO_Port, LD2_Pin,GPIO_PIN_SET);
//Por 300 ms
HAL_Delay(300);
//Ahora se apaga el LED
HAL_GPIO_WritePin(LD2_GPIO_Port, LD2_Pin,GPIO_PIN_RESET);

If (!HAL_GPIO_ReadPin(B1_GPIO_Port,B1_Pin)){
c= c+1;
}
a= 0;
b=0
c=0
}
}
}

~~~

Pasos para correr el programa [Aquí](https://github.com/MarianaEstrada/Pasos-para-correr-un-proyecto/blob/master/README.md)

## Máquina de estados

Se permite modelar el comportamiento de un sistema, sabiendo todos los posibles estados de esta. El estado de la máquina es evaluado periódicamente, cada vez que se evalúa el nuevo estado es elegido junto con su salida.

Existes dos tipos de máquinas de estados:

* **Máquina de estado Moore:** La salida depende únicamente del estado y el próximo estado depende del estado actual y de la entrada.
* **Máquina de estados Mealy:** Crea máquinas de estado desde funciones matemáticas donde describe las salidas en términos de la entrada.

**Ejemplo**

![MEF](https://github.com/MarianaEstrada/Biblioteca_HAL/blob/master/Imagenes/MEF.PNG)

Lo que se presenta en la imagen es un cambio de estado de un LED, encendido o apagado y el cambio de este depende de si se oprime o no el pulsador.


Pasos para la creación de un proyecto [aquí](https://github.com/MarianaEstrada/Pasos-para-crear-un-proyecto/blob/master/README.md "Pasos para crear un proyecto")

Acontinuación se presentan algunos ejemplos de máquina de estados:

## Antirrebote

### 1. La primera máquina de estados que se va a usar es la siguiente:

![ME2](https://github.com/MarianaEstrada/Biblioteca_HAL/blob/master/Imagenes/ME2.PNG)

Esta posee cuatro estados para el LED y dos estados para el pulsador, donde los estados 2 y 4 son de transición

* En el main. h del proyecto, ubicado en la carpeta Inc se pone la siguiente información:
~~~


/* USER CODE BEGIN ET */
// Se van a crear dos variables del tipo enum, una para los estados del LED y otra para los estados del pulsador.
// En C++ un tipo de dato enum permite asociar nombres con números, enumerando automáticamente cualquier lista.
enum states {LED_ON_DOWN, LED_ON_UP, LED_OFF_DOWN, LED_OFF_UP} current_state;
enum inputs {PB_DOWN,PB_UP} current_input;
/* USER CODE END ET */

/* USER CODE BEGIN EFP */
// Se crea una variable de un tamaño fijo de 8 bits con signo
int8_t get_input(void);
// Se crea una función void(que no tiene atributo o declaración) llamada set_output
void set_output(int8_t current_state);
/* USER CODE END EFP */
~~~
* En el main. c del proyecto, ubicado en la carpeta Src se pone la siguiente información:
~~~
/* USER CODE BEGIN 2 */
  //Se crea una variable sin signo de 8 bits para ser usada en un contador y determinar cuantas veces ha sido oprimido el pulsador.
  uint8_t cuenta = 0;
  // Se inicializa la variable current_state, estando aquí en la primera etapa de la maquina de estados.
  current_state = LED_OFF_UP;
  /* USER CODE END 2 */

/* USER CODE BEGIN 3 */
	  	//LED CONTROL 4 STATES
		// La función current_imput me dirá en que estado está mi entrada UP o down esto apartir de la función get_input
	  	  current_input = get_input();
		  // Current_state empieza en LED_OFF_UP, porque así se declaró en el main
	  	  switch(current_state)
	  	  {
		  // Si current_state es 1, esta en el primer estado de la máquina de estados
	  	  case LED_ON_UP:
		  \\Ahora se mira cual es el estado del pulsador, dada por la función get_input
	  		  switch(current_input)
	  		  {
			  //Si el estado del pulsador es bajo,0, cambia el estado del LED a 2, es decir, a LED_ON_DOWN
	  		  case PB_DOWN:
	  			  current_state = LED_ON_DOWN;
				  \\Aumentando la variable cuenta para saber cuantas veces se ha oprimido el botón
	  			  cuenta++;
	  			  break;
	  		  default:
			  \\De lo contrario el estado del LED será LED_ON_UP.
	  			  current_state = LED_ON_UP;
	  		  }
	  		  break;
	          // Se tiene la misma lógica para los casos siguintes.
	  	  case LED_ON_DOWN:
	  		  switch(current_input)
	  		  {
	  		  case PB_DOWN:
	  			  current_state = LED_ON_DOWN;
	  			  break;
	  		  default:
	  			  current_state = LED_OFF_UP;
	  		  }
	  		  break;
	  	  case LED_OFF_UP:
	  		  switch(current_input)
	  		  {
	  		  case PB_DOWN:
	  			  current_state = LED_OFF_DOWN;
	  			  cuenta++;
	  			  break;
	  		  default:
	  			  current_state = LED_OFF_UP;
	  		  }
	  		  break;
	  	  case LED_OFF_DOWN:
	  		  switch(current_input)
	  		  {
	  		  case PB_DOWN:
	  			  current_state = LED_OFF_DOWN;
	  			  break;
	  		  default:
	  			  current_state = LED_ON_UP;
	  		  }
	  		  break;
	  	  }
	  	  set_output(current_state);

/* USER CODE BEGIN 4 */
// La función get input revisa si el pulsador esta en UP o en DOWN.
int8_t get_input(void)
// Se pone el signo ! antes del HAL, para detectar que el pulsador fue oprimido, devolviendo un 0.
{
	if (!HAL_GPIO_ReadPin(B1_GPIO_Port, B1_Pin))
		return PB_DOWN;
	else
		return PB_UP;
}

void set_output(int8_t current_state)
\\La función set_output me define si se debe prender o apagar el LED, según el estado del pulsador.
{
	switch(current_state)
	{
	\\Si se esta en los dos primeros estados de la máquina de estados, el LED se encenderá.
	case LED_ON_UP:
	case LED_ON_DOWN:
		HAL_GPIO_WritePin(LD2_GPIO_Port, LD2_Pin, GPIO_PIN_SET);
		break;
		
	\\De lo contrarió el LED estará apagado.
	case LED_OFF_UP:
	case LED_OFF_DOWN:
		HAL_GPIO_WritePin(LD2_GPIO_Port, LD2_Pin, GPIO_PIN_RESET);
		break;
	}

}
/* USER CODE END 4 */


~~~


Pasos para correr el programa [Aquí](https://github.com/MarianaEstrada/Pasos-para-correr-un-proyecto/blob/master/README.md)

Pasos para correr paso a paso el programa [Aquí](https://github.com/MarianaEstrada/Pasos-para-correr-paso-a-paso-)


### 2. Otra forma de realizar máquinas de estado es aplicando tablas, acontinuación se presentará otra forma de realizar el ejemplo anterior.

* En el main. h del proyecto, ubicado en la carpeta Inc se pone la siguiente información:
~~~


/* USER CODE BEGIN ET */
// Se van a crear dos variables del tipo enum, una para los estados del LED y otra para los estados del pulsador.
// En C++ un tipo de dato enum permite asociar nombres con números, enumerando automáticamente cualquier lista.
enum states {LED_ON_DOWN, LED_ON_UP, LED_OFF_DOWN, LED_OFF_UP} current_state;
enum inputs {PB_DOWN,PB_UP} current_input;
/* USER CODE END ET */

/* USER CODE BEGIN EFP */
// Se crea una variable de un tamaño fijo de 8 bits con signo, para determinar en que posición está el botón
int8_t get_input(void);
//Las funciones ciclo1 y ciclo2 me permiten realizar los cambios del current_state debido a los current_input
void ciclo1 (void);
void ciclo2(void);
/* USER CODE END EFP */
~~~

* En el main. c del proyecto, ubicado en la carpeta Src se pone la siguiente información:

~~~
/
/* USER CODE BEGIN Includes */
//Se van a definir dos variables las cuales tendrán como valor la cantidad de estados que hay para el LED (4) y para el pulsador (2).
#define MAX_CURRENT_STATE 4
#define MAX_CURRENT_INPUT 2
Se crea la tabla de estados que va a ser usada.
typedef void (*transition)();
/* USER CODE END Includes */

//Se crea la tabla de estados 
/* USER CODE BEGIN 0 */

transition state_table[MAX_STATES][MAX_EVENTS] = {
//		PB_DOWN	PB_UP
		{ciclo1,ciclo2},	// LED_ON_UP
		{ciclo1,ciclo2},	// LED_ON_DOWN
		{ciclo1, ciclo2},	// LED_OFF_UP
		{ciclo1, ciclo2}};	// LED_OFF_DOWN

/* USER CODE END 0 */

//Se inicializa la variable current_state en el estado 3 del LED
/* USER CODE BEGIN 2 */
current_state = LED_OFF_UP;
/* USER CODE END 2 */

//Se crea el ciclo while para que se pueda mover dentro de ola tabla de estados dependiendo del estado del pulsador y del LED
 while (1)
  {
  //La variable current_ state depende de la función get_input que se encarga de valor si el pulsador está Down o Up
	  current_input = get_input();
	  if( current_input>=0 && current_input <= MAX_EVENTS &&
	  	  current_state >= 0 && current_state <= MAX_STATES){
		  state_table[current_state][current_input]();
	  }
     }
}

/* USER CODE BEGIN 4 */
// La función get input revisa si el pulsador esta en UP o en DOWN.
int8_t get_input(void)
// Se pone el signo ! antes del HAL, para detectar que el pulsador fue oprimido, devolviendo un 0.
{
	if (!HAL_GPIO_ReadPin(B1_GPIO_Port, B1_Pin)){
		return PB_DOWN;
	}else{
		return PB_UP;
	}
}

//En ciclo 1 sirve cuando el pulsador esta en down haciendo los cambios de estado correspondiente
void ciclo1(){

	if (current_input == PB_DOWN){
		if (current_state == LED_ON_UP){
			current_state = LED_ON_DOWN;
			}else if (current_state == LED_OFF_UP){
			current_state= LED_OFF_DOWN;
		    }
	}
}


//En ciclo 1 sirve cuando el pulsador esta en UP haciendo los cambios de estado correspondiente
void ciclo2(){

	if (current_input == PB_UP){
		if(current_state == LED_ON_DOWN){
			current_state = LED_OFF_UP;
			HAL_GPIO_WritePin(LD2_GPIO_Port, LD2_Pin, RESET);
		}else if (current_state == LED_OFF_DOWN){
			current_state = LED_ON_UP;
			HAL_GPIO_WritePin(LD2_GPIO_Port, LD2_Pin, SET);
		}
	}

}

/* USER CODE END 4 */
~~~
Pasos para correr el programa [Aquí](https://github.com/MarianaEstrada/Pasos-para-correr-un-proyecto/blob/master/README.md)

Pasos para correr paso a paso el programa [Aquí](https://github.com/MarianaEstrada/Pasos-para-correr-paso-a-paso-)

### 3. Otra forma de hacer el antirrebote

Para este programa se va a usar la siguiente máquina de estados:

![ME3](https://github.com/MarianaEstrada/Biblioteca_HAL/blob/master/Imagenes/ME3.PNG)

Esta máquina de estados va a eliminar el donounce del pulsador. Tiene 4 estados:

* Waiting: Espera a que sea oprimido el botón.
* Detected: Si el pulsador despúes de 10ms está en alto se toma como una pulsación, de lo contrario se toma como un rebote y regresa a el estado Waiting.
* Wating for relase: Se espera a que se libere el botón.
* Updata: Se actualizan las variables usadas.


Para solucionar este problema se realiza lo siguiente:

* En la carpeta stm32l4xx_it.c se pone lo siguiente:
~~~
void SysTick_Handler(void)
{
  /* USER CODE BEGIN SysTick_IRQn 0 */
  tick_time ++;
  /* USER CODE END SysTick_IRQn 0 */
  HAL_IncTick();
  /* USER CODE BEGIN SysTick_IRQn 1 */

  /* USER CODE END SysTick_IRQn 1 */
}
~~~

* En el main.h se pone lo siguiente:

~~~
/* USER CODE BEGIN ET */
// Se van a crear dos datos tipo emun uno para los estados y el otro para las transiciones
enum states {espera,detectado,liberacion,actualizacion} current_state;
enum events {tick_e,no_tick_e}current_event;
/* USER CODE END ET */

// Ahora se van a crear las funciones que van a ser usadas a lo largo del programa:
uint64_t tick_time;
int8_t f_espera (void);
void f_detectado (void);
void f_liberacion (void);
void f_actualizacion (void);
void f_error (void);
~~~

* En el main.c se pone lo siguiente :

~~~
/* USER CODE BEGIN Includes */
//Se determinan las dimensiones de la tabla de estados
#define MAX_STATES 4
#define MAX_EVENTS 2
//Se crean dos varibles contador(cuenta el número de pulsaciones) y tiempog (me va a permitir determinar los 10ms)
int contador=0;
int8_t tiempog=0;
//Se crea la tabla de estados que va a ser usada.
typedef void (*transition)();
/* USER CODE END Includes */

/* USER CODE BEGIN 0 */
//Se crea la tabla de transición de estados.
transition state_table[MAX_STATES][MAX_EVENTS] = {
//		TICK_E	NO_TICK_E
		{f_detectado,f_error},	// ESPERA
		{f_detectado,f_espera},	// DETECTADO
		{f_liberacion, f_error},	// LIBERACION
		{f_actualizacion, f_error}};	// ACTUALIZACION
/* USER CODE END 0 */

  //Se inicializa con el estado espera
  /* USER CODE BEGIN 2 */
  current_state= espera;
  /* USER CODE END 2 */


 while (1)
  {
    /* USER CODE END WHILE */
	  
	  
      //Current event toma el valor que le duvuelve la función f_espera para saber si se está en tick_e o en no_tick_e
	  current_event = f_espera() ;
	  if( current_event>=0 && current_event <= MAX_EVENTS &&
	  	 current_state >= 0 && current_state <= MAX_STATES){
	  	 state_table[current_state][current_event]();
    /* USER CODE BEGIN 3 */
       }
  /* USER CODE END 3 */

  }
}

//Finalmente se crean las funciones que son llamadas desde laa tabla de estados
/* USER CODE BEGIN 4 */

void f_error(void){
	current_state=espera;
}

int8_t f_espera (void){

	// Se pone el signo ! antes del HAL, para detectar que el pulsador fue oprimido, devolviendo un 0.
	    if (!HAL_GPIO_ReadPin(B1_GPIO_Port, B1_Pin)){
	    	return tick_e;
		}else{
			return no_tick_e;
		}
}


void f_detectado (void){

	//Se hace el conteo de los 10ms, dado que no se puede modificar la variable tick_time se hace una resta con una variable que se cambia constantemente
	if((tick_time-tiempog)>=10){
	tiempog=tick_time;
	contador++;
	current_state=liberacion;

	}else{
	tiempog=tick_time;
	current_state=espera;
	current_event= no_tick_e;

	}
}

void f_liberacion (void){

	if(HAL_GPIO_ReadPin(B1_GPIO_Port, B1_Pin)){

	current_state= actualizacion ;

	}
}

void f_actualizacion (void){
	current_state=espera;
}

/* USER CODE END 4 */

~~~
Pasos para correr el programa [Aquí](https://github.com/MarianaEstrada/Pasos-para-correr-un-proyecto/blob/master/README.md)

Pasos para correr paso a paso el programa [Aquí](https://github.com/MarianaEstrada/Pasos-para-correr-paso-a-paso-)

### Dimmer

A continuación se presenta la máaquina de estados que va a ser usada:

![ME4](https://github.com/MarianaEstrada/Biblioteca_HAL/blob/master/Imagenes/ME4.PNG)


Esta máquina tiene 3 estados:

* OFF= LED apagado
* DIM = El LED solo enciende a una fracción de su intensidad.
* BRIGHT= El LED enciende a su máxima inttensidad.

El funcionamiento de la máquina de estados es el siguiente:

* OFF: Si desde que se paso a este estado hasta que se vuelve a oprimir el botón el tiempo es mayor a 20 seg el estado del LED va a ser Bright, pero si el pulsador se oprimió antes de los 20 seg el estado del LED pasa a DIM.
* Dim: Si desde que se paso a este estado hasta que se vuelve a oprimir el botón el tiempo es mayor o igual a 4 seg el LED pasa a el estado OFF, de lo contrario si el tiempo es menor a 4seg el LED pasa a estado BRIGHT.
* Bright: Si desde que se paso a este estado hasta que se vuelve a oprimir el botón el tiempo es mayor o igual a 4 seg el LED pasa a el estado DIM , de lo contrario si el tiempo es menor a 4seg el LED pasa a estado OFF.


Para solucionar el problema se debe hacer lo siguiente:

* Primero se debe configurar el timer, de la siguiente forma:

1. En el ioc del proyecto se va la sección Timer.

![paso1T](https://github.com/MarianaEstrada/Biblioteca_HAL/blob/master/Imagenes/paso1T.PNG)

2. Se selecciona el timer que se va a usar, posteriormente en la ventana que aparece se selecciona el canal y se pone la opcion PWM Generation

![paso2T](https://github.com/MarianaEstrada/Biblioteca_HAL/blob/master/Imagenes/paso2T.PNG)

3. En la casilla configuración se selecciona GPIO SETTINGS y en la opción GPIO Pull-Up/Pull-Down se pone la opción Pull-up.

![paso3T](https://github.com/MarianaEstrada/Biblioteca_HAL/blob/master/Imagenes/paso3T.PNG)

4. En la casilla cofiguraciones se selecciona PARAMETER SETTINGS y se configura el valor el prescaler y conter period

![paso4T](https://github.com/MarianaEstrada/Biblioteca_HAL/blob/master/Imagenes/paso4T.PNG)

5. Finalmente se le da clic al siguiente icono:

![paso5T](https://github.com/MarianaEstrada/Biblioteca_HAL/blob/master/Imagenes/paso5T.PNG)

Ahora se añaden los siguientes segmentos de código:


* En la carpeta stm32l4xx_it.c se pone lo siguiente:
~~~
void SysTick_Handler(void)
{
  /* USER CODE BEGIN SysTick_IRQn 0 */
  tick_time ++;
  /* USER CODE END SysTick_IRQn 0 */
  HAL_IncTick();
  /* USER CODE BEGIN SysTick_IRQn 1 */

  /* USER CODE END SysTick_IRQn 1 */
}
~~~


* El el main h


~~~
/* USER CODE BEGIN ET */
// Se van a crear cuatro datos tipo enum, los primeros dos son para el antirrebote y los últimos para el dimmer
enum states {espera,detectado,liberacion,actualizacion} current_state;
enum events {tick_e,no_tick_e}current_event;
enum states2{apagado,atenuado,encendido}current_state_2;
enum events2 {touch}current_event_2;
/* USER CODE END ET */

// Ahora se van a crear las funciones que van a ser usadas a lo largo del programa:
uint64_t tick_time;
int8_t f_espera (void);
void f_detectado (void);
void f_liberacion (void);
void f_actualizacion (void);
void f_error (void);
void  f_apagado  (void);
void f_atenuado (void);
void f_encendido (void);
/* USER CODE END EFP */


~~~

* En el main c

~~~
/* USER CODE BEGIN Includes */
//Se determinan las dimensiones de las tablas de estados
#define MAX_STATES 4
#define MAX_EVENTS 2
#define MAX_STATES2 3
#define MAX_EVENTS2 1
//Se crean cuatro varibles: contador(cuenta el número de pulsaciones),tiempog (me va a permitir determinar los 10ms), timpoc (cuenta las condiciones del dimmer), press (bandera)
int contador=0;
int tiempog=0;
int tiempoc=0;
int press=0;

//Se crean las tablas de estados que va a ser usada.
typedef void (*transition)();
typedef void (*transition2)();
/* USER CODE END Includes */

//Se crea la tabla de transición de estados.
transition state_table[MAX_STATES][MAX_EVENTS] = {
//		TICK_E	NO_TICK_E
		{f_detectado,f_error},	// ESPERA
		{f_detectado,f_espera},	// DETECTADO
		{f_liberacion, f_error},	// LIBERACION
		{f_actualizacion, f_error}};	// ACTUALIZACION

transition2 state_table2[MAX_STATES2][MAX_EVENTS2] = {
//		TOUCH
		{ f_apagado },	// APAGADO
		{f_atenuado},	// ATENUADO
		{f_encendido}};	// ENCENDIDO

/* USER CODE END 0 */

  /* USER CODE BEGIN 2 */
  //Se inicializan las variables 
  current_state= espera;
  current_state_2=apagado;
  current_event_2=touch;
  //Se activa el PWM 
    HAL_TIM_PWM_Start(&htim1,TIM_CHANNEL_4 );
 __HAL_TIM_SET_COMPARE(&htim1,TIM_CHANNEL_4,0);
  /* USER CODE END 2 */


while (1)
  {
    /* USER CODE END WHILE */

    /* USER CODE BEGIN 3 */

	  
	  current_event = f_espera() ;
	  if( current_event>=0 && current_event <= MAX_EVENTS &&
	  	 current_state >= 0 && current_state <= MAX_STATES){
	  	 state_table[current_state][current_event]();



		if(press==1 && current_event_2>=0 && current_event_2<= MAX_EVENTS2 &&
		   current_state_2 >= 0 && current_state_2 <= MAX_STATES2){
			state_table2[current_state_2][current_event_2]();
			}

	  }


    /* USER CODE BEGIN 3 */
       }
  /* USER CODE END 3 */
}

/* USER CODE BEGIN 4 */

void f_error(void){
	current_state=espera;
}

int8_t f_espera (void){

// Se pone el signo ! antes del HAL, para detectar que el pulsador fue oprimido, devolviendo un 0.
    if (!HAL_GPIO_ReadPin(B1_GPIO_Port, B1_Pin)){
	return tick_e;
	}else{
		return no_tick_e;
	}
}


void f_detectado (void){

	//Se hace el conteo de los 10ms, dado que no se puede modificar la variable tick_time se hace una resta con una variable que se cambia constantemente
	if((tick_time-tiempog)>=10){
	// La variable tiempoc es para la segunda máquina de estados
	tiempoc=tick_time-tiempog;
	tiempog=tick_time;
	// Se activa la bandera
	press=1;
	contador++;
	current_state=liberacion;

	}else{

	tiempog=tick_time;
	press=0;
	current_state=espera;
	current_event= no_tick_e;

	}
}

void f_liberacion (void){

	if(HAL_GPIO_ReadPin(B1_GPIO_Port, B1_Pin)){

	current_state= actualizacion ;

	}
}

void f_actualizacion (void){


	current_state=espera;


}

void f_apagado (void){
	
	
   //Se apaga el LED y se determina cual va a ser el siguiente estado dependiendo de tiempoc
	__HAL_TIM_SET_COMPARE(&htim1,TIM_CHANNEL_4,0);
	if (press==1 && tiempoc >= 20000){
	__HAL_TIM_SET_COMPARE(&htim1,TIM_CHANNEL_4,99);
	current_state_2=encendido;
	 current_event_2=touch;
	 // Se desactiva la bandera
	 press=0;


	} else if (press==1 &&  tiempoc < 20000 ){
	__HAL_TIM_SET_COMPARE(&htim1,TIM_CHANNEL_4,5);
	current_state_2=atenuado;
	 current_event_2=touch;
	 // Se desactiva la bandera
	 press=0;

	}
}

void f_atenuado (void){


	//El LED pasa a un estado atenuado y se determina cual va a ser el siguiente estado dependiendo de tiempoc
	if (press==1 &&  tiempoc >=4000){
	    __HAL_TIM_SET_COMPARE(&htim1,TIM_CHANNEL_4,0);
	    current_state_2=apagado;
	    current_event_2=touch;
	    // Se desactiva la bandera
	    press=0;

	} else if (press==1 &&  tiempoc <4000 ){
	    __HAL_TIM_SET_COMPARE(&htim1,TIM_CHANNEL_4,99);
	    current_state_2=encendido;
	     current_event_2=touch;
	     // Se desactiva la bandera
	     press=0;

	}
}

void f_encendido (void){

	
	//El LED pasa a un estado brillo total y se determina cual va a ser el siguiente estado dependiendo de tiempoc
	if (press==1 && tiempoc >=4000){
	    __HAL_TIM_SET_COMPARE(&htim1,TIM_CHANNEL_4,5);
    	    current_state_2=atenuado;
            current_event_2=touch;
            // Se desactiva la bandera
            press=0;

	} else if (press==1 && tiempoc < 4000){
		__HAL_TIM_SET_COMPARE(&htim1,TIM_CHANNEL_4,0);
		current_state_2=apagado;
		current_event_2=touch;
		 // Se desactiva la bandera
		 press=0;
	}
}


/* USER CODE END 4 */


~~~
Pasos para correr el programa [Aquí](https://github.com/MarianaEstrada/Pasos-para-correr-un-proyecto/blob/master/README.md)

Pasos para correr paso a paso el programa [Aquí](https://github.com/MarianaEstrada/Pasos-para-correr-paso-a-paso-)


## Uso del Node-red

Después de haber instalado el programa:

1. Se oprime en el teclado windows + r y ponemos el comando cmd, en la ventana de color negro que aparece pones node-red y esperamos a que se inicialice el programa.
2. En una ventana de Chrome ponemos localhost:1880. Debe aparecer la siguiente ventana:

![node-red](https://github.com/MarianaEstrada/Biblioteca_HAL/blob/master/Imagenes/node-red.PNG)

3. Se va a intalar el puerto serial, para esto se va la parte superior derecha de la pantalla y se oprimen en las 3 rayas.

![node-red-1](https://github.com/MarianaEstrada/Biblioteca_HAL/blob/master/Imagenes/node-red-1.PNG)

4. Se selecciona la opción manage palette

![node-red-2](https://github.com/MarianaEstrada/Biblioteca_HAL/blob/master/Imagenes/node-red-2.PNG)

5. Se va a la pestaña install y se busca node-red-dashboard y se instala.

![node-red-3](https://github.com/MarianaEstrada/Biblioteca_HAL/blob/master/Imagenes/node-red-3.PNG)

6. Siguiendo el mismo proceso anterior se instala también node-red-node-serialport.

7.Con ayuda de los comando que estan al lado izquierdo de la pantalla se realiza la siguiente configuración:

![node-red-6](https://github.com/MarianaEstrada/Biblioteca_HAL/blob/master/Imagenes/node-red-6.PNG).

8. En la esquina superior derecha se oprime el botón Deploy.

![node-red-7](https://github.com/MarianaEstrada/Biblioteca_HAL/blob/master/Imagenes/node-red-7.PNG).

8. Después va ir a la esquina superior derecha y se oprime el símbolo que tiene una flecha y un cuadrado.

![node-red-8](https://github.com/MarianaEstrada/Biblioteca_HAL/blob/master/Imagenes/node-red-8.PNG).

9. Entonces aparecerá la siguiente página.

![node-red-9](https://github.com/MarianaEstrada/Biblioteca_HAL/blob/master/Imagenes/node-red-9.PNG).

10 Finalmente se pone el siguiente código en el STM32CUBEIDE en el mainc:

~~~
/* USER CODE BEGIN Header */
/**
  ******************************************************************************
  * @file           : main.c
  * @brief          : Main program body
  ******************************************************************************
  * @attention
  *
  * <h2><center>&copy; Copyright (c) 2020 STMicroelectronics.
  * All rights reserved.</center></h2>
  *
  * This software component is licensed by ST under BSD 3-Clause license,
  * the "License"; You may not use this file except in compliance with the
  * License. You may obtain a copy of the License at:
  *                        opensource.org/licenses/BSD-3-Clause
  *
  ******************************************************************************
  */
/* USER CODE END Header */

/* Includes ------------------------------------------------------------------*/
#include "main.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */

/* USER CODE END Includes */

/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN PTD */

/* USER CODE END PTD */

/* Private define ------------------------------------------------------------*/
/* USER CODE BEGIN PD */
/* USER CODE END PD */

/* Private macro -------------------------------------------------------------*/
/* USER CODE BEGIN PM */

/* USER CODE END PM */

/* Private variables ---------------------------------------------------------*/
TIM_HandleTypeDef htim2;

UART_HandleTypeDef huart2;

/* USER CODE BEGIN PV */

/* USER CODE END PV */

/* Private function prototypes -----------------------------------------------*/
void SystemClock_Config(void);
static void MX_GPIO_Init(void);
static void MX_USART2_UART_Init(void);
static void MX_TIM2_Init(void);
/* USER CODE BEGIN PFP */

/* USER CODE END PFP */

/* Private user code ---------------------------------------------------------*/
/* USER CODE BEGIN 0 */
uint8_t tx_buff[3] = {"30"};
uint8_t rx_buff[2];

/* USER CODE END 0 */

/**
  * @brief  The application entry point.
  * @retval int
  */
int main(void)
{
  /* USER CODE BEGIN 1 */

  /* USER CODE END 1 */

  /* MCU Configuration--------------------------------------------------------*/

  /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
  HAL_Init();

  /* USER CODE BEGIN Init */

  /* USER CODE END Init */

  /* Configure the system clock */
  SystemClock_Config();

  /* USER CODE BEGIN SysInit */

  /* USER CODE END SysInit */

  /* Initialize all configured peripherals */
  MX_GPIO_Init();
  MX_USART2_UART_Init();
  MX_TIM2_Init();
  /* USER CODE BEGIN 2 */
  HAL_TIM_PWM_Start(&htim2, TIM_CHANNEL_1);

  //HAL_UART_Transmit(&huart2, tx_buff, 3, 10);
  __HAL_UART_ENABLE_IT(&huart2, UART_IT_RXNE);
  /* USER CODE END 2 */

  /* Infinite loop */
  /* USER CODE BEGIN WHILE */
  while (1)
  {
    /* USER CODE END WHILE */

    /* USER CODE BEGIN 3 */
  }
  /* USER CODE END 3 */
}

/**
  * @brief System Clock Configuration
  * @retval None
  */
void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};
  RCC_PeriphCLKInitTypeDef PeriphClkInit = {0};

  /** Initializes the CPU, AHB and APB busses clocks 
  */
  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSI;
  RCC_OscInitStruct.HSIState = RCC_HSI_ON;
  RCC_OscInitStruct.HSICalibrationValue = RCC_HSICALIBRATION_DEFAULT;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_ON;
  RCC_OscInitStruct.PLL.PLLSource = RCC_PLLSOURCE_HSI;
  RCC_OscInitStruct.PLL.PLLM = 1;
  RCC_OscInitStruct.PLL.PLLN = 10;
  RCC_OscInitStruct.PLL.PLLP = RCC_PLLP_DIV7;
  RCC_OscInitStruct.PLL.PLLQ = RCC_PLLQ_DIV2;
  RCC_OscInitStruct.PLL.PLLR = RCC_PLLR_DIV2;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }
  /** Initializes the CPU, AHB and APB busses clocks 
  */
  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_PLLCLK;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_4) != HAL_OK)
  {
    Error_Handler();
  }
  PeriphClkInit.PeriphClockSelection = RCC_PERIPHCLK_USART2;
  PeriphClkInit.Usart2ClockSelection = RCC_USART2CLKSOURCE_PCLK1;
  if (HAL_RCCEx_PeriphCLKConfig(&PeriphClkInit) != HAL_OK)
  {
    Error_Handler();
  }
  /** Configure the main internal regulator output voltage 
  */
  if (HAL_PWREx_ControlVoltageScaling(PWR_REGULATOR_VOLTAGE_SCALE1) != HAL_OK)
  {
    Error_Handler();
  }
}

/**
  * @brief TIM2 Initialization Function
  * @param None
  * @retval None
  */
static void MX_TIM2_Init(void)
{

  /* USER CODE BEGIN TIM2_Init 0 */

  /* USER CODE END TIM2_Init 0 */

  TIM_ClockConfigTypeDef sClockSourceConfig = {0};
  TIM_MasterConfigTypeDef sMasterConfig = {0};
  TIM_OC_InitTypeDef sConfigOC = {0};

  /* USER CODE BEGIN TIM2_Init 1 */

  /* USER CODE END TIM2_Init 1 */
  htim2.Instance = TIM2;
  htim2.Init.Prescaler = 8;
  htim2.Init.CounterMode = TIM_COUNTERMODE_UP;
  htim2.Init.Period = 100;
  htim2.Init.ClockDivision = TIM_CLOCKDIVISION_DIV1;
  htim2.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_DISABLE;
  if (HAL_TIM_Base_Init(&htim2) != HAL_OK)
  {
    Error_Handler();
  }
  sClockSourceConfig.ClockSource = TIM_CLOCKSOURCE_INTERNAL;
  if (HAL_TIM_ConfigClockSource(&htim2, &sClockSourceConfig) != HAL_OK)
  {
    Error_Handler();
  }
  if (HAL_TIM_PWM_Init(&htim2) != HAL_OK)
  {
    Error_Handler();
  }
  sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
  sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
  if (HAL_TIMEx_MasterConfigSynchronization(&htim2, &sMasterConfig) != HAL_OK)
  {
    Error_Handler();
  }
  sConfigOC.OCMode = TIM_OCMODE_PWM1;
  sConfigOC.Pulse = 0;
  sConfigOC.OCPolarity = TIM_OCPOLARITY_HIGH;
  sConfigOC.OCFastMode = TIM_OCFAST_DISABLE;
  if (HAL_TIM_PWM_ConfigChannel(&htim2, &sConfigOC, TIM_CHANNEL_1) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN TIM2_Init 2 */

  /* USER CODE END TIM2_Init 2 */
  HAL_TIM_MspPostInit(&htim2);

}

/**
  * @brief USART2 Initialization Function
  * @param None
  * @retval None
  */
static void MX_USART2_UART_Init(void)
{

  /* USER CODE BEGIN USART2_Init 0 */

  /* USER CODE END USART2_Init 0 */

  /* USER CODE BEGIN USART2_Init 1 */

  /* USER CODE END USART2_Init 1 */
  huart2.Instance = USART2;
  huart2.Init.BaudRate = 115200;
  huart2.Init.WordLength = UART_WORDLENGTH_8B;
  huart2.Init.StopBits = UART_STOPBITS_1;
  huart2.Init.Parity = UART_PARITY_NONE;
  huart2.Init.Mode = UART_MODE_TX_RX;
  huart2.Init.HwFlowCtl = UART_HWCONTROL_NONE;
  huart2.Init.OverSampling = UART_OVERSAMPLING_16;
  huart2.Init.OneBitSampling = UART_ONE_BIT_SAMPLE_DISABLE;
  huart2.AdvancedInit.AdvFeatureInit = UART_ADVFEATURE_NO_INIT;
  if (HAL_UART_Init(&huart2) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN USART2_Init 2 */

  /* USER CODE END USART2_Init 2 */

}

/**
  * @brief GPIO Initialization Function
  * @param None
  * @retval None
  */
static void MX_GPIO_Init(void)
{
  GPIO_InitTypeDef GPIO_InitStruct = {0};

  /* GPIO Ports Clock Enable */
  __HAL_RCC_GPIOC_CLK_ENABLE();
  __HAL_RCC_GPIOH_CLK_ENABLE();
  __HAL_RCC_GPIOA_CLK_ENABLE();
  __HAL_RCC_GPIOB_CLK_ENABLE();

  /*Configure GPIO pin : B1_Pin */
  GPIO_InitStruct.Pin = B1_Pin;
  GPIO_InitStruct.Mode = GPIO_MODE_IT_FALLING;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  HAL_GPIO_Init(B1_GPIO_Port, &GPIO_InitStruct);

  /* EXTI interrupt init*/
  HAL_NVIC_SetPriority(EXTI15_10_IRQn, 0, 0);
  HAL_NVIC_EnableIRQ(EXTI15_10_IRQn);

}

/* USER CODE BEGIN 4 */

/* USER CODE END 4 */

/**
  * @brief  This function is executed in case of error occurrence.
  * @retval None
  */
void Error_Handler(void)
{
  /* USER CODE BEGIN Error_Handler_Debug */
  /* User can add his own implementation to report the HAL error return state */

  /* USER CODE END Error_Handler_Debug */
}

#ifdef  USE_FULL_ASSERT
/**
  * @brief  Reports the name of the source file and the source line number
  *         where the assert_param error has occurred.
  * @param  file: pointer to the source file name
  * @param  line: assert_param error line source number
  * @retval None
  */
void assert_failed(uint8_t *file, uint32_t line)
{ 
  /* USER CODE BEGIN 6 */
  /* User can add his own implementation to report the file name and line number,
     tex: printf("Wrong parameters value: file %s on line %d\r\n", file, line) */
  /* USER CODE END 6 */
}
#endif /* USE_FULL_ASSERT */

/************************ (C) COPYRIGHT STMicroelectronics *****END OF FILE****/


~~~
Y el siguiente fragmento de código en stm32l4xx_it.c

~~~

/* USER CODE BEGIN Header */
/**
  ******************************************************************************
  * @file    stm32l4xx_it.c
  * @brief   Interrupt Service Routines.
  ******************************************************************************
  * @attention
  *
  * <h2><center>&copy; Copyright (c) 2020 STMicroelectronics.
  * All rights reserved.</center></h2>
  *
  * This software component is licensed by ST under BSD 3-Clause license,
  * the "License"; You may not use this file except in compliance with the
  * License. You may obtain a copy of the License at:
  *                        opensource.org/licenses/BSD-3-Clause
  *
  ******************************************************************************
  */
/* USER CODE END Header */

/* Includes ------------------------------------------------------------------*/
#include "main.h"
#include "stm32l4xx_it.h"
#include <stdlib.h>
/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
/* USER CODE END Includes */

/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN TD */

/* USER CODE END TD */

/* Private define ------------------------------------------------------------*/
/* USER CODE BEGIN PD */
 
/* USER CODE END PD */

/* Private macro -------------------------------------------------------------*/
/* USER CODE BEGIN PM */

/* USER CODE END PM */

/* Private variables ---------------------------------------------------------*/
/* USER CODE BEGIN PV */

/* USER CODE END PV */

/* Private function prototypes -----------------------------------------------*/
/* USER CODE BEGIN PFP */

/* USER CODE END PFP */

/* Private user code ---------------------------------------------------------*/
/* USER CODE BEGIN 0 */
bool up = true;
uint8_t dutty = 0;
bool flag = false;
extern uint8_t rx_buff[2];

/* USER CODE END 0 */

/* External variables --------------------------------------------------------*/
extern TIM_HandleTypeDef htim2;
extern UART_HandleTypeDef huart2;
/* USER CODE BEGIN EV */

/* USER CODE END EV */

/******************************************************************************/
/*           Cortex-M4 Processor Interruption and Exception Handlers          */ 
/******************************************************************************/
/**
  * @brief This function handles Non maskable interrupt.
  */
void NMI_Handler(void)
{
  /* USER CODE BEGIN NonMaskableInt_IRQn 0 */

  /* USER CODE END NonMaskableInt_IRQn 0 */
  /* USER CODE BEGIN NonMaskableInt_IRQn 1 */

  /* USER CODE END NonMaskableInt_IRQn 1 */
}

/**
  * @brief This function handles Hard fault interrupt.
  */
void HardFault_Handler(void)
{
  /* USER CODE BEGIN HardFault_IRQn 0 */

  /* USER CODE END HardFault_IRQn 0 */
  while (1)
  {
    /* USER CODE BEGIN W1_HardFault_IRQn 0 */
    /* USER CODE END W1_HardFault_IRQn 0 */
  }
}

/**
  * @brief This function handles Memory management fault.
  */
void MemManage_Handler(void)
{
  /* USER CODE BEGIN MemoryManagement_IRQn 0 */

  /* USER CODE END MemoryManagement_IRQn 0 */
  while (1)
  {
    /* USER CODE BEGIN W1_MemoryManagement_IRQn 0 */
    /* USER CODE END W1_MemoryManagement_IRQn 0 */
  }
}

/**
  * @brief This function handles Prefetch fault, memory access fault.
  */
void BusFault_Handler(void)
{
  /* USER CODE BEGIN BusFault_IRQn 0 */

  /* USER CODE END BusFault_IRQn 0 */
  while (1)
  {
    /* USER CODE BEGIN W1_BusFault_IRQn 0 */
    /* USER CODE END W1_BusFault_IRQn 0 */
  }
}

/**
  * @brief This function handles Undefined instruction or illegal state.
  */
void UsageFault_Handler(void)
{
  /* USER CODE BEGIN UsageFault_IRQn 0 */

  /* USER CODE END UsageFault_IRQn 0 */
  while (1)
  {
    /* USER CODE BEGIN W1_UsageFault_IRQn 0 */
    /* USER CODE END W1_UsageFault_IRQn 0 */
  }
}

/**
  * @brief This function handles System service call via SWI instruction.
  */
void SVC_Handler(void)
{
  /* USER CODE BEGIN SVCall_IRQn 0 */

  /* USER CODE END SVCall_IRQn 0 */
  /* USER CODE BEGIN SVCall_IRQn 1 */

  /* USER CODE END SVCall_IRQn 1 */
}

/**
  * @brief This function handles Debug monitor.
  */
void DebugMon_Handler(void)
{
  /* USER CODE BEGIN DebugMonitor_IRQn 0 */

  /* USER CODE END DebugMonitor_IRQn 0 */
  /* USER CODE BEGIN DebugMonitor_IRQn 1 */

  /* USER CODE END DebugMonitor_IRQn 1 */
}

/**
  * @brief This function handles Pendable request for system service.
  */
void PendSV_Handler(void)
{
  /* USER CODE BEGIN PendSV_IRQn 0 */

  /* USER CODE END PendSV_IRQn 0 */
  /* USER CODE BEGIN PendSV_IRQn 1 */

  /* USER CODE END PendSV_IRQn 1 */
}

/**
  * @brief This function handles System tick timer.
  */
void SysTick_Handler(void)
{
  /* USER CODE BEGIN SysTick_IRQn 0 */

  /* USER CODE END SysTick_IRQn 0 */
  HAL_IncTick();
  /* USER CODE BEGIN SysTick_IRQn 1 */

  /* USER CODE END SysTick_IRQn 1 */
}

/******************************************************************************/
/* STM32L4xx Peripheral Interrupt Handlers                                    */
/* Add here the Interrupt Handlers for the used peripherals.                  */
/* For the available peripheral interrupt handler names,                      */
/* please refer to the startup file (startup_stm32l4xx.s).                    */
/******************************************************************************/

/**
  * @brief This function handles USART2 global interrupt.
  */
void USART2_IRQHandler(void)
{
  /* USER CODE BEGIN USART2_IRQn 0 */

  /* USER CODE END USART2_IRQn 0 */
  HAL_UART_IRQHandler(&huart2);
  /* USER CODE BEGIN USART2_IRQn 1 */
  HAL_UART_Receive_IT(&huart2, rx_buff, 2);

  if (flag){
  HAL_UART_Transmit(&huart2, rx_buff, 2, 10);
  dutty = atoi((const char *)rx_buff);
  __HAL_TIM_SET_COMPARE(&htim2, TIM_CHANNEL_1, dutty);
  /* USER CODE END USART2_IRQn 1 */
  flag=false;
  }else{

  flag=true;

}
}

/**
  * @brief This function handles EXTI line[15:10] interrupts.
  */
void EXTI15_10_IRQHandler(void)
{
  /* USER CODE BEGIN EXTI15_10_IRQn 0 */
	if(up) {
		dutty += 100;
		__HAL_TIM_SET_COMPARE(&htim2, TIM_CHANNEL_1, dutty);
	}else {
		dutty -= 100;
		__HAL_TIM_SET_COMPARE(&htim2, TIM_CHANNEL_1, dutty);
	}

	if(__HAL_TIM_GET_COMPARE(&htim2, TIM_CHANNEL_1) == 1000) {
		up = false;
	}else if(__HAL_TIM_GET_COMPARE(&htim2, TIM_CHANNEL_1) == 0) {
		up = true;
	}
  /* USER CODE END EXTI15_10_IRQn 0 */
  HAL_GPIO_EXTI_IRQHandler(GPIO_PIN_13);
  /* USER CODE BEGIN EXTI15_10_IRQn 1 */

  /* USER CODE END EXTI15_10_IRQn 1 */
}

/* USER CODE BEGIN 1 */

/* USER CODE END 1 */
/************************ (C) COPYRIGHT STMicroelectronics *****END OF FILE****/

~~~

## Tareas

Se va a realizar un programa el cual va a tener dos tareas, cada una de estas tareas va a estar formada por una máquina de estados:

1. La primera máquina de estados es la siguiente:

![ME3](https://github.com/MarianaEstrada/Biblioteca_HAL/blob/master/Imagenes/ME3.PNG)

Esta máquina de estados va a eliminar el donounce del pulsador. Tiene 4 estados:

* Waiting: Espera a que sea oprimido el botón.
* Detected: Si el pulsador despúes de 10ms está en alto se toma como una pulsación, de lo contrario se toma como un rebote y regresa a el estado Waiting.
* Wating for relase: Se espera a que se libere el botón.
* Updata: Se actualizan las variables usadas.

2. La segunda máquina de estados es la siguiente:

![ME4](https://github.com/MarianaEstrada/Biblioteca_HAL/blob/master/Imagenes/ME4.PNG)

Esta máquina tiene 3 estados:

* OFF= LED apagado
* DIM = El LED solo enciende a una fracción de su intensidad.
* BRIGHT= El LED enciende a su máxima intensidad.

El funcionamiento de la máquina de estados es el siguiente:

* OFF: Si desde que se paso a este estado hasta que se vuelve a oprimir el botón el tiempo es mayor a 20 seg el estado del LED va a ser Bright, pero si el pulsador se oprimió antes de los 20 seg el estado del LED pasa a DIM.
* Dim: Si desde que se paso a este estado hasta que se vuelve a oprimir el botón el tiempo es mayor o igual a 4 seg el LED pasa a el estado OFF, de lo contrario si el tiempo es menor a 4seg el LED pasa a estado BRIGHT.
* Bright: Si desde que se paso a este estado hasta que se vuelve a oprimir el botón el tiempo es mayor o igual a 4 seg el LED pasa a el estado DIM , de lo contrario si el tiempo es menor a 4seg el LED pasa a estado OFF.


Para solucionar este problema se realiza lo siguiente:

* Primero se debe configurar el timer, de la siguiente forma:

1. En el ioc del proyecto se va la sección Timer.

![paso1T](https://github.com/MarianaEstrada/Biblioteca_HAL/blob/master/Imagenes/paso1T.PNG)

2. Se selecciona el timer que se va a usar, posteriormente en la ventana que aparece se selecciona el canal y se pone la opcion PWM Generation

![paso2T](https://github.com/MarianaEstrada/Biblioteca_HAL/blob/master/Imagenes/paso2T.PNG)

3. En la casilla configuración se selecciona GPIO SETTINGS y en la opción GPIO Pull-Up/Pull-Down se pone la opción Pull-up.

![paso3T](https://github.com/MarianaEstrada/Biblioteca_HAL/blob/master/Imagenes/paso3T.PNG)

4. En la casilla cofiguraciones se selecciona PARAMETER SETTINGS y se configura el valor el prescaler y conter period

![paso4T](https://github.com/MarianaEstrada/Biblioteca_HAL/blob/master/Imagenes/paso4T.PNG)


* Segundo se van a crear las tareas, para el ejercicio se crearán 2:

1. En el ioc del proyecto se va la sección middleware y se selecciona FREERTOS.

![Img1](https://github.com/MarianaEstrada/Biblioteca_HAL/blob/master/Imagenes/Img1.PNG)

2. En interface se selecciona la opción CMSIS_V2.
![Img2](https://github.com/MarianaEstrada/Biblioteca_HAL/blob/master/Imagenes/Img2.PNG)

3. Finalmente en la opción Tasks and Queues se crean las tareas que van a ser usadas y se le da la prioridad a cada una de ellas.

![Img3](https://github.com/MarianaEstrada/Biblioteca_HAL/blob/master/Imagenes/Img3.PNG)

* Tercero se cambia la fuente de la base del tiempo.

1. En el ioc del proyecto se va la seccionar System core y se selecciona la opción SYS, aquí en la lista de Timebase source se selecciona el Timer 2.

![Img4](https://github.com/MarianaEstrada/Biblioteca_HAL/blob/master/Imagenes/Img4.PNG)

2. Finalmente se le da clic al siguiente icono:

![paso5T](https://github.com/MarianaEstrada/Biblioteca_HAL/blob/master/Imagenes/paso5T.PNG)

Ahora se añaden los siguientes segmentos de código:


* En la carpeta stm32l4xx_it.c se pone lo siguiente:

~~~
void TIM2_IRQHandler(void)
{
  /* USER CODE BEGIN TIM2_IRQn 0 */
	tick_time ++;
  /* USER CODE END TIM2_IRQn 0 */
  HAL_TIM_IRQHandler(&htim2);
  /* USER CODE BEGIN TIM2_IRQn 1 */

  /* USER CODE END TIM2_IRQn 1 */
}
~~~

* En la carpeta main.h se pone lo siguiente:

~~~

/* USER CODE BEGIN ET */
// Se van a crear cuatro datos tipo enum, los primeros dos son para el antirrebote y los últimos para el dimmer
enum states {espera,detectado,liberacion,actualizacion} current_state;
enum events {tick_e,no_tick_e}current_event;
enum states2{apagado,atenuado,encendido}current_state_2;
enum events2 {touch}current_event_2;
/* USER CODE END ET */

/* USER CODE BEGIN EFP */
uint64_t tick_time;
int8_t f_espera (void);
void f_detectado (void);
void f_liberacion (void);
void f_actualizacion (void);
void f_error (void);
void  f_apagado  (void);
void f_atenuado (void);
void f_encendido (void);
/* USER CODE END EFP */
~~~
* En la carpeta main.c se pone lo siguiente:

~~~
/* USER CODE BEGIN Header */
/**
  ******************************************************************************
  * @file           : main.c
  * @brief          : Main program body
  ******************************************************************************
  * @attention
  *
  * <h2><center>&copy; Copyright (c) 2020 STMicroelectronics.
  * All rights reserved.</center></h2>
  *
  * This software component is licensed by ST under BSD 3-Clause license,
  * the "License"; You may not use this file except in compliance with the
  * License. You may obtain a copy of the License at:
  *                        opensource.org/licenses/BSD-3-Clause
  *
  ******************************************************************************
  */
/* USER CODE END Header */

/* Includes ------------------------------------------------------------------*/
#include "main.h"
#include "cmsis_os.h"

/* Private includes ----------------------------------------------------------*/
/* USER CODE BEGIN Includes */
//Se determinan las dimensiones de las tablas de estados
#define MAX_STATES 4
#define MAX_EVENTS 2
#define MAX_STATES2 3
#define MAX_EVENTS2 1
//Se crean cuatro varibles: contador(cuenta el número de pulsaciones),tiempog (me va a permitir determinar los 10ms), timpoc (cuenta las condiciones del dimmer), press (bandera)
int contador=0;
int tiempog=0;
int tiempoc=0;
int press=0;

//Se crean las tablas de estados que va a ser usada.
typedef void (*transition)();
typedef void (*transition2)();
/* USER CODE END Includes */

/* Private typedef -----------------------------------------------------------*/
/* USER CODE BEGIN PTD */

/* USER CODE END PTD */

/* Private define ------------------------------------------------------------*/
/* USER CODE BEGIN PD */
/* USER CODE END PD */

/* Private macro -------------------------------------------------------------*/
/* USER CODE BEGIN PM */

/* USER CODE END PM */

/* Private variables ---------------------------------------------------------*/
TIM_HandleTypeDef htim1;

UART_HandleTypeDef huart2;

/* Definitions for Debounce */
osThreadId_t DebounceHandle;
const osThreadAttr_t Debounce_attributes = {
  .name = "Debounce",
  .priority = (osPriority_t) osPriorityNormal,
  .stack_size = 128 * 4
};
/* Definitions for mymaquinadeesta */
osThreadId_t mymaquinadeestaHandle;
const osThreadAttr_t mymaquinadeesta_attributes = {
  .name = "mymaquinadeesta",
  .priority = (osPriority_t) osPriorityNormal,
  .stack_size = 128 * 4
};
/* USER CODE BEGIN PV */

/* USER CODE END PV */

/* Private function prototypes -----------------------------------------------*/
void SystemClock_Config(void);
static void MX_GPIO_Init(void);
static void MX_USART2_UART_Init(void);
static void MX_TIM1_Init(void);
void Startdebounce(void *argument);
void Startmaquinadeestados(void *argument);

/* USER CODE BEGIN PFP */

/* USER CODE END PFP */

/* Private user code ---------------------------------------------------------*/
/* USER CODE BEGIN 0 */
//Se crea la tabla de transición de estados.
transition state_table[MAX_STATES][MAX_EVENTS] = {
//		TICK_E	NO_TICK_E
		{f_detectado,f_error},	// ESPERA
		{f_detectado,f_espera},	// DETECTADO
		{f_liberacion, f_error},	// LIBERACION
		{f_actualizacion, f_error}};	// ACTUALIZACION

transition2 state_table2[MAX_STATES2][MAX_EVENTS2] = {
//		TOUCH
		{ f_apagado },	// APAGADO
		{f_atenuado},	// ATENUADO
		{f_encendido}};	// ENCENDIDO
/* USER CODE END 0 */

/**
  * @brief  The application entry point.
  * @retval int
  */
int main(void)
{
  /* USER CODE BEGIN 1 */

  /* USER CODE END 1 */

  /* MCU Configuration--------------------------------------------------------*/

  /* Reset of all peripherals, Initializes the Flash interface and the Systick. */
  HAL_Init();

  /* USER CODE BEGIN Init */

  /* USER CODE END Init */

  /* Configure the system clock */
  SystemClock_Config();

  /* USER CODE BEGIN SysInit */

  /* USER CODE END SysInit */

  /* Initialize all configured peripherals */
  MX_GPIO_Init();
  MX_USART2_UART_Init();
  MX_TIM1_Init();
  /* USER CODE BEGIN 2 */

  /* USER CODE END 2 */

  /* Init scheduler */
  osKernelInitialize();

  /* USER CODE BEGIN RTOS_MUTEX */
  /* add mutexes, ... */
  /* USER CODE END RTOS_MUTEX */

  /* USER CODE BEGIN RTOS_SEMAPHORES */
  /* add semaphores, ... */
  /* USER CODE END RTOS_SEMAPHORES */

  /* USER CODE BEGIN RTOS_TIMERS */
  /* start timers, add new ones, ... */
  //Se inicializan las variables
   current_state= espera;
   current_state_2=apagado;
   current_event_2=touch;
   //Se activa el PWM
     HAL_TIM_PWM_Start(&htim1,TIM_CHANNEL_4 );
  __HAL_TIM_SET_COMPARE(&htim1,TIM_CHANNEL_4,0);
  /* USER CODE END RTOS_TIMERS */

  /* USER CODE BEGIN RTOS_QUEUES */
  /* add queues, ... */
  /* USER CODE END RTOS_QUEUES */

  /* Create the thread(s) */
  /* creation of Debounce */
  DebounceHandle = osThreadNew(Startdebounce, NULL, &Debounce_attributes);

  /* creation of mymaquinadeesta */
  mymaquinadeestaHandle = osThreadNew(Startmaquinadeestados, NULL, &mymaquinadeesta_attributes);

  /* USER CODE BEGIN RTOS_THREADS */
  /* add threads, ... */
  /* USER CODE END RTOS_THREADS */

  /* Start scheduler */
  osKernelStart();
 
  /* We should never get here as control is now taken by the scheduler */
  /* Infinite loop */
  /* USER CODE BEGIN WHILE */
  while (1)
  {
    /* USER CODE END WHILE */

    /* USER CODE BEGIN 3 */
  }
  /* USER CODE END 3 */
}

/**
  * @brief System Clock Configuration
  * @retval None
  */
void SystemClock_Config(void)
{
  RCC_OscInitTypeDef RCC_OscInitStruct = {0};
  RCC_ClkInitTypeDef RCC_ClkInitStruct = {0};
  RCC_PeriphCLKInitTypeDef PeriphClkInit = {0};

  /** Initializes the CPU, AHB and APB busses clocks 
  */
  RCC_OscInitStruct.OscillatorType = RCC_OSCILLATORTYPE_HSI;
  RCC_OscInitStruct.HSIState = RCC_HSI_ON;
  RCC_OscInitStruct.HSICalibrationValue = RCC_HSICALIBRATION_DEFAULT;
  RCC_OscInitStruct.PLL.PLLState = RCC_PLL_ON;
  RCC_OscInitStruct.PLL.PLLSource = RCC_PLLSOURCE_HSI;
  RCC_OscInitStruct.PLL.PLLM = 1;
  RCC_OscInitStruct.PLL.PLLN = 10;
  RCC_OscInitStruct.PLL.PLLP = RCC_PLLP_DIV7;
  RCC_OscInitStruct.PLL.PLLQ = RCC_PLLQ_DIV2;
  RCC_OscInitStruct.PLL.PLLR = RCC_PLLR_DIV2;
  if (HAL_RCC_OscConfig(&RCC_OscInitStruct) != HAL_OK)
  {
    Error_Handler();
  }
  /** Initializes the CPU, AHB and APB busses clocks 
  */
  RCC_ClkInitStruct.ClockType = RCC_CLOCKTYPE_HCLK|RCC_CLOCKTYPE_SYSCLK
                              |RCC_CLOCKTYPE_PCLK1|RCC_CLOCKTYPE_PCLK2;
  RCC_ClkInitStruct.SYSCLKSource = RCC_SYSCLKSOURCE_PLLCLK;
  RCC_ClkInitStruct.AHBCLKDivider = RCC_SYSCLK_DIV1;
  RCC_ClkInitStruct.APB1CLKDivider = RCC_HCLK_DIV1;
  RCC_ClkInitStruct.APB2CLKDivider = RCC_HCLK_DIV1;

  if (HAL_RCC_ClockConfig(&RCC_ClkInitStruct, FLASH_LATENCY_4) != HAL_OK)
  {
    Error_Handler();
  }
  PeriphClkInit.PeriphClockSelection = RCC_PERIPHCLK_USART2;
  PeriphClkInit.Usart2ClockSelection = RCC_USART2CLKSOURCE_PCLK1;
  if (HAL_RCCEx_PeriphCLKConfig(&PeriphClkInit) != HAL_OK)
  {
    Error_Handler();
  }
  /** Configure the main internal regulator output voltage 
  */
  if (HAL_PWREx_ControlVoltageScaling(PWR_REGULATOR_VOLTAGE_SCALE1) != HAL_OK)
  {
    Error_Handler();
  }
}

/**
  * @brief TIM1 Initialization Function
  * @param None
  * @retval None
  */
static void MX_TIM1_Init(void)
{

  /* USER CODE BEGIN TIM1_Init 0 */

  /* USER CODE END TIM1_Init 0 */

  TIM_MasterConfigTypeDef sMasterConfig = {0};
  TIM_OC_InitTypeDef sConfigOC = {0};
  TIM_BreakDeadTimeConfigTypeDef sBreakDeadTimeConfig = {0};

  /* USER CODE BEGIN TIM1_Init 1 */

  /* USER CODE END TIM1_Init 1 */
  htim1.Instance = TIM1;
  htim1.Init.Prescaler = 15;
  htim1.Init.CounterMode = TIM_COUNTERMODE_UP;
  htim1.Init.Period = 99;
  htim1.Init.ClockDivision = TIM_CLOCKDIVISION_DIV1;
  htim1.Init.RepetitionCounter = 0;
  htim1.Init.AutoReloadPreload = TIM_AUTORELOAD_PRELOAD_DISABLE;
  if (HAL_TIM_PWM_Init(&htim1) != HAL_OK)
  {
    Error_Handler();
  }
  sMasterConfig.MasterOutputTrigger = TIM_TRGO_RESET;
  sMasterConfig.MasterOutputTrigger2 = TIM_TRGO2_RESET;
  sMasterConfig.MasterSlaveMode = TIM_MASTERSLAVEMODE_DISABLE;
  if (HAL_TIMEx_MasterConfigSynchronization(&htim1, &sMasterConfig) != HAL_OK)
  {
    Error_Handler();
  }
  sConfigOC.OCMode = TIM_OCMODE_PWM1;
  sConfigOC.Pulse = 0;
  sConfigOC.OCPolarity = TIM_OCPOLARITY_HIGH;
  sConfigOC.OCFastMode = TIM_OCFAST_DISABLE;
  sConfigOC.OCIdleState = TIM_OCIDLESTATE_RESET;
  sConfigOC.OCNIdleState = TIM_OCNIDLESTATE_RESET;
  if (HAL_TIM_PWM_ConfigChannel(&htim1, &sConfigOC, TIM_CHANNEL_4) != HAL_OK)
  {
    Error_Handler();
  }
  sBreakDeadTimeConfig.OffStateRunMode = TIM_OSSR_DISABLE;
  sBreakDeadTimeConfig.OffStateIDLEMode = TIM_OSSI_DISABLE;
  sBreakDeadTimeConfig.LockLevel = TIM_LOCKLEVEL_OFF;
  sBreakDeadTimeConfig.DeadTime = 0;
  sBreakDeadTimeConfig.BreakState = TIM_BREAK_DISABLE;
  sBreakDeadTimeConfig.BreakPolarity = TIM_BREAKPOLARITY_HIGH;
  sBreakDeadTimeConfig.BreakFilter = 0;
  sBreakDeadTimeConfig.Break2State = TIM_BREAK2_DISABLE;
  sBreakDeadTimeConfig.Break2Polarity = TIM_BREAK2POLARITY_HIGH;
  sBreakDeadTimeConfig.Break2Filter = 0;
  sBreakDeadTimeConfig.AutomaticOutput = TIM_AUTOMATICOUTPUT_DISABLE;
  if (HAL_TIMEx_ConfigBreakDeadTime(&htim1, &sBreakDeadTimeConfig) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN TIM1_Init 2 */

  /* USER CODE END TIM1_Init 2 */
  HAL_TIM_MspPostInit(&htim1);

}

/**
  * @brief USART2 Initialization Function
  * @param None
  * @retval None
  */
static void MX_USART2_UART_Init(void)
{

  /* USER CODE BEGIN USART2_Init 0 */

  /* USER CODE END USART2_Init 0 */

  /* USER CODE BEGIN USART2_Init 1 */

  /* USER CODE END USART2_Init 1 */
  huart2.Instance = USART2;
  huart2.Init.BaudRate = 115200;
  huart2.Init.WordLength = UART_WORDLENGTH_8B;
  huart2.Init.StopBits = UART_STOPBITS_1;
  huart2.Init.Parity = UART_PARITY_NONE;
  huart2.Init.Mode = UART_MODE_TX_RX;
  huart2.Init.HwFlowCtl = UART_HWCONTROL_NONE;
  huart2.Init.OverSampling = UART_OVERSAMPLING_16;
  huart2.Init.OneBitSampling = UART_ONE_BIT_SAMPLE_DISABLE;
  huart2.AdvancedInit.AdvFeatureInit = UART_ADVFEATURE_NO_INIT;
  if (HAL_UART_Init(&huart2) != HAL_OK)
  {
    Error_Handler();
  }
  /* USER CODE BEGIN USART2_Init 2 */

  /* USER CODE END USART2_Init 2 */

}

/**
  * @brief GPIO Initialization Function
  * @param None
  * @retval None
  */
static void MX_GPIO_Init(void)
{
  GPIO_InitTypeDef GPIO_InitStruct = {0};

  /* GPIO Ports Clock Enable */
  __HAL_RCC_GPIOC_CLK_ENABLE();
  __HAL_RCC_GPIOH_CLK_ENABLE();
  __HAL_RCC_GPIOA_CLK_ENABLE();
  __HAL_RCC_GPIOB_CLK_ENABLE();

  /*Configure GPIO pin Output Level */
  HAL_GPIO_WritePin(LD2_GPIO_Port, LD2_Pin, GPIO_PIN_RESET);

  /*Configure GPIO pin : B1_Pin */
  GPIO_InitStruct.Pin = B1_Pin;
  GPIO_InitStruct.Mode = GPIO_MODE_IT_FALLING;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  HAL_GPIO_Init(B1_GPIO_Port, &GPIO_InitStruct);

  /*Configure GPIO pin : LD2_Pin */
  GPIO_InitStruct.Pin = LD2_Pin;
  GPIO_InitStruct.Mode = GPIO_MODE_OUTPUT_PP;
  GPIO_InitStruct.Pull = GPIO_NOPULL;
  GPIO_InitStruct.Speed = GPIO_SPEED_FREQ_LOW;
  HAL_GPIO_Init(LD2_GPIO_Port, &GPIO_InitStruct);

}

/* USER CODE BEGIN 4 */
	void f_error(void){
	  	current_state=espera;
	  }

	  int8_t f_espera (void){

	  // Se pone el signo ! antes del HAL, para detectar que el pulsador fue oprimido, devolviendo un 0.
	      if (!HAL_GPIO_ReadPin(B1_GPIO_Port, B1_Pin)){
	  	return tick_e;
	  	}else{
	  		return no_tick_e;
	  	}
	  }


	  void f_detectado (void){

	  	//Se hace el conteo de los 10ms, dado que no se puede modificar la variable tick_time se hace una resta con una variable que se cambia constantemente
	  	if((tick_time-tiempog)>=10){
	  	// La variable tiempoc es para la segunda máquina de estados
	  	tiempoc=tick_time-tiempog;
	  	tiempog=tick_time;
	  	// Se activa la bandera
	  	press=1;
	  	contador++;
	  	current_state=liberacion;

	  	}else{

	  	tiempog=tick_time;
	  	press=0;
	  	current_state=espera;
	  	current_event= no_tick_e;

	  	}
	  }

	  void f_liberacion (void){

	  	if(HAL_GPIO_ReadPin(B1_GPIO_Port, B1_Pin)){

	  	current_state= actualizacion ;

	  	}
	  }

	  void f_actualizacion (void){


	  	current_state=espera;


	  }




	  void f_apagado (void){


	     //Se apaga el LED y se determina cual va a ser el siguiente estado dependiendo de tiempoc
	  	__HAL_TIM_SET_COMPARE(&htim1,TIM_CHANNEL_4,0);
	  	if (press==1 && tiempoc >= 20000){
	  	__HAL_TIM_SET_COMPARE(&htim1,TIM_CHANNEL_4,99);
	  	current_state_2=encendido;
	  	 current_event_2=touch;
	  	 // Se desactiva la bandera
	  	 press=0;


	  	} else if (press==1 &&  tiempoc < 20000 ){
	  	__HAL_TIM_SET_COMPARE(&htim1,TIM_CHANNEL_4,5);
	  	current_state_2=atenuado;
	  	 current_event_2=touch;
	  	 // Se desactiva la bandera
	  	 press=0;

	  	}
	  }

	  void f_atenuado (void){


	  	//El LED pasa a un estado atenuado y se determina cual va a ser el siguiente estado dependiendo de tiempoc
	  	if (press==1 &&  tiempoc >=4000){
	  	    __HAL_TIM_SET_COMPARE(&htim1,TIM_CHANNEL_4,0);
	  	    current_state_2=apagado;
	  	    current_event_2=touch;
	  	    // Se desactiva la bandera
	  	    press=0;

	  	} else if (press==1 &&  tiempoc <4000 ){
	  	    __HAL_TIM_SET_COMPARE(&htim1,TIM_CHANNEL_4,99);
	  	    current_state_2=encendido;
	  	     current_event_2=touch;
	  	     // Se desactiva la bandera
	  	     press=0;

	  	}
	  }

	  void f_encendido (void){


	  	//El LED pasa a un estado brillo total y se determina cual va a ser el siguiente estado dependiendo de tiempoc
	  	if (press==1 && tiempoc >=4000){
	  	    __HAL_TIM_SET_COMPARE(&htim1,TIM_CHANNEL_4,5);
	      	    current_state_2=atenuado;
	              current_event_2=touch;
	              // Se desactiva la bandera
	              press=0;

	  	} else if (press==1 && tiempoc < 4000){
	  		__HAL_TIM_SET_COMPARE(&htim1,TIM_CHANNEL_4,0);
	  		current_state_2=apagado;
	  		current_event_2=touch;
	  		 // Se desactiva la bandera
	  		 press=0;
	  	}
	  }
/* USER CODE END 4 */

/* USER CODE BEGIN Header_Startdebounce */
/**
  * @brief  Function implementing the Debounce thread.
  * @param  argument: Not used
  * @retval None
  */
/* USER CODE END Header_Startdebounce */

//Se crea la primera tarea la cual esta relacionada con el antirebote del pulsador.
void Startdebounce(void *argument)
{
  /* USER CODE BEGIN 5 */
  /* Infinite loop */
  for(;;)
  {
	  current_event = f_espera() ;
	 	  if( current_event>=0 && current_event <= MAX_EVENTS &&
	 	  	 current_state >= 0 && current_state <= MAX_STATES){
	 	  	 state_table[current_state][current_event]();
    osDelay(10);
  }
  /* USER CODE END 5 */
}
}

/* USER CODE BEGIN Header_Startmaquinadeestados */
/**
* @brief Function implementing the mymaquinadeesta thread.
* @param argument: Not used
* @retval None
*/
/* USER CODE END Header_Startmaquinadeestados */
void Startmaquinadeestados(void *argument)
{

//Se crea la segunda tarea, la cual esta relacionada con la iluminación del LED
  /* USER CODE BEGIN Startmaquinadeestados */
  /* Infinite loop */
  for(;;)
  {
  
		if(press==1 && current_event_2>=0 && current_event_2<= MAX_EVENTS2 &&
			   current_state_2 >= 0 && current_state_2 <= MAX_STATES2){
				state_table2[current_state_2][current_event_2]();
				}

	  }
    osDelay(10);
  }
  /* USER CODE END Startmaquinadeestados */


 /**
  * @brief  Period elapsed callback in non blocking mode
  * @note   This function is called  when TIM2 interrupt took place, inside
  * HAL_TIM_IRQHandler(). It makes a direct call to HAL_IncTick() to increment
  * a global variable "uwTick" used as application time base.
  * @param  htim : TIM handle
  * @retval None
  */
void HAL_TIM_PeriodElapsedCallback(TIM_HandleTypeDef *htim)
{
  /* USER CODE BEGIN Callback 0 */

  /* USER CODE END Callback 0 */
  if (htim->Instance == TIM2) {
    HAL_IncTick();
  }
  /* USER CODE BEGIN Callback 1 */

  /* USER CODE END Callback 1 */
}

/**
  * @brief  This function is executed in case of error occurrence.
  * @retval None
  */
void Error_Handler(void)
{
  /* USER CODE BEGIN Error_Handler_Debug */
  /* User can add his own implementation to report the HAL error return state */

  /* USER CODE END Error_Handler_Debug */
}

#ifdef  USE_FULL_ASSERT
/**
  * @brief  Reports the name of the source file and the source line number
  *         where the assert_param error has occurred.
  * @param  file: pointer to the source file name
  * @param  line: assert_param error line source number
  * @retval None
  */
void assert_failed(uint8_t *file, uint32_t line)
{ 
  /* USER CODE BEGIN 6 */
  /* User can add his own implementation to report the file name and line number,
     tex: printf("Wrong parameters value: file %s on line %d\r\n", file, line) */
  /* USER CODE END 6 */
}
#endif /* USE_FULL_ASSERT */

/************************ (C) COPYRIGHT STMicroelectronics *****END OF FILE****/



~~~










