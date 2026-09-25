## Desenvolvimento do sistema mecânico do projeto ##

*Estrutura mecânica inicial:*

<img width="1200" height="896" alt="image" src="https://github.com/user-attachments/assets/51c70b72-56cd-4c54-b855-56e5d7b300b0" />

*Objetivo:*
- Desenvolver a estrutura mecânica do carrinho;
- Integrar o sistema de tração e a roda boba;
- Fixar a ESP32-CAM e o servo motor ao chassi;
- Garantir estabilidade e organização dos componentes.

*Estrutura:*

O chassi será composto por uma estrutura de plastico (feito na 3D), responsável pela sustentação dos componentes e pela distribuição da massa do carrinho.

As duas rodas motrizes são posicionadas nas laterais do chassi e conectadas aos motores CC com caixa de redução. Na região frontal é instalada uma roda boba, responsável pelo apoio da extremidade do carrinho e pela redução do atrito durante as mudanças de direção.

A parte superior do chassi será utilizada para a fixação dos componentes eletrônicos, incluindo o ESP32, a ponte H e a alimentação do sistema.

Na região frontal, será instalado um suporte para a ESP32-CAM. O suporte será conectado ao microservo SG90, possibilitando o movimento angular da câmera.

Na parte traseira será instalado um aerofólio, utilizado como elemento complementar à estrutura e à estética do protótipo.

*Componentes mecânicos:*
Componente	Tipo/Especificações
Chassi	Estrutura de placas para suporte dos componentes
2x Rodas motrizes	Rodas acopladas aos motores CC
Roda boba	Roda livre para apoio e estabilidade
2x Motores	Motor CC com caixa de redução
Suporte da ESP32-CAM	Estrutura de fixação e movimentação da câmera
Servo motor	MicroServo SG90
Aerofólio	Elemento estrutural e estético traseiro
Elementos de fixação	Parafusos, porcas e espaçadores

*Distribuição dos componentes:*

A distribuição dos componentes foi definida de maneira a aproveitar o espaço disponível no chassi e manter uma disposição organizada.

Parte frontal: ESP32-CAM e servo motor;
- Laterais: duas rodas motrizes e seus respectivos motores;
- Parte central: ESP32 e componentes de controle;
- Parte superior/traseira: ponte H e alimentação;
- Parte inferior/frontal: roda boba;
- Parte traseira: aerofólio.
*Observação:*

O conceito apresentado inicialmente possuía uma turbina/motor adicional na parte traseira. Após a análise do projeto, a utilização da turbina foi descartada, permanecendo no sistema apenas os componentes necessários para a locomoção, controle e transmissão de vídeo.
