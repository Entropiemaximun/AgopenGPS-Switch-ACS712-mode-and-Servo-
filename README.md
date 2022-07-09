# AgopenGPS-Switch-ACS712-mode-and-Servo-

video https://www.youtube.com/watch?v=G-5kT9UqfJE


---------------------FR-----------------------------
Code modifié pour l'ajout d'un servo sur la pin D5  du PCB  V2
 
 L'objectif  est : 
 - d'utiliser le mode Switch  pour l'engagement du moteur electrique   sur les pignon
 - detecter le sur couranrt  avec un AC 712 pour arreter la fonction d'autoguidage 
 - activer le servo pour debrayer le moteur 
 
 C'est une fonction de securité  et pas un embrayage par servo 
   
  Le servo a 3 mode
  1)  sleep  mode d'attente 
  2) engagé  mode activé  avec le switch et pret a la deconnextion
  3) stop mode  pour debrayé le mechanism 


---------------------EN-----------------------------
code modify to insert a Servo  in D5  PCB v2    
 
 Target  is: 
 - to use Switch   mode   by  knowing  if  electric motor   is engaged to the gear
 - detect  over currect  with ACS  712  to   stop   autosteer   function
 - engaged  the  servo to disconnect the  motor 
 
 This is an Safety function and  not an servo clutch 
 
The servo  has  3  way
  1/  sleep mode wait  action 
  2/ engaged  mode acivate  with the switch  and  ready to disconnect
  3/  Stop mode   to unclutch  the mecanism
  

  
  
 ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- 
 code  to be add 
  
   //--------------------------- servo -------------------------INIT
  /* Inclut la lib Servo pour manipuler le servomoteur */
  #include <Servo.h>

  /* Créer un objet Servo pour contrôler le servomoteur */
  Servo monServomoteur;

  #define SERVO_PIN 5       // D5
  int servo_position = 160 ; //
  int servo_position_sleep =170 ; //
  int servo_position_steer =120 ; //
  int servo_position_stop =35 ; //
  int servo_delai = 75 ;
  int delais_servo = 75 ;







  //--------------------------- servo -------------------------Setup

   monServomoteur.attach(SERVO_PIN);










  //--------------------------- servo -------------------------loop

// if (steerConfig.SteerSwitch == 1)         //steer switch on - off
//    {
//    steerSwitch = digitalRead(STEERSW_PIN); //read auto steer enable switch open = 0n closed = Off
// }

 
 
        if (steerConfig.SteerSwitch == 1)         //steer switch on - off
      {
       if   (servo_delai < delais_servo)
       {steerSwitch = 0;}
       else
       {steerSwitch = digitalRead(STEERSW_PIN);
        }//read auto steer enable switch open = 0n closed = Off
        }
        





 //Current sensor?        

//     if (sensorReading >= steerConfig.PulseCountMax)
//      {
//          steerSwitch = 1; // reset values like it turned off
//          currentState = 1;
//          previous = 0;
//      }


      if (sensorReading >= steerConfig.PulseCountMax)
      {
            steerSwitch = 1; // reset values like it turned off
            currentState = 1;
            previous = 0;          
   
            servo_delai = 5; //////////////////////////////////////////servo activation//////////////////////////
        }  
     

        
  //----------------------- servo ------------------------servo_position  before  “  } //end of timed loop”

     //servo_position

//nothing   so sleep mode
if (servo_position++ > servo_position_sleep) servo_position = servo_position_sleep;

// ready  to stop  so steer mode
if ( !steerSwitch ){  
  servo_position = servo_position_steer;
}

//  need to be stop steering  so delay  be at minimun   to stop  autosteer  and  servo to unactivate the mecanism
if (servo_delai++ >=  delais_servo){
  servo_delai = delais_servo + 1;
}else
{    servo_position = servo_position_stop ;  // unactivate  clutch
}




      
  //--------------------------- servo ------------------------servo_action  after  “  } //end of timed loop”

//  write the position of the servo
   monServomoteur.write( servo_position);        

