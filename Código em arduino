#include "LedControl.h"

//LedControl variable = LedControl(int dataPin, int clkPin, int csPin, int numDevices);
LedControl lc = LedControl(2, 3, 4, 1);

//Variáveis global
int button1 = 8, button2 = 7; //Portas de botão 1 e botão 2

//Carinha feliz está armazenada aqui
const uint64_t IMAGES[] = { 
  0x3c4299a581a5423c
};
const int IMAGES_LEN = sizeof(IMAGES) / 8;

void setup()
{
  Serial.begin(9600); //Não sei bem o que é
  lc.shutdown(0, false); //habilita o display
  lc.setIntensity(0, 8); //maximum = 15
  pinMode(button1, INPUT);  //Setup inicial da leitura dos botões
  pinMode(button2, INPUT);  //Setup inicial da leitura dos botões
  randomSeed(analogRead(0));//Setup inicial pra utilizar o random
}
//Impressão da carinha feliz nessa função
void displayImage(uint64_t image) {
  for (int i = 0; i < 8; i++) {
    byte row = (image >> i * 8) & 0xFF;
    for (int j = 0; j < 8; j++) {
      lc.setLed(0, i, j, bitRead(row, j));
      delay(50);
    }
  }
  delay(1000);
}
void loop()
{
  int directionX; //Bolinha sobi ou desce
  int directionY; //Bolinha vai pra esquerda ou direita
  float speedball = 300;  //Velocidade inicial da bolinha
  int platform = 3; //Posição inicial da plataforma
  int Random; //Bolinha nasce em um local aleátorio
  int counter = 0; //Printa a carinha feliz se conseguir 25 ou mais

  int row = 0, column = random(8);  //Linha sempre começa pelo 0 e a coluna é aleátorio de 0 a 7

  while (row != 7)
  {
    Random = random(3);

    if (Random == 0 && row == 0) //Se random for 0, bolinha começa pela direita
    {
      directionY = 1;
    }
    else if (Random == 1 && row == 0)  //Se random for 1, bolinha começa pela esquerda
    {
      directionY = -1;
    }
    else if (Random == 2 && row == 0)  //Se random for 1, bolinha começa pela esquerda
    {
      directionY = 0;
    }

    if (digitalRead(button1) && platform + 1 < 7) //vai pra direita quando botão1 foi clicado e não deixa ultrapassa a margem
    {
      platform++;
    }
    if (digitalRead(button2) && platform > 0) //vai pra esquerda quando botão2 foi clicado e não deixa ultrapassa a margem
    {
      platform--;
    }
    
    lc.clearDisplay(0);
    lc.setLed(0, 7, platform, HIGH);  //Acende a plataforma
    lc.setLed(0, 7, platform + 1, HIGH);  //Acende a plataforma
    lc.setLed(0, row, column, true);  //Acende a bolinha
    delay(speedball); //controla a velocidade da bolinha e da plataforma
    
    if (row == 0)directionX = 1;  //Faz descer a bolinha quando linha for 0
    if (row == 6 && (platform == column || (platform - 1 == column && (directionY == +1 || column == 0)))){directionX = -1;counter += 1;} //Faz subir a bolinha quando linha for 6 e o player conseguir atingir a bolinha
    if (row == 6 && (platform + 1 == column || (platform + 2 == column && (directionY == -1 || column == 7)))){directionX = -1;counter += 1;} //Faz subir a bolinha quando linha for 6 e o player conseguir atingir a bolinha
    if (row == 6 && (platform == column || platform + 1 == column) && random(2) == 0)directionY = 0; //Bolinha sobi reto
    if (column == 0)directionY = 1; //A bolinha vai pra direita se caso pegar na coluna 0
    if (column == 7)directionY = -1;  //A bolinha vai pra esquerda se caso pegar na coluna 7

    row += directionX;  //As condições de row são aplicadas
    column += directionY; //As condições de column são aplicadas

    speedball -= 0.5; //Cada ciclo a velocidade da plataforma e a bolinha aumentam
  }

  //Pequena animação pra saber onde a bolinha morreu
  lc.clearDisplay(0);
  for (int i = 0; i < 2; i++)
  {
    lc.setLed(0, 7, platform, HIGH);
    lc.setLed(0, 7, platform + 1, HIGH);
    lc.setLed(0, row, column, HIGH);
    delay(500);
    lc.setLed(0, 7, platform, LOW);
    lc.setLed(0, 7, platform + 1, LOW);
    lc.setLed(0, row, column, LOW);
    delay(500);
  }
  //Contador para saber se atingiu a condição para printar a carinha feliz
  if (counter >= 25)
  {
    displayImage(IMAGES[0]);
  }
}
