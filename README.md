// Push Button Realy Controller.ino

#define RELAY_ON 0                     // The  relays are active-LOW; on when LOW (0)
#define RELAY_OFF 1                    // and off when HIGH (1). These #defines just help keep track.

const int RelayPin1 =  2;         // Pin numbers for the  relays
const int RelayPin2 =  3;
const int RelayPin3 =  4;  // TIMER
const int RelayPin4 =  5; // alarm
const int RelayPin5 =  6;

const int ButtonPin1 = 12;
const int ButtonPin2 = 11;         // Pin numbers for the push buttons
const int ButtonPin3 = 10;
const int ButtonPin4 = 9;
const int ButtonPin5 = 8;
const int Duration = 500;          // Number of millisecs that the Relay run after the buttons pressed
const int Duration1 = 10000;                                       
const int Duration2 = 2000;
const int Duration3 = 9000;
const int Duration4 = 6000;
// ----VARIABLES
const long interval = 1000;
byte State1 = RELAY_OFF;           // Used to record whether the  are on or off
byte State2 = RELAY_OFF;           // (default to HIGH/OFF)
byte State3 = RELAY_OFF;
byte State4 = RELAY_OFF;
byte State5 = RELAY_OFF;

int currentMillis = 0;       // Stores the value of millis() in each iteration of loop()
int val2;
int TimerMillis1 = 0;    // Stores the times when the buttons last pressed
int TimerMillis2 = 0;
int TimerMillis3 = 0;
int TimerMillis4 = 0;
int TimerMillis5 = 0;

// ----SETUP (Sets up the pins at startup/reset)

void setup()
{

    // Debug
    Serial.begin(9600);
    Serial.println("Starting Relay Controller.ino");

    //  relays are active-LOW. Initialize realy
    // pins HIGH so the relays are inactive at startup/reset
    digitalWrite(RelayPin1, RELAY_OFF);
    digitalWrite(RelayPin2, RELAY_OFF);
    digitalWrite(RelayPin3, RELAY_OFF);
    digitalWrite(RelayPin4, RELAY_OFF);
    digitalWrite(RelayPin5, RELAY_OFF);
    
    // THEN set the pins as outputs
    pinMode(RelayPin1, OUTPUT);
    pinMode(RelayPin2, OUTPUT);
    pinMode(RelayPin3, OUTPUT);
    pinMode(RelayPin4, OUTPUT);
    pinMode(RelayPin5, OUTPUT);
    
    // Set the float sensor pins as inputs and use the built-in pullup resistors
    pinMode(ButtonPin1, INPUT_PULLUP);
    pinMode(ButtonPin2, INPUT_PULLUP);
    pinMode(ButtonPin3, INPUT_PULLUP);
    pinMode(ButtonPin4, INPUT_PULLUP);
    pinMode(ButtonPin5, INPUT_PULLUP);
}


// ----MAIN LOOP

void loop()
{
    // Get the current clock count

    currentMillis = millis() + Duration;           // We add the countdown timer duration to
                                                   // the clock to prevent the realy from
                                                   // running at boot time, while the clock
                                                   // counter is still below the timer value

    // Call the functions that do the work

    readButtonPin1();             // check each button and decide whether
    readButtonPin2();             // to start the relay and countdown timer
    readButtonPin3();
    readButtonPin4();
    triggerRealy();                 // Actually toggles the relay pins based on
    readButtonPin5();
}                                   // the data from the above functions.


// ----WORKER FUNCTIONS

void  readButtonPin1()
{
    if (digitalRead(ButtonPin1) == LOW)   // If the button is tripped (pulled low)
    {
        State1 = RELAY_ON;                 // then trigger the relay pin
        TimerMillis1 = currentMillis;      // and set the relay countdown timer to the current clock time
    }

    if (currentMillis - TimerMillis1 >= Duration1)   // If the countdown timer has expired,
    {
        State1 = RELAY_OFF;                             // then turn off the  relay pin
        TimerMillis1 = 0;                               // and reset the timer to 0 for the next trigger
    }
}

//========

void  readButtonPin2()
{
    if (digitalRead(ButtonPin2) == LOW)   // If the button is tripped (pulled low)
    {
        State2 = RELAY_ON;                 // then trigger the  relay pin
        TimerMillis2 = currentMillis;      // and set the relay countdown timer to the current clock time
    }

    if (currentMillis - TimerMillis2 >= Duration1)   // If the countdown timer has expired,
    {
        State2 = RELAY_OFF;                             // then turn off the  relay pin
        TimerMillis2 = 0;                               // and reset the timer to 0 for the next trigger
    }
}

//========

void  readButtonPin3()
{
    if (digitalRead(ButtonPin3) == LOW)   // If the button is tripped (pulled low)
    {
        State3 = RELAY_ON;                 // then trigger the  relay pin
        TimerMillis3 = currentMillis;      // and set the  countdown timer to the current clock time
    }

    if (currentMillis - TimerMillis3 >= Duration)   // If the countdown timer has expired,
    {
        State3 = RELAY_OFF;                             // then turn off the relay pin
        TimerMillis3 = 0;                               // and reset the timer to 0 for the next trigger
    }
}

//========

void  readButtonPin4()
{
    if (digitalRead(ButtonPin4) == LOW)   // If the button is tripped (pulled low)
    {
        State5 = RELAY_ON;                 // then trigger the  relay pin
        TimerMillis5 = currentMillis;      // and set the  countdown timer to the current clock time
    }

    if (currentMillis - TimerMillis5 >= Duration)   // If the countdown timer has expired,
    {
        State5 = RELAY_OFF;                             // then turn off the  relay pin
        TimerMillis5 = 0;                               // and reset the timer to 0 for the next trigger
    }
}
void  readButtonPin5()
{
    Serial.print(" TimerMillis1 =");   
Serial.println(TimerMillis1);
Serial.print(" TimerMillis2 =");   
Serial.println(TimerMillis2);
Serial.print(" TimerMillis4 =");   
Serial.println(TimerMillis4);


Serial.print(" val2 =");   
Serial.println(val2);
  TimerMillis1++;
  TimerMillis2++;
  TimerMillis4++;
  
  val2++;
   
  val2 = (TimerMillis1 + TimerMillis2);
    if (val2 >12000)   // If the countdown timer has expired,
     {
      
        State4 = RELAY_ON;
        TimerMillis4 = currentMillis;// then trigger the  relay pin
        TimerMillis1 = currentMillis; 
        TimerMillis2 = currentMillis; 
              // and set the  countdown ti mer to the current clock time
    }
 if (currentMillis - TimerMillis4 >= Duration2) 
    
    {
        State4 = RELAY_OFF;                             // then turn off the  relay pin
        TimerMillis4 = 0; 
        val2 = 0;
    }
}
void triggerRealy()
{
    // Toggle the  relay pins on and off based on
    // State from the readButton functions

    digitalWrite(RelayPin1, State1);
    digitalWrite(RelayPin2, State2);
    digitalWrite(RelayPin3, State3);
    digitalWrite(RelayPin4, State4);
    digitalWrite(RelayPin5, State5);
}
