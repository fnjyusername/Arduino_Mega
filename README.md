# Arduino_Mega TIMER

INITIALIZATION OF ATMEGA 1280/2560 TIMER1, TIMER3,  TIMER4, TIMER5
- All 4 Timers sends 8 channel of PWM to usable Pin 12, 13, 3, 4, 45, 46
```
#define Top_400hz   5000       // Counts required for 400Hz Frequency
#define Top_050hz   40000      // Counts required for  50Hz Frequency

#define PulseCH1 1100  // +Pulse or Duty Cycle for PWM Pinout of Timer1 at pin 11, 12, 13 (11 not used)
#define PulseCH3 1100  // +Pulse or Duty Cycle for PWM Pinout of Timer1 at pin  3, 4, (2 not use)
#define PulseCH4 1100  // +Pulse or Duty Cycle for PWM Pinout of Timer1 at pin  7, 8
#define PulseCH5 1100  // +Pulse or Duty Cycle for PWM Pinout of Timer1 at pin  45, 46, (44 not use)
```

```
void Init_PWM1(PulseCH1)
{
  pinMode(11,OUTPUT);
  pinMode(12,OUTPUT);
  pinMode(13,OUTPUT);
 
  TCCR1A =((1<<WGM11)|(1<<COM1A1)|(1<<COM1B1)|(1<<COM1C1));  
  TCCR1B = (1<<WGM13)|(1<<WGM12)|(1<<CS11); //Prescaler= 8 (approx resolution of 1us as tested) page 134 of data sheet
  OCR1A = PulseCH1; //PB5, none
  OCR1B = PulseCH1; //PB6, channel2
  OCR1C = PulseCH1; //PB7  channel3
   ICR1 = Top_400hz; //400hz freq = {MCU_freq 16Mhz/prescaler 8)/Desired_frequency 400hz} is (16000000hz/8)/400hz=5000
}

```
```

void Init_PWM3(PulseCH3)
{
  /Timer3 Utilizes Pin 2,3,4
  pinMode(2,OUTPUT);
  pinMode(3,OUTPUT);
  pinMode(4,OUTPUT);


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
  pinMode(49, OUTPUT);
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
