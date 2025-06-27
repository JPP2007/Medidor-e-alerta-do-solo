🌱 Medidor de Umidade do Solo com Alerta Visual e Sonoro


📦 Visão Geral da Solução


Este projeto é um protótipo funcional baseado no Arduino Uno R3, desenvolvido para medir e monitorar a umidade do solo. O sistema fornece alertas visuais por meio de LEDs e alerta sonoro via buzzer quando a umidade do solo atinge níveis críticos.

Além disso, um display LCD I2C exibe em tempo real a umidade do solo e o horário, fornecido por um módulo RTC DS3231.

🔧 Resumo do Funcionamento


Mede a umidade do solo (sensor simulado com potenciômetro)

Exibe umidade e horário no display LCD

LEDs indicam:

🟢 Verde → Solo ideal

🟡 Amarelo → Atenção

🔴 Vermelho → Solo seco

Buzzer emite alarme sonoro em caso de solo muito seco

🧰 Componentes Utilizados


Arduino Uno R3

Display LCD 16x2 com módulo I2C

Sensor de Umidade do Solo ou Potenciômetro (para simulação)

Módulo RTC DS3231

LEDs: Verde, Amarelo e Vermelho

Buzzer piezoelétrico

Protoboard e jumpers

🔬 Simulação Online


Simular no Wokwi (Substitua pelo link real da sua simulação)

🎥 Vídeo Demonstrativo


Assista ao vídeo explicativo aqui (Coloque o link do seu vídeo)

📟 Funcionamento do Sistema


Nível de Umidade	LED Ativado	Alarme Sonoro
≥ 60%	Verde	Não
30% a 59%	Amarelo	Não
< 30%	Vermelho	Sim

📝 Código-Fonte


#include <Wire.h>


#include <LiquidCrystal_I2C.h>


#include <RTClib.h>


LiquidCrystal_I2C lcd(0x27, 16, 2); // Endereço do LCD I2C
RTC_DS3231 rtc;

// Pinos


const int sensorSolo = A0;


const int ledVerde = 6;


const int ledAmarelo = 7;


const int ledVermelho = 8;


const int buzzer = 9;



void setup() {


  Serial.begin(9600);

  
  lcd.init();

  
  lcd.backlight();

  

  pinMode(sensorSolo, INPUT);

  
  pinMode(ledVerde, OUTPUT);

  
  pinMode(ledAmarelo, OUTPUT);

  
  pinMode(ledVermelho, OUTPUT);

  
  pinMode(buzzer, OUTPUT);

  if (!rtc.begin()) {

  
    lcd.print("RTC nao encontrado!");

    
    while (1);
  }
}

void loop() {
  int leitura = analogRead(sensorSolo);

  
  int umidade = map(leitura, 1023, 0, 0, 100);

  
  umidade = constrain(umidade, 0, 100);

  

  DateTime agora = rtc.now();

  // Exibe dados no LCD

  
  lcd.clear();

  
  lcd.setCursor(0, 0);

  
  lcd.print("Solo: ");

  
  lcd.print(umidade);

  
  lcd.print("%");

  

  lcd.setCursor(0, 1);

  
  lcd.print(agora.hour());

  
  lcd.print(":");

  
  if (agora.minute() < 10) lcd.print("0");

  
  lcd.print(agora.minute());

  
  lcd.print(":");

  
  if (agora.second() < 10) lcd.print("0");

  
  lcd.print(agora.second());

  

  // Controle dos LEDs e buzzer

  
  digitalWrite(ledVerde, LOW);

  
  digitalWrite(ledAmarelo, LOW);

  
  digitalWrite(ledVermelho, LOW);

  
  noTone(buzzer);

  

  if (umidade >= 60) {
    digitalWrite(ledVerde, HIGH);
  } else if (umidade >= 30) {
    digitalWrite(ledAmarelo, HIGH);
  } else {
    digitalWrite(ledVermelho, HIGH);
    tone(buzzer, 1000);
  }

  delay(1000);
}
📚 Bibliotecas Necessárias


LiquidCrystal_I2C

RTClib

Wire

👨‍💻 Autor


Desenvolvido por João Pedro Palmeira

📜 Licença


Este projeto é de código aberto, sob a licença MIT. Sinta-se livre para usar, modificar e compartilhar.

