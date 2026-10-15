# Projeto Integrador — Competição de Carrinhos

## Velozes e curiosos (VC)

**Nome da equipe:** Equipe Velozes e curiosos(VC)  
**Turma:** MECA 3M  
**Professor técnico:** José Soares Batista Lopes

### Integrantes e áreas de atuação

| Integrante | Área principal | Responsabilidades |
|---|---|---|
| Lucas Podosk | Piloto e calibração | Testes de dirigibilidade, calibração e operação |
| Maria Eduarda | Mecânica | Chassi, direção, transmissão e montagem |
| Ana Allyce e Maria Eloisa | Projeto e documentação | Arquitetura, desenhos, organização da documentação |
| Joaquim | Eletrônica e integração elétrica | Alimentação, drivers, motores, sensores e conexões |
| Lucas Gabriel | Software e controle | Comunicação, controle, sensores e automação |

---

## 1. Objetivo do projeto

Desenvolver um veículo terrestre em pequena escala para participação na Competição de Carrinhos do Projeto Integrador. O veículo deverá percorrer a pista definida pela organização, respeitando os requisitos do regulamento e utilizando a interface de controle disponibilizada.

---

## 2. Conceito da solução

Solução escolhida pela equipe.

- **Arquitetura de tração:** diferencial com dois motores;
- **Direção:** Variação da velocidade das rodas;
- **Controlador principal:** ESP32 devkitv1;
- **Driver de motores:** Ponte H (Módulo L298n);
- **Câmera embarcada:** ESP32 S3 CAM;
- **Estratégia de alimentação:** 4 baterias AA recarregáveis (Em análise);
- **Recursos de automação:** imagem transmitida do ESP cam pro celular via WiFi.

---

## 3. Arquitetura geral

Inserir aqui um diagrama da arquitetura do sistema ou um link para o arquivo correspondente em `docs/arquitetura/`.

Exemplo de organização:

```text
Volante da organização
        |
        | UDP
        v
      ESP32
        |
        +--> controle dos motores
        +--> sensores
        +--> atuadores
        |
        +--> MQTT --> telemetria

Celular embarcado --> transmissão de vídeo
```

### Subsistemas

- **Mecânica:** responsável pela estrutura física do carrinho, incluindo o chassi, a fixação dos motores, as rodas e a organização dos componentes. O projeto mecânico está sendo atualizado para melhorar a distribuição dos componentes e o desempenho do veículo.
- **Eletrônica:** composta pelo ESP32, pelo driver de motores e pelas conexões elétricas necessárias para controlar os dois motores DC. Também inclui a organização da alimentação e a integração dos componentes eletrônicos.
- **Software:** responsável pela programação do ESP32, pelo recebimento dos comandos, pelo controle da velocidade e do sentido de rotação dos motores e pela integração dos sistemas do carrinho.
- **Comunicação:** prevê a comunicação com o sistema de controle da competição por UDP, conforme o protocolo definido pela organização. A câmera ESP32-S3-CAM também está prevista para transmitir imagens por Wi-Fi para um celular.
- **Alimentação:** descrever resumidamente.

---

## 4. Estado atual do desenvolvimento
*- 0.0-Criação do repositório no github*;  
*- 0.1-Criação da planilha de planejamento*;    
*- 0.2-Inicio do desenvolvimento dos sistemas*;    
*- 0.3-Desenvolvimento do novo chassi e organização dos componentes no hardware*.;    
*- 0.4-Projeto do carrinho no Fusion360*.;  
*- 0.5-Controle do carrinho com a fonte de bancada*.;   
*- 0.6-Desenvolvimento do código para comandos via Bluetooth*.;   
*- 0.7-Sistema de direção básica testado*.;  
*- 0.8-Teste de desenvolvimento com rodas frontais*.;   
*- 0.9-Organização do carrinho e documentos para avaliaçãoda Etapa 1*.;   


### Concluído

- [✓] Definição da arquitetura geral
- [✓] Projeto mecânico inicial
- [✓] Diagrama elétrico inicial
- [✓] Comunicação com o sistema da organização
- [✓] Controle dos motores em bancada
- [✓] Integração mecânica
- [✓] Integração eletroeletrônica
- [✓] Teste do veículo em movimento
- [ ] Integração da câmera
- [ ] Outros: ____________________

### Em desenvolvimento

- Confecção de revestimentos para os pneus dos carrinhos.
- Encoder mecânico para sincronização das rodas.


### Pendências principais

- Encoder mecânico ou óptico.
- Arranjo de Oring para pneus

---

## 5. Planejamento

O planejamento semanal da equipe está disponível em:

[`PLANEJAMENTO.md`](PLANEJAMENTO.md)

O registro semanal de atividades está disponível em:

[`PROGRESSO.md`](PROGRESSO.md)

---

## 6. Documentação técnica

Documentação técnica nas seguintes pastas:

- Arquitetura geral: **docs -> arquitetura_e_design.md**
- Projeto mecânico: **docs -> mecânica.md**
- Diagrama elétrico: **docs -> Eletrônica.md**
- Documentação do software: **docs -> Software.md**
- Lista de materiais: **docs -> mecânica.md**
- Registros de testes: **README.md**

### Documentos disponíveis

- Arquitetura geral: ____________________
- Projeto mecânico: ____________________
- Diagrama elétrico: ____________________
- Documentação do software: ____________________
- Lista de materiais: ____________________
- Registros de testes: ____________________

---

## 7. Materiais e componentes

| Item | Quantidade | Origem | Situação |
|---|---:|---|---|
| ESP32 | 1 | Kit da organização | Disponível |
| Motor DC | 2 | Kit da organização | Disponível |
| Driver de motor | 1 | Kit da organização | Disponível |
| Ponte H | 1 | Kit da organização | Disponível |
| Chave interruptor| 1 | Equipe | Disponível |

---

## 8. Comunicação com a organização

### Comandos

- Protocolo: UDP unicast
- Porta: 5000
- Formato: JSON em UTF-8
- Frequência nominal: 60 Hz

Formato esperado:

```json
{
  "sequencia": 123,
  "volante": 0,
  "aceleracao": 0,
  "habilitado": true
}
```

### Telemetria

- Protocolo: MQTT 3.1.1 sobre TCP
- Porta: 1883
- Tópico previsto: `carrinhos/<equipe>/telemetria`

Os campos definitivos de telemetria serão definidos pela equipe em conjunto com os professores.

---

## 9. Testes realizados

Registrar os testes relevantes do projeto. Para registros mais detalhados, utilizar `docs/testes/`.

| Data | Teste | Resultado | Próxima ação |
|---|---|---|---|
| 18/9/2026 | Funcionamento das rodas do carrinho | Funcionaram corretamente | Criar código de controle |
| 18/9/2026 | Montagem do chassi oferecido pelo professor | Carro estável porém pesado | Organizar componentes no carro |
| 25/9/2026 | Controle do carrinho | Código funcionou muito bem, carrinho com movimentação livre | Avançar no código visando eficiência |
| 25/9/2026 | Roda ball carter no carrinho | Ideia interessante mas gerou bastante atrito e redução de velocidade | Conseguir rodas para a frente do carro |


Controle do carrinho
---

## 10. Observações

Registrar aqui informações importantes que não se encaixem nas demais seções.
