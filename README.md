# Desafio-Calle-Semaforos
#include <DCMotor.h>

DCMotor motor_izq(M0_EN,M0_D0,M0_D1);
DCMotor motor_der(M1_EN,M1_D0,M1_D1);

#define pin_s_izq_ext 2
#define pin_s_der_ext 3

#define pin_s_izq_cen 0
#define pin_s_der_cen 5

#define pin_s_izq_luz 4
#define pin_s_der_luz 1

#define negro 40
#define negro_ext 50
#define oscuro 700

#define blanco_izq 111
#define blanco_der 198

#define blanco_izq_e 171
#define blanco_der_e 160

#define luz_izq 673
#define luz_der 709

#define izq 10
#define der 11

int semaforo;

int s_izq_ext; //13-24
int s_der_ext;

int s_izq_cen;
int s_der_cen;

int s_izq_luz;
int s_der_luz;

void setup(){
  Serial.begin(9600);
  motor_der.setClockwise(false);
}

void loop(){
  s_izq_ext = analogRead(pin_s_izq_ext);
  s_der_ext = analogRead(pin_s_der_ext);
  s_izq_cen = analogRead(pin_s_izq_cen);
  s_der_cen = analogRead(pin_s_der_cen);
  s_izq_luz = analogRead(pin_s_izq_luz);
  s_der_luz = analogRead(pin_s_der_luz);
/*Serial.print("s_izq_ext:");
Serial.println(s_izq_ext);
Serial.print("s_der_ext:");
Serial.println(s_der_ext);
delay(500);*/
seguidor();
while(semaforo!=0){
  f_semaforo();
}
  
}

void seguidor (){
 if(s_izq_cen > negro && s_der_cen > negro){
 if(s_izq_luz > oscuro || s_der_luz > oscuro){
   if(s_izq_luz >400){
     semaforo=izq;
   }
   else{
    semaforo=der; 
   }
   return;
 } 
  else {
  motores(30,30);
  delay(100);
  }
 } 
 else if(s_izq_ext >40){
  motores(30,-10);
 }
 else if(s_der_ext >40){
motores(-10,30);
 }
 else {
  motores(30,30);
 }
} 

void f_semaforo(){
  s_izq_ext = analogRead(pin_s_izq_ext);
  s_der_ext = analogRead(pin_s_der_ext);
  s_izq_cen = analogRead(pin_s_izq_cen);
  s_der_cen = analogRead(pin_s_der_cen);

  if(s_der_ext > 40 ){
   motores(-30,30);
   
   if(s_izq_cen > negro && s_der_cen > negro){
    motores(30,-30);
   delay(500);
  semaforo=0;
   if(s_izq_cen > negro && s_der_cen > negro){
 
  motores(30,10);
  delay(150);
  }
  
 else if(s_izq_ext >40){
  motores(30,-10);
 }
 else if(s_der_ext >40){
motores(-10,30);
 }
 else {
  motores(30,30);
 }
 return;
} 
   

}
else{
 motores(-20,-30); 

}
}


void motores(int b,int c){
  motor_izq.setSpeed(b);
  motor_der.setSpeed(c);
}

void retroceder(){
 motores(-30,-30);
delay(100); 
}
