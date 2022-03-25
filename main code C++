#include "mbed.h"
#include "C12832.h"

volatile float freq = 2500;

// time period in use = 1 / 2 * (frequency) as 

C12832 lcd(D11, D13, D12, D7, D10);

InterruptIn centre(D4);
InterruptIn down(A3);
InterruptIn up(A2);
    
    Ticker TICKR;

  volatile float delta_freq = 250;
  
  class Speaker {
        
private:
DigitalOut outputSignal;
char state;    // Can be set to either 1 or 0 to record output value

public:
Speaker(PinName pin) : outputSignal(pin){off();}

void on(void)
{  
   outputSignal = 1;
   state = 'a';
}   
    
void off(void)
{ 
   outputSignal = 0;
   state = 'b'; 
}
 
void toggle(void)
{ 
   if (state == 'a') off();
     else on(); 
}
        
bool getstate(void)
 { 
    if (state == 'b') return false;
     else return true;    
}
 
 };


class LED                                           
{

protected:                                         
    DigitalOut outputSignal;                        
    bool status;                                    

public:                                             
    LED(PinName pin) : outputSignal(pin){off();}    

    void on(void)                                   
    {
        outputSignal = 0;                           
        status = true;                             
    }

    void off(void)                                  
    {
        outputSignal = 1;                           
        status = false;                            
    }

    void toggle(void)                              
    {
        if (status)                               
            off();                                 
        else                                      
            on();                                 
    }

    bool getStatus(void)                           
    {
        return status;                              
    }
};


LED RED(D5);
LED GREEN(D9);
Speaker SPEAKER(D6);

void centreISR()
{
    if (SPEAKER.getstate()) {
        TICKR.attach(&SPEAKER, &Speaker::toggle, ( 1 / ( 2 * freq )));
        
         SPEAKER.toggle();
          
           lcd.cls();
            lcd.locate(20,10);
            lcd.printf("frequency: %.3lf", freq);
            
            
             RED.off();
             GREEN.on();
        }
        
    else if (!SPEAKER.getstate()) {
         SPEAKER.toggle();
          
           lcd.cls();
           lcd.locate(20,10);
            lcd.printf("Speaker is off.");
             
             
              RED.on();
              GREEN.off();
              
              TICKR.detach();
        }
            }
            

void downISR()
{
    if (SPEAKER.getstate()) {
        
        freq = freq - delta_freq;
        
        TICKR.attach(&SPEAKER, &Speaker::toggle, ( 1 / ( 2 * freq )));
        

        
          lcd.cls();
           lcd.locate(20,10);
           lcd.printf("frequency: %.3lf", freq);
            
        }
}

void upISR()
{
    if (SPEAKER.getstate()) {
        
        freq = freq + delta_freq;
        
        TICKR.attach(&SPEAKER, &Speaker::toggle, ( 1 / ( 2 * freq )));
        

        
        lcd.cls();
         lcd.locate(20,10);
         lcd.printf("frequency: %.3lf", freq);
        
        }
}

int main() {
    
    centre.rise(&centreISR);
    down.rise(&downISR);
    up.rise(&upISR);
    
    GREEN.on();
    RED.off();
     
    SPEAKER.on();
    
    
    lcd.cls();
     lcd.locate(20,10);
      lcd.printf("frequency: %.3lf", freq);
    
   


    TICKR.attach(&SPEAKER, &Speaker::toggle, ( 1 / ( 2 * freq )));
    
      while(1) {}
      
      }
