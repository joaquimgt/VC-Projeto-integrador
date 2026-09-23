# Desenvolvimento dos sistemas eletrônicos do projeto #
## * Diagrama elétrico inicial: ##
<img width="914" height="558" alt="diagrama eletrico inicial" src="https://github.com/user-attachments/assets/dd4115d7-a620-4931-8a62-371e2ea3f755" />

### Objetivo: ###

- Esp32 cam destinado à transmissão do vídeo;
- Esp32 destinado ao controle dos motores;

### Observação: ###
Idealmente seria interessante utilizar somente o esp cam para as duas funções, mas algumas pesquisas indicaram que talvez haja 
interferência e perda de pacotes durante a transmissão do vídeo e do controle remoto.

### Alimentação: ###

- Inicialmente o uso de 4 pilhas AA, porém, em análise.


## Componentes: ##
| Componente | Tipo/Especificações |
|---|---|
| Esp32 Cam | Esp32 S3 cam |
| Esp32 | Esp32 devkitv1 |
| Ponte H | Módulo L298n |
|  Servo motor | MicroServo SG90 |
| 2x Motores cc | Motor cc com caixa de redução |
| R1 |  |
| R2 |  |
| C1 |  |
| Q1 |  |
| Motor Turbo | Motor cc |



  

