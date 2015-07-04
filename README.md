# Arduino_Mega TIMER


Arduino mega pins and hardware Timers

Timer  | output	 |Arduino| Chip |  BIT  |  BIT    
-------|---------|-------|------|-------|---------
Timer0 |  OCR0A	 |  4	   |  	  |  8    |
Timer0 |  OCR0B	 |  13   |  	  |  8    |
Timer1 |  OCR1A	 |  11	 |  	  |  16   |
Timer1 |  OCR1B	 |  12	 |  	  |  16   |
Timer2 |  OCR2A	 |  9	   |  	  |  8    |
Timer2 |  OCR2B	 |  10	 |    	|  8    |
Timer3 |  OCR0A	 |  4	   |  	  |  16   |
Timer3 |  OCR0B	 |  13   |  	  |  16   |
Timer4 |  OCR1A	 |  11	 |  	  |  16   |
Timer4 |  OCR1B	 |  12	 |  	  |  16   |
Timer5 |  OCR2A	 |  44   |  	  |  16   |
Timer5 |  OCR2B	 |  45	 |     	|  16   |
Timer5 |  OCR2C	 |  46	 |     	|  16   |


INITIALIZATION OF ATMEGA 1280/2560 TIMER1, TIMER3,  TIMER4, TIMER5
- All 4 Timers sends 8 channel of PWM to usable Pin 11, 12, 2, 4, 45, 46
```
#define Top_400hz   5000       // Counts required for 400Hz Frequency
#define Top_050hz   40000      // Counts required for  50Hz Frequency

#define PulseCH1 1100  // +Pulse or Duty Cycle for PWM Pinout of Timer1 at pin  11, 12
#define PulseCH3 1100  // +Pulse or Duty Cycle for PWM Pinout of Timer1 at pin  2, 3, 5
#define PulseCH4 1100  // +Pulse or Duty Cycle for PWM Pinout of Timer1 at pin  6, 7, 8
#define PulseCH5 1100  // +Pulse or Duty Cycle for PWM Pinout of Timer1 at pin  45, 46, 44 
```

```
void Init_PWM1(PulseCH1)
{
  pinMode(11,OUTPUT);
  pinMode(12,OUTPUT);
 
  TCCR1A =((1<<WGM11)|(1<<COM1A1)|(1<<COM1B1)|(1<<COM1C1));  
  TCCR1B = (1<<WGM13)|(1<<WGM12)|(1<<CS11); //Prescaler= 8 (approx resolution of 1us as tested) page 134 of data sheet
  OCR1A = PulseCH1; //PB5, channel1, route to Pin 11
  OCR1B = PulseCH1; //PB6, channel2, route to Pin 12
  OCR1C = PulseCH1; //PB7  Not Supported
   ICR1 = Top_400hz; //400hz freq = {MCU_freq 16Mhz/prescaler 8)/Desired_frequency 400hz} is (16000000hz/8)/400hz=5000
}

```
```

void Init_PWM3(PulseCH3)
{
  /Timer3 Utilizes Pin 2,3,5
  pinMode(2,OUTPUT);
  pinMode(3,OUTPUT);
  pinMode(5,OUTPUT);


  TCCR3A =((1<<WGM31)|(1<<COM3A1)|(1<<COM3B1)|(1<<COM3C1));   
  TCCR3B = (1<<WGM33)|(1<<WGM32)|(1<<CS31); //Prescaler= 8 (approx resolution of 1us as tested) page 134 of data sheet
   OCR3A = PulseCH3; //PE3, Not Usable
   OCR3B = PulseCH3; //PE4, channel7 
   OCR3C = PulseCH3; //PE5, channel6 
    ICR3 = Top_400hz ; //400hz freq = {MCU_freq 16Mhz/prescaler 8)/Desired_frequency 400hz} is (16000000hz/8)/400hz=5000
}

```
```

void Init_PWM4(void)
{
  pinMode(6, OUTPUT);
  pinMode(7,  OUTPUT);
  pinMode(8,  OUTPUT);
  TCCR4A = ((1<<WGM41)|(1<<COM4C1)|(1<<COM4B1)|(1<<COM4A1));  
  TCCR4B = ((1<<WGM43)|(1<<WGM42)|(1<<CS41);  //Prescaler= 8 (approx resolution of 1us as tested) page 134 of data sheet
  
  OCR4A = PulseCH4; // Not Usable 
  OCR4B = PulseCH4; //PH4, channel5
  OCR4C = PulseCH4; //PH5, channel4
   ICR3 = Top_050hz ; //50hz freq = {MCU_freq 16Mhz/prescaler 8)/Desired_frequency 50hz} is (16000000hz/8)/50hz=40000 
}

```

```

void Init_PWM5(void)
{
  pinMode(44,OUTPUT);
  pinMode(45,OUTPUT);
  pinMode(46,OUTPUT);
  
  TCCR5A =((1<<WGM51)|(1<<COM5A1)|(1<<COM5B1)|(1<<COM5C1)); 
  TCCR5B = (1<<WGM53)|(1<<WGM52)|(1<<CS51);  //Prescaler= 8 (approx resolution of 1us as tested) page 134 of data sheet
  OCR5A = PulseCH5; //PL3, 
  OCR5B = PulseCH5; //PL4, channel0
  OCR5C = PulseCH5; //PL5  channel1
   ICR5 = Top_400hz ; //400hz freq = {MCU_freq 16Mhz/prescaler 8)/Desired_frequency 400hz} is (16000000hz/8)/400hz=5000
}
