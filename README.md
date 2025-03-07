# STM32_GPIO
The previous version of my GPIO base class had a different approach of interrupt handling and GPIO configuration.
The GPIO base class had a static list which containing the event callback reference for each GPIO.

![image](https://github.com/user-attachments/assets/1de6a56d-1cda-4771-93ff-fb419be9082e)

![image](https://github.com/user-attachments/assets/eb970915-6ccf-4938-8dcf-adeee92ac977)

The initialization function of the GPIO objects was an "empty" function because normaly the GPIO initalization is done by MX_GPIO_Init function which is generated by CubeMX during project configuration/code generation process.

# New approach:

This new GPIO base class (v2.0) has no callback tracking so you need to handle them in the global interrupt routine.
The previous version required lot of extra codes without almost any benefits: 

-generic ISR class definition for storing gpio objects and their cb routines in a static list: ISRbase<<Din></Din> Din::ISR_LIST;

-when interrupt was fired find the callback function for the realted gpio 

-etc.

![image](https://github.com/user-attachments/assets/ee47a4dd-e967-464c-8ae0-5a5c83491ee7)

You can define as many GPIO child class with preconfigured GPIO settings as you want so you no need to use anymore CubeMX for GPIO configuration for you own GPIOs.

Typical digital input with falling edge detection and pull up resistor
![image](https://github.com/user-attachments/assets/f78acf83-a7e6-42a9-a5ac-2900e0f1428b)


Then init function calls the HAL_GPIO_Init(_Port, &_GPIOx); with PORT and other necessary parameteres defined in constructor.

GPIO object definition and usage:

![image](https://github.com/user-attachments/assets/535032b2-27b4-4348-a60c-9873643d549e)

![image](https://github.com/user-attachments/assets/f4fb55df-636a-4e63-b5b4-052ea0c84e90)

The project description is also changed, until now I let everything to be managed by CubeIDE.
It was good more or less but Cmakelists is doing a way much better job... 

Previous version is located here:
https://github.com/czagaadam/STM32CPP/tree/main/CPPLib/GPIObase

Feel free to choose which approach is more suitable for you!
