# Biblioteca HAL

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

1. La primera máquina de estados que se va a usar es la siguiente:

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


2. Otra forma de realizar máquinas de estado es aplicando tablas, acontinuación se presentará otra forma de realizar el ejemplo anterior.

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

3. Circuito antirrebote

Para este programa se va a usar la siguiente máquina de estados:

![ME3](https://github.com/MarianaEstrada/Biblioteca_HAL/blob/master/Imagenes/ME3.PNG)

Esta máquina de estados lo que busca es que la pulsación solo se cuente cada vez que sea mayor a 10ms. Acontinuación se presenta el código usado:

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


Pasos para correr el programa [Aquí](https://github.com/MarianaEstrada/Pasos-para-correr-un-proyecto/blob/master/README.md)

Pasos para correr paso a paso el programa [Aquí](https://github.com/MarianaEstrada/Pasos-para-correr-paso-a-paso-)
