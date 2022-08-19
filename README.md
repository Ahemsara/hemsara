#MICROCOMPTERSLAB4
micro
Content
•	Objective
•	Apparatus
•	Introduction
•	Task
•	Operation Table
•	Breadboard Implementation
•	The PCB Design
•	Code
•	Results
•	Discussion
•	References
Objective
This lab's main goal is for you to create a simple water level control system for a water tank utilizing your understanding of interrupts and other PIC16f877a programming techniques from all the previous labs we covered in the course.
Apparatus
•	PCB board
•	Breadboard power supply module
•	DC motors
•	Wave distance sensors
•	 Wires
•	Capacitors(470µF , 47µF
•	Crystal oscillator
•	Pic adaptor
•	PIC16F877A Microcontroller
•	PICKIT 3
•	 Diode
•	Soldering iron


Introduction
The objective of the project is to create a system that automatically controls the water level. A PIC microcontroller and IR sensors are used in the recommended system to detect the water level within a tank and automatically turn on or off two DC motors based on that information.

The trials' findings show that the proposed system is effective in determining the water level and turning the water pump ON or OFF in response to the water level.

Electronic circuits known as PIC microcontrollers (Programmable Interface Controllers) can be configured to perform a variety of activities. They can be set up to perform a number of functions, such as controlling a production line and acting as timers. A PIC microcontroller can be programmed using a MP lab software. We used assembly language to program PIC Microcontrollers.

PCB DESIGN
![image](https://user-images.githubusercontent.com/111573678/185655296-e62c4bda-65b7-438f-8605-a1ad9c0d359c.png)
![image](https://user-images.githubusercontent.com/111573678/185655461-4b071362-ff39-4d19-8f7c-71519a97598f.png)
![image](https://user-images.githubusercontent.com/111573678/185655576-b7c45bb3-5511-4f4f-b28f-1793ef47bfa0.png)

![image](https://user-images.githubusercontent.com/111573678/185657725-db044a76-5e92-4e19-93c9-494823553026.png)
![Screenshot (158)](https://user-images.githubusercontent.com/111573678/185658530-3ce6e2b5-0a99-4fc9-821d-828dcbd28f27.png)

Code 



// PIC16LF877A Configuration Bit Settings

// 'C' source line config statements

// CONFIG
#pragma config FOSC = HS        // Oscillator Selection bits (HS oscillator)
#pragma config WDTE = OFF       // Watchdog Timer Enable bit (WDT disabled)
#pragma config PWRTE = OFF      // Power-up Timer Enable bit (PWRT disabled)
#pragma config BOREN = OFF      // Brown-out Reset Enable bit (BOR disabled)
#pragma config LVP = OFF        // Low-Voltage (Single-Supply) In-Circuit Serial Programming Enable bit (RB3 is digital I/O, HV on MCLR must be used for programming)
#pragma config CPD = OFF        // Data EEPROM Memory Code Protection bit (Data EEPROM code protection off)
#pragma config WRT = OFF        // Flash Program Memory Write Enable bits (Write protection off; all program memory may be written to by EECON control)
#pragma config CP = OFF         // Flash Program Memory Code Protection bit (Code protection off)

// #pragma config statements should precede project file includes.
// Use project enums instead of #define for ON and OFF.

#include <xc.h>

#define _XTAL_FREQ 20000000

void __interrupt() isr(void) {
    
    if(RB0 == 1 && RB1 == 1 && RB2 == 1){   //case 3
        RC0 = 0; //Motor 1 OFF
        RC1 = 1; //Motor 2 ON
        __delay_ms(500);
        RC1 = 0; //Motor 2 OFF
    }
    INTF = 0;
}

void main(void){
    
    GIE = 1;  
    INTE = 1; 
    INTF = 0;  
    INTEDG = 1;
    
    TRISB0 = 1;//Sensor 03
    TRISB1 = 1;//Sensor 01
    TRISB2 = 1;//Sensor 02
    
    TRISC0 = 0;//Motor 1
    TRISC1 = 0;//Motor 2 
    
    PORTC = 0X00; //PortC all pins configured as output 
      
    while(1){        
        if(RB1 == 1 && RB2 == 0 && RB0 == 0){ //case 1
            RC0 = 1; //Motor 1 ON
            RC1 = 0; //Motor 2 OFF
        }
        if(RB1 == 1 && RB2 == 1 && RB0 == 0){ //case 2
            RC0 = 1; //Motor 1 ON
            RC1 = 0; //Motor 2 OFF
        }
        //else if(RB2 == 0){
            //RC0 = 0;
            //RC1 = 0;
        //}
    }
    return;
}

Discussion

In this system, there are two DC motors, one of which empties the tank of water and the other of which pumps water into it. Three motion sensors also keep an eye on the water level in the tank. The motion sensors generate a logic low signal when they identify an item. The first time, according to the preceding table 1, when switch one is on and switches two and three are off, motor one pumps water into the tank while motor two is off. When switches one and two are turned on and switch three is turned off, motor one pumps water into the tank while motor two is turned of.
When all three switches are turned on, motor two sucks water from the water tank for 500 milliseconds before turning off, and motor one is turned off. The microcontroller generates a logic high or low depending on the water level to turn the switches on and off. And the IR sensors we utilized in this application have a low level trigger output.
In this arrangement, switch one is wired to pin 2 of the pic16F877A microcontroller's PORT B. And switch 2 is linked to PORT B pin 1 and switch 3 to PORT B pin 0. The motors 1 and 2 are then attached to pins o and 1 IN port 3

Reference

George, L. and George, L., 2022. Water Level Indicator Controller Using PIC Microcontroller. [online] electroSome. Available at: https://electrosome.com/water-level-indicator-controller-pic/ [Accessed 19 July 2022].
• 2022. [online] Available at: https://www.researchgate.net/publication/335210568_PIC_Microcontroller_Based_Water_level_Monitoring_and_Controlling_System_using_Sharp_Infra-red_range_sensor [Accessed 19 July 2022].
• 2022. [online] Available at: https://livetechindia.com/water-level-indicator-using-micro-controller-pic16f877a/ [Accessed 19 July 2022].
• 2022. [online] Available at: https://www.fierceelectronics.com/sensors/what-ir-sensor [Accessed 19 July 2022].


