# Código Teste: 01 (Bluethooth) #

```
 #include "BluetoothSerial.h"
BluetoothSerial SerialBT;

const int IN1 = 23;
const int IN2 = 22;
const int IN3 = 19;
const int IN4 = 18;
int ENA = 21;
int ENB = 4;

char sentidoFB; 
char sentidoRL;   
int velocidadeFB = 0;
int velocidadeRL = 0;

// Limiar mínimo onde seu motor realmente começa a girar com força
const int PWM_MINIMO = 100; 

// Função que escala o PWM de 0..255 para PWM_MINIMO..255
int ajustarPwmMinimo(int valorBruto) {
  if (valorBruto <= 0) return 0;
  
  // Mapeia o valor recebido de (1 até 255) para a faixa (PWM_MINIMO até 255)
  return map(valorBruto, 1, 255, PWM_MINIMO, 255);
}

void stop() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW); 
  digitalWrite(IN4, LOW);
  analogWrite(ENA, 0);
  analogWrite(ENB, 0);
  Serial.println("~~Carro parado~~");
}

void setup() {
  Serial.begin(115200);
  Serial.println("ESP OK!");
  SerialBT.begin("ESP32_Carrinho");

  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);
  pinMode(ENA, OUTPUT);
  pinMode(ENB, OUTPUT);

  stop();
}

void loop() {
  if (SerialBT.available()) {
    String comando = SerialBT.readStringUntil('\n');
    comando.trim();

    if (comando.length() > 0) {
      int lidos = sscanf(comando.c_str(), "%c%d%c%d", &sentidoFB, &velocidadeFB, &sentidoRL, &velocidadeRL);

      if (lidos == 4) {
        
        // 1. Aplica a escala para vencer o atrito inicial
        int velLinearAjustada = ajustarPwmMinimo(velocidadeFB);

        // 2. Define o sentido dos motores (F / B)
        switch (sentidoFB) {
          case 'F':
            digitalWrite(IN1, LOW);
            digitalWrite(IN2, HIGH);
            digitalWrite(IN3, LOW);
            digitalWrite(IN4, HIGH);
            break;

          case 'B':
            digitalWrite(IN2, LOW);
            digitalWrite(IN1, HIGH);
            digitalWrite(IN4, LOW);
            digitalWrite(IN3, HIGH);
            break;

          default:
            stop();
            break;
        }

        // 3. Calcula a velocidade para cada roda
        int velEsquerda = velLinearAjustada;
        int velDireita  = velLinearAjustada;

        // 4. Aplica o diferencial de curva
        switch (sentidoRL) {
          case 'L':
            velDireita = velLinearAjustada - velocidadeRL;
            break;

          case 'R':
            velEsquerda = velLinearAjustada - velocidadeRL;
            break;

          default:
            break;
        }

        // 5. Garante que os valores fiquem dentro de limites seguros (0 a 255)
        velEsquerda = constrain(velEsquerda, 0, 255);
        velDireita  = constrain(velDireita, 0, 255);

        // Se após o diferencial o motor ainda estiver com valor baixo (mas maior que 0), 
        // mantemos o patamar mínimo para a roda externa/interna não travar
        if (velEsquerda > 0 && velEsquerda < PWM_MINIMO) velEsquerda = PWM_MINIMO;
        if (velDireita > 0 && velDireita < PWM_MINIMO) velDireita = PWM_MINIMO;

        // 6. Envia o sinal PWM ajustado para a Ponte H
        if (sentidoFB == 'F' || sentidoFB == 'B') {
          analogWrite(ENA, velEsquerda);
          analogWrite(ENB, velDireita);
        }
      }
    }
  }

  delay(10); 
}
```
# Código Teste: 02 (Wi-fi) #

```

#include <WiFi.h>
#include <WebServer.h>

// 1. GPIO
const int IN1 = 23;
const int IN2 = 22;
const int IN3 = 19;
const int IN4 = 18;
const int ENA = 21;
const int ENB = 4;

// 2. Rede Wi-Fi do carrinho
const char* ssid = "VelozeseCuriosos";
const char* password = "123321123";

WebServer server(80);

const int PWM_MINIMO = 100;
const int velocidade = 180;

char direcao = 'S';
char ultimaDirecao = 'S';

unsigned long tempoParada = 0;
bool esperando = false;

int ajustarPwm(int valor) {
  if (valor == 0) return 0;
  return map(valor, 1, 255, PWM_MINIMO, 255);
}

void pararMotores() {
  analogWrite(ENA, 0);
  analogWrite(ENB, 0);

  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
}

void mover(char comando) {
  int pwm = ajustarPwm(velocidade);

  switch (comando) {
    case 'F':
      digitalWrite(IN1, LOW);
      digitalWrite(IN2, HIGH);
      digitalWrite(IN3, LOW);
      digitalWrite(IN4, HIGH);
      break;

    case 'B':
      digitalWrite(IN1, HIGH);
      digitalWrite(IN2, LOW);
      digitalWrite(IN3, HIGH);
      digitalWrite(IN4, LOW);
      break;

    case 'L':
      digitalWrite(IN1, HIGH);
      digitalWrite(IN2, LOW);
      digitalWrite(IN3, LOW);
      digitalWrite(IN4, HIGH);
      break;

    case 'R':
      digitalWrite(IN1, LOW);
      digitalWrite(IN2, HIGH);
      digitalWrite(IN3, HIGH);
      digitalWrite(IN4, LOW);
      break;

    default:
      pararMotores();
      return;
  }

  analogWrite(ENA, pwm);
  analogWrite(ENB, pwm);
}

void receberComando(char comando) {
  if (comando == 'S') {
    direcao = 'S';
    ultimaDirecao = 'S';
    esperando = false;
    pararMotores();
    return;
  }

  if (comando != direcao) {
    pararMotores();

    direcao = comando;
    ultimaDirecao = 'S';

    tempoParada = millis();
    esperando = true;
  }
}

void frente() {
  receberComando('F');
  server.send(200, "text/plain", "Frente");
}

void tras() {
  receberComando('B');
  server.send(200, "text/plain", "Tras");
}

void esquerda() {
  receberComando('L');
  server.send(200, "text/plain", "Esquerda");
}

void direita() {
  receberComando('R');
  server.send(200, "text/plain", "Direita");
}

void parar() {
  receberComando('S');
  server.send(200, "text/plain", "Parado");
}

void handleRoot() {
  String html = R"rawliteral(
  <!DOCTYPE html>
  <html>
  <head>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Controle do Carrinho</title>

    <style>
      body {
        text-align: center;
        font-family: Arial;
        background: #eeeeee;
      }

      button {
        width: 100px;
        height: 65px;
        font-size: 18px;
        margin: 6px;
        touch-action: none;
      }
    </style>

    <script>
      function enviar(comando) {
        fetch('/' + comando);
      }

      function configurar(id, comando) {
        const botao = document.getElementById(id);

        botao.addEventListener('pointerdown', function(e) {
          e.preventDefault();
          botao.setPointerCapture(e.pointerId);
          enviar(comando);
        });

        botao.addEventListener('pointerup', function() {
          enviar('S');
        });

        botao.addEventListener('pointercancel', function() {
          enviar('S');
        });

        botao.addEventListener('lostpointercapture', function() {
          enviar('S');
        });
      }

      window.onload = function() {
        configurar('frente', 'F');
        configurar('tras', 'B');
        configurar('esquerda', 'L');
        configurar('direita', 'R');
      };
    </script>
  </head>

  <body>
    <h2>Carrinho ESP32</h2>

    <button id="frente">Frente</button><br>

    <button id="esquerda">Esquerda</button>
    <button id="direita">Direita</button><br>

    <button id="tras">Tras</button>
  </body>
  </html>
  )rawliteral";

  server.send(200, "text/html", html);
}

void setup() {
  Serial.begin(115200);

  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);
  pinMode(ENA, OUTPUT);
  pinMode(ENB, OUTPUT);

  pararMotores();

  WiFi.softAP(ssid, password);

  Serial.println("Wi-Fi do carrinho iniciado");
  Serial.print("IP: ");
  Serial.println(WiFi.softAPIP());

  server.on("/", handleRoot);
  server.on("/F", frente);
  server.on("/B", tras);
  server.on("/L", esquerda);
  server.on("/R", direita);
  server.on("/S", parar);

  server.begin();
}

void loop() {
  server.handleClient();

  if (esperando && millis() - tempoParada >= 80) {
    esperando = false;
    ultimaDirecao = direcao;
    mover(direcao);
  }
}

```
