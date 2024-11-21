# GS---Edge-Computing
Global Solution - Edge Computing (PhotonClean)

# **PhotonClean ® - Sistema de Monitoramento e Limpeza de Placas Solares**  

O **PhotonClean®** é um sistema inovador que utiliza a plataforma Arduino para monitorar e auxiliar na limpeza de placas solares. Ele mede a intensidade luminosa refletida pelas placas para determinar seu estado de limpeza, fornecendo indicadores visuais e sonoros em tempo real. Este projeto visa maximizar a eficiência energética e garantir a sustentabilidade dos sistemas fotovoltaicos.

---

## **Visão Geral**  

### **Problema Abordado**  
Placas solares acumulam sujeira ao longo do tempo, como poeira e poluição, reduzindo drasticamente sua eficiência na geração de energia. A falta de manutenção adequada leva a perda de desempenho e aumento de custos operacionais.  

### **Solução Proposta**  
O PhotonClean monitoriza a limpeza das placas solares em tempo real utilizando sensores de luminosidade (LDR), LEDs indicadores e um display LCD. Ele alerta os usuários sobre a necessidade de manutenção, garantindo que as placas operem com máxima eficiência.  

### **Destaques do Projeto**  
- **Monitoramento Contínuo:** Leitura em tempo real da intensidade luminosa.  
- **Indicadores Visuais:** LEDs verdes, amarelos e vermelhos indicam o estado de limpeza das placas.  
- **Alarme Sonoro:** Um buzzer alerta para situações críticas.  
- **Sustentabilidade:** Melhora a eficiência energética de sistemas solares, incentivando o uso de energias renováveis.  

---

## **Componentes Utilizados**  

### **Hardware**  
1. **Arduino UNO**  
2. **Sensor LDR (Light Dependent Resistor)** - Leitura de luminosidade (lux).  
3. **Display LCD 16x2 I2C** - Exibição do estado de limpeza e valores de lux.  
4. **LEDs (Verde, Amarelo, Vermelho):**  
   - Verde: Indica "Muito Limpo".  
   - Amarelo: Indica "Meio Limpo".  
   - Vermelho: Indica "Muito Sujo".  
5. **Buzzer:** Alerta sonoro em casos de sujeira excessiva.  
6. **Resistores e Jumpers:** Para proteção e conexões.  

### **Software**  
- **Arduino IDE:** Para desenvolvimento e upload do código.  
- **Biblioteca LiquidCrystal_I2C:** Controle do display LCD.  
- **GitHub Copilot:** Utilizado como apoio na construção do código.  
- **WokWi Simulator:** Ferramenta para simular e testar o circuito.  

---

## **Diagrama do Circuito**  
O projeto foi implementado e testado no simulador WokWi. Clique no link abaixo para visualizar o modelo completo:  

[🔗 Acesse a Simulação do PhotonClean no WokWi](https://wokwi.com/projects/12345667890r)  

---

## **Código Fonte**  

O código para o projeto foi desenvolvido em **C++** e está disponível abaixo:  

```cpp
#include <LiquidCrystal_I2C.h>
int pin_ldr = A0;
int led_r = 7;
int led_y = 8;
int led_g = 9;
int buzzer = 6;

LiquidCrystal_I2C lcd(0x27, 16, 2);

const float GAMMA = 0.7;
const float RL10 = 50;

void setup() {
  lcd.init();
  lcd.backlight();
  Serial.begin(9600);
  pinMode(pin_ldr, INPUT);
  pinMode(led_r, OUTPUT);
  pinMode(led_y, OUTPUT);
  pinMode(led_g, OUTPUT);
  pinMode(buzzer, OUTPUT);
  lcd.setCursor(3, 0); //C x L
  lcd.print("Projeto GS");
  lcd.setCursor(3, 1); //C x L
  lcd.print("PhotonClean");
  delay(2000);
}

void loop() {
  int analogValue = analogRead(A0);
  float voltage = analogValue / 1024. * 5;
  float resistance = 2000 * voltage / (1 - voltage / 5);
  float lux = pow(RL10 * 1e3 * pow(10, GAMMA) / resistance, (1 / GAMMA));
  Serial.println(lux);

  lcd.clear();
  lcd.setCursor(0, 0);
  lcd.print("Lux:" + String(lux));

  if (lux > 50000) {
    digitalWrite(led_g, HIGH);
    digitalWrite(led_y, LOW);
    digitalWrite(led_r, LOW);
    noTone(buzzer);
    lcd.setCursor(0, 1);
    lcd.print("MUITO LIMPO");
    delay(2000);
  } else if (lux > 10000) {
    digitalWrite(led_y, HIGH);
    digitalWrite(led_g, LOW);
    digitalWrite(led_r, LOW);
    noTone(buzzer);
    lcd.setCursor(0, 1);
    lcd.print("MEIO LIMPO");
    delay(2000);
  } else {
    digitalWrite(led_r, HIGH);
    digitalWrite(led_g, LOW);
    digitalWrite(led_y, LOW);
    lcd.setCursor(0, 1);
    lcd.print("MUITO SUJO");
    tone(buzzer, 1000);
    delay(2000);
    noTone(buzzer);
  }
}
```

---

## **Instruções de Uso**  

1. **Montagem do Circuito:**  
   Monte o circuito conforme o diagrama disponível na simulação WokWi. Certifique-se de conectar corretamente o sensor LDR, LEDs, buzzer e display LCD ao Arduino.  

2. **Upload do Código:**  
   - Abra o código no Arduino IDE.  
   - Conecte o Arduino ao computador via USB.  
   - Selecione a porta e o modelo do Arduino (UNO).  
   - Faça o upload do código.  

3. **Operação:**  
   - O sistema inicia com uma mensagem de boas-vindas no display LCD.  
   - O sensor LDR mede a luz refletida pelas placas solares em tempo real.  
   - O display e os LEDs indicam o estado das placas:  
     - **Verde:** Limpeza adequada.  
     - **Amarelo:** Necessidade de manutenção em breve.  
     - **Vermelho:** Necessidade urgente de limpeza.  
   - Caso as placas estejam muito sujas, o buzzer emitirá um alarme.  

---

## **Dependências**  

Certifique-se de instalar a biblioteca LiquidCrystal_I2C no Arduino IDE antes de carregar o código.  

---

## **Resultados Esperados e Impacto**  

- **Melhorias na Eficiência Energética:** Manter as placas solares limpas aumenta a eficiência na geração de energia.  
- **Redução de Custos Operacionais:** Monitoramento contínuo evita manutenções desnecessárias e prolonga a vida útil das placas.  
- **Sustentabilidade:** Promove o uso de fontes renováveis e a preservação ambiental.  

O **PhotonClean®** é uma solução acessível e prática que visa transformar a forma como sistemas fotovoltaicos são gerenciados, garantindo um futuro mais verde e sustentável para todos.

---

## **Participantes**  

Davi Correa - RM560438
Filip Arnhold - RM559294
Marcos Azuma - RM559893 

---

**Desenvolvido com o auxílio do GitHub Copilot e utilizando o WokWi Docs como material base.**
