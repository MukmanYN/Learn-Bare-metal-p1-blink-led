 
The first part exercise will be turning on an LED from scratch, here I am using the NUCLEO-F446RE board.  
the goal is to blink the LED2 on pin PA5 on the board 
 
## STM32F446 board schematic
![board schematic](images/Picture1.png)

You will need your MCU chips datasheet and reference manual, with these keys you can unlock the world of BareMetal 
1. First you need to find your MCU block diagram on the datasheet, here’s the STM32F446xC/E

## STM32F446 Block Diagram
![STM32F446 Block Diagram](images/Picture2.png)
 
Since we are trying to turn on LED2 from the GPIO pin A5, we look for GPIOA port, what bus it’s connected to, 
you’ll notice GPIOA port is connected to AHB1 bus
2. Then go to reference manual, Memory map and register boundary addresses to find the address of the AHB1 bus

## AHB1 Memory Map
![AHB1 Memory Map](images/Picture3.png)

 

Here we will find the address of the AHB1 bus (0x40020000), now we can start programming

3. Now we create a project with our MCU, and make sure the project is empty
# finding mcu
![finding mcu](images/Picture4.png) ![](images/Picture5.png)
  
You will find an empty main file 
![](images/Picture6.png)
 
4.next you will create a base header file to define all your base addresses you need for your project.  Looking back at the memory map and register boundaries, you define the AHB1 peripheral address, GPIOA peripheral address, and RCC peripheral address all with their OFFSETs to make sure you are referring to the right address. 
 # Register Boundary Addresses
 ![Register Boundary Addresses](images/Picture7.png)
 ![](images/Picture8.png)
I like to do it this way (you can just define peripheral address as is)
5. next you create a header file as GPIO.h, then define the registers you want to read and right to, according to
To the reference manual
![](images/Picture9.png)
 
Here we need the GPIO mode and the Output data register why? To turn GPIO mode to then output data to the
Appropriate pin
 ![](images/Picture10.png)
 ![](images/Picture11.png)
Here In C, we are saying that: (volatile int *) → cast the address to a pointer, then dereference it to read/write the
Data inside.  “Treat this address as a pointer to a volatile int, then read/write the int located there”. then you
Define your Output Data register
 ![](images/Picture12.png)
 
Lastly RCC enables register, to have the bus that we need to use the GPIO, in your case the AHB1 bus
 
 ![](images/Picture13.png)
Lastly you initialize the gpio initialize functions 
 

 
6. next create a gpio.c file to initiialize the GPIO perihperial , in the RCC_AHB1ENNR register we see 
Where what bit Enables  GPIO port we need (GPIOA)
 
 ![](images/Picture14.png)

 

7.next we set the pin we want to talk to, ( in my case pin 5). To the mode type ( General Purpose
Output mode _ so bits 01 on the GPIOA_MODER register MODER5 ( bits 11 , and 10)

![](images/Picture15.png)
 


8. Then to actually write to the GPIOA_ODR register to output data (voltage) to turn on LD2

![](images/Picture17.png)
 
Then you write to the the function to turn it on; after initialzing

 ![](images/Picture18.png)
 
And you clear the bit to turn off 

![](images/Picture19.png)
 
9. lastly you call the functions in the main function, add a delay function to see the diffreence (.5s)
 
![](images/Picture20.png)
10. run the code and check your LED here is mine -   https://www.youtube.com/shorts/QAdSiPyPYvU
You should have you LED blinking comleting the first project of bare metal programming congratulations! Now for project 2



 
 
 

 




 

