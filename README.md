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

Acontinuación se presentarán algunos códigos para evitar el rebote en los pulsadores.

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
/* USER CODE END EFP */
~~~

* En el main. c del proyecto, ubicado en la carpeta Src se pone la siguiente información:

~~~
/
/* USER CODE BEGIN Includes */
//Se van a definir dos variables las cuales tendrán como valor la cantidad de estados que hay para el LED y para el pulsador.
#define MAX_CURRENT_STATE 4
#define MAX_CURRENT_INPUT 2
typedef void (*transition)();
/* USER CODE END Includes */

/* USER CODE BEGIN 0 */

//Cuando se está en los dos primeros estados el LED se va a encender
void LED_1(void){
	HAL_GPIO_WritePin(LD2_GPIO_Port, LD2_Pin, GPIO_PIN_SET);
}
//Mientras que cuando está en los otros dos estados el estará apagado.
void LED_2(void)
{
	HAL_GPIO_WritePin(LD2_GPIO_Port, LD2_Pin, GPIO_PIN_RESET);
}

//Estas funciones me permiten moverme dentro de la tabla de estados.
void PB_1(void)
{
current_state=LED_OFF_UP;
cuenta++;
}

void PB_2(void)
{
current_state=LED_ON_DOWN;
}

void PB_3(void)
{
current_state=LED_ON_UP;
}

void PB_4(void)
{
current_state=LED_OFF_DOWN;
}

//La tabla de estados se refiere a el estado donde se encuentra PB_1... y decir si el LED se enciende o se apaga
transition state_table[MAX_CURRENT_STATE][MAX_CURRENT_INPUT]={
//PULSADOR ARRIBA PB1
{LED_1,PB_1},
{LED_1,PB_2},
{LED_2, PB_3},
{LED_2, PB_4}} ;


/* USER CODE END 0 */
// Se inicializa el estado acutual en el tercer estado LED_OFF_UP
  /* USER CODE BEGIN 2 */
  current_state = LED_OFF_UP ;
  /* USER CODE END 2 */

    while (1)
  {
    /* USER CODE END WHILE */
         // Se lee el estado de la entrada con la función get_input
	  current_input = get_input();
          // Se va a la tabla de eestados para ejecutar lo pedido 
	  if ((current_input>= 0 ) && (current_input < MAX_CURRENT_INPUT)
			  && (current_state >= 0) && (current_state < MAX_CURRENT_STATE))
			  {
	  state_table[current_state][current_input]();
	  }

  }
  /* USER CODE END 3 */
}

/* USER CODE BEGIn 4 */
int8_t get_input(void)
// Se pone el signo ! antes del HAL, para detectar que el pulsador fue oprimido, devolviendo un 0.
{
	if (!HAL_GPIO_ReadPin(B1_GPIO_Port, B1_Pin)){

		return PB_DOWN;
	}else{
		return PB_UP;

	}
}




/* USER CODE END 4 */


~~~

3. Circuito antirrebote

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
uint8_t aprobacion=0;
uint8_t estado_led=0;
uint64_t tick_time;
#define MAX_CURRENT_STATE 5
#define MAX_CURRENT_INPUT 2
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
UART_HandleTypeDef huart2;

/* USER CODE BEGIN PV */

/* USER CODE END PV */

/* Private function prototypes -----------------------------------------------*/
void SystemClock_Config(void);
static void MX_GPIO_Init(void);
static void MX_USART2_UART_Init(void);
/* USER CODE BEGIN PFP */

/* USER CODE END PFP */

/* Private user code ---------------------------------------------------------*/
/* USER CODE BEGIN 0 */

void STATE_1(void){
	//Estado inicio
	HAL_GPIO_WritePin(LD2_GPIO_Port, LD2_Pin, GPIO_PIN_RESET);
}

void STATE_2(void){

	{}
}

void STATE_3(void){
	current_input=get_imput();
	if(current_input=PB_DOWN && tick_time==10000){
	aprobacion=1;

	}
}

void STATE_4(void){

	if( aprobacion=1){
	   estado_led= HAL_GPIO_ReadPin(LD2_GPIO_Port, LD2_Pin);
	   if(estado_led=1){
	HAL_GPIO_WritePin(LD2_GPIO_Port, LD2_Pin, GPIO_PIN_RESET);
	   }else{
		   HAL_GPIO_WritePin(LD2_GPIO_Port, LD2_Pin, GPIO_PIN_SET);
	   }
	}
}

void STATE_5(void){
	aprobacion=0 ;
	tick_time=0;
}


void PB_1(void)
{
current_state=STATE_2;
}

void PB_2(void)
{
current_state=STATE_3;
}

void PB_3(void)
{
current_state=STATE_4;
}

void PB_4(void)
{
current_state=STATE_5;
}

void PB_4(void)
{
current_state=STATE_1;
}


transition state_table[MAX_CURRENT_STATE][MAX_CURRENT_INPUT]={
//PULSADOR ARRIBA PB1
{STATE_1,PB_1},
{STATE_2,PB_2},
{STATE_3, PB_3},
{STATE_4, PB_4},
{STATE_5, PB_4}} ;
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
  /* USER CODE BEGIN 2 */
  current_state=Inicio;
  /* USER CODE END 2 */

  /* Infinite loop */
  /* USER CODE BEGIN WHILE */
  while (1)
  {
    /* USER CODE END WHILE */

     state_table[current_state][current_input]();



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
int8_t get_input(void)
// Se pone el signo ! antes del HAL, para detectar que el pulsador fue oprimido, devolviendo un 0.
{
	if (!HAL_GPIO_ReadPin(B1_GPIO_Port, B1_Pin)){
		cuenta ++;
		return PB_DOWN;
	}else{
		return PB_UP;

	}
}

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





Pasos para correr el programa [Aquí](https://github.com/MarianaEstrada/Pasos-para-correr-un-proyecto/blob/master/README.md)

Pasos para correr paso a paso el programa [Aquí](https://github.com/MarianaEstrada/Pasos-para-correr-paso-a-paso-)
