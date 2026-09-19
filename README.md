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

- **Mecânica:** descrever resumidamente.
- **Eletrônica:** descrever resumidamente.
- **Software:** descrever resumidamente.
- **Comunicação:** descrever resumidamente.
- **Alimentação:** descrever resumidamente.

---

## 4. Estado atual do desenvolvimento
*- 0.0-Criação do repositório no github*;  
*- 0.1-Criação da planilha de planejamento*;    
*- 0.2-Inicio do desenvolvimento dos sistemas*;    
*- 0.3-Desenvolvimento do novo chassi e organização do hardware*.;   


### Concluído

- [✓] Definição da arquitetura geral
- [ ] Projeto mecânico inicial
- [✓] Diagrama elétrico inicial
- [ ] Comunicação com o sistema da organização
- [ ] Controle dos motores em bancada
- [ ] Integração mecânica
- [ ] Integração eletroeletrônica
- [ ] Teste do veículo em movimento
- [ ] Integração da câmera
- [ ] Outros: ____________________

### Em desenvolvimento

- Desenvolvimento do novo chassi.


### Pendências principais

- Baterias adequadas pro projeto.  
- Implementação do sistema em um único ESP (Planos futuros).

---

## 5. Planejamento

O planejamento semanal da equipe está disponível em:

[`PLANEJAMENTO.md`](PLANEJAMENTO.md)

O registro semanal de atividades está disponível em:

[`PROGRESSO.md`](PROGRESSO.md)

---

## 6. Documentação técnica

Documentação técnica nas seguintes pastas:

```text
docs/
├── arquitetura/
├── mecanica/
├── eletronica/
├── software/
└── testes/
```

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
| ____________________ | ___ | Equipe / organização | ____________________ |

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
| __/__/2026 | ____________________ | ____________________ | ____________________ |

---

## 10. Observações

Registrar aqui informações importantes que não se encaixem nas demais seções.
