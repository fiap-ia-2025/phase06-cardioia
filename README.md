# ❤️🤖 CardioIA — Fase 6

## Sistema Preditivo de Pico de Risco e Integração Multiagente

Projeto acadêmico desenvolvido para a **Fase 6 do CardioIA**, com foco na construção de um modelo de Machine Learning capaz de prever situações de **pico de risco sistêmico** e preparar sua saída para uma futura integração com um sistema multiagente.

> ⚠️ **Aviso:** os dados utilizados neste projeto são simulados e têm finalidade exclusivamente acadêmica. O sistema não substitui avaliação, diagnóstico ou decisão clínica profissional.

---

## 📌 Visão geral

O projeto é dividido em duas etapas principais:

### Parte 1 — Modelo Preditivo de Pico de Risco

Nesta etapa foi desenvolvido um modelo supervisionado de classificação para identificar se um determinado cenário apresenta ou não um **pico de risco**.

O fluxo desenvolvido é:

```text
Dados simulados
      ↓
Preparação da base
      ↓
Separação entre variáveis e alvo
      ↓
Divisão treino / teste
      ↓
Treinamento dos modelos
      ↓
Avaliação das métricas
      ↓
Seleção do modelo
      ↓
Previsão de um novo caso
      ↓
Verificação dos protocolos
      ↓
Saída padronizada
```

### Parte 2 — Sistema Multiagente

A segunda etapa dará continuidade ao resultado produzido na Parte 1.

A proposta é utilizar a previsão do modelo como entrada para uma arquitetura de agentes, permitindo que diferentes agentes trabalhem em conjunto para interpretar o risco e indicar os protocolos correspondentes.

---

## 🧠 Parte 1 — Modelo Preditivo

### 1. Objetivo

O objetivo da primeira parte é desenvolver um modelo de **classificação supervisionada** capaz de prever a variável:

```text
pico_risco
```

A variável representa a ocorrência ou não de um cenário classificado como pico de risco.

A base utilizada possui **5.000 registros simulados**, contendo informações relacionadas a pacientes, condições do sistema e disponibilidade de recursos.

---

### 2. Dados utilizados

As principais variáveis utilizadas na base são:

| Variável | Descrição |
|---|---|
| `idade_media` | Idade média considerada no cenário |
| `carga_sistema` | Carga atual do sistema |
| `tempo_espera` | Tempo de espera |
| `fc_media` | Frequência cardíaca média |
| `spo2_media` | Saturação de oxigênio média |
| `pa_sistolica` | Pressão arterial sistólica |
| `temperatura` | Temperatura média |
| `densidade_eventos` | Densidade de eventos |
| `eventos_24h` | Quantidade de eventos nas últimas 24 horas |
| `disponibilidade_recursos` | Disponibilidade de recursos |

A variável-alvo utilizada pelo modelo é:

```text
pico_risco
```

O `risk_score` utilizado durante a geração da variável-alvo não é utilizado como entrada do modelo, evitando que o modelo tenha acesso direto à informação utilizada para construir o resultado.

---

### 3. Preparação dos dados

Os dados são separados em:

- **X:** variáveis preditoras;
- **y:** variável-alvo `pico_risco`.

O conjunto de dados é dividido em:

- **80% para treinamento**
- **20% para teste**

A divisão utiliza `random_state=42` e estratificação da variável-alvo, mantendo a proporção das classes entre treinamento e teste.

No notebook, essa etapa é realizada antes do treinamento dos modelos. 

---

### 🔬 4. Modelos testados

Foram utilizados dois modelos de classificação supervisionada:

### Regressão Logística

Utilizada como modelo inicial de referência para o problema de classificação.

### Árvore de Decisão

Utilizada posteriormente para comparação com a Regressão Logística.

A comparação permite verificar qual modelo apresenta melhor desempenho na identificação dos casos de pico de risco.

---

### 📊 5. Avaliação dos modelos

Os modelos são avaliados utilizando:

- **Acurácia**
- **Precisão**
- **Recall**
- **F1-score**
- **Matriz de confusão**

Essas métricas são importantes porque o problema envolve uma classe de interesse específica: os casos classificados como **pico de risco**.

A matriz de confusão também permite observar os casos corretamente classificados e os erros cometidos pelo modelo.

---

### 🏆 6. Modelo selecionado

A **Árvore de Decisão** apresentou o melhor desempenho entre os modelos avaliados e foi utilizada para realizar a previsão do novo caso.

No conjunto de teste, a versão registrada no notebook apresenta:

```text
Acurácia: 99,70%

Pico de risco:
Precisão: 0,98
Recall:   0,96
F1-score: 0,97
```

A matriz de confusão registrada para a Árvore de Decisão foi:

```text
[[942   1]
 [  2  55]]
```

Esses resultados indicam um desempenho elevado na identificação das duas classes no conjunto de teste.

---

### 🧪 7. Simulação de um novo caso

Após o treinamento, o modelo é utilizado para analisar um novo cenário.

O caso de teste utilizado possui:

```text
idade_media:                62
carga_sistema:              92
tempo_espera:               75
fc_media:                   118
spo2_media:                 89
pa_sistolica:               100
temperatura:                37.5
densidade_eventos:          7
eventos_24h:                 5
disponibilidade_recursos:   25
```

Antes da previsão, as variáveis do novo caso são reorganizadas para manter a mesma ordem utilizada durante o treinamento do modelo.

A Árvore de Decisão então retorna:

1. a **classe prevista**;
2. a **probabilidade estimada de pico de risco**.

---

### 🚨 8. Protocolos operacionais

Além da previsão do modelo, o sistema verifica as condições do novo caso em relação aos protocolos operacionais definidos no enunciado.

Os protocolos são:

1. **SPO2 < 88%** → iniciar suporte respiratório imediato
2. **Carga do sistema > 85%** → abrir unidades temporárias
3. **FC > 120** → iniciar monitoramento intensivo
4. **Eventos_24h > 5** → ativar equipe de resposta rápida
5. **Disponibilidade de recursos < 30%** → redistribuir insumos
6. **Tempo de espera > 90 min** → priorizar triagem emergencial
7. **Temperatura > 38°C média** → iniciar protocolo infeccioso
8. **Densidade de eventos > 6** → ativar plano de contingência
9. **PA sistólica < 90** → iniciar protocolo de choque
10. **Risco sistêmico alto** → ativar estado de emergência

A aplicação dos protocolos é feita separadamente da previsão do modelo, permitindo que o resultado da classificação seja combinado com regras operacionais.

---

### 🔎 9. Resultado do caso de teste

Para o caso utilizado no notebook, o modelo retornou:

```text
RISCO PREVISTO: 100.00%
CLASSIFICAÇÃO: Pico de risco
```

Os protocolos acionados foram:

```text
- Carga do sistema > 85%: Abrir unidades temporárias.
- Disponibilidade de recursos < 30%: Redistribuir insumos.
- Densidade de eventos > 6: Ativar plano de contingência.
- Risco sistêmico alto: Ativar estado de emergência.
```

Os protocolos são acionados de acordo com os valores do novo caso e seus respectivos limiares.

---

### 💾 10. Salvamento do modelo

O modelo treinado é preparado para ser salvo e posteriormente reutilizado.

Essa etapa é importante porque a Parte 2 não deverá precisar treinar novamente o modelo toda vez que receber um novo caso.

A ideia é:

```text
Modelo treinado
      ↓
Salvamento
      ↓
Carregamento posterior
      ↓
Agente de análise de risco
```

Dessa forma, o modelo pode ser incorporado à arquitetura multiagente na próxima etapa do projeto.

---

### 🤖 11. Preparação para a Parte 2

A Parte 1 deixa preparada a principal entrada para o futuro sistema multiagente:

```text
Novo caso
   ↓
Modelo de Machine Learning
   ↓
Probabilidade de risco
   ↓
Classificação do risco
   ↓
Protocolos aplicáveis
```

Na Parte 2, essa saída poderá ser utilizada por um **agente de análise de risco**, que receberá o resultado do modelo e trabalhará em conjunto com os demais agentes do sistema.

A comunicação entre os agentes e a definição da arquitetura multiagente serão implementadas na continuação do projeto.

---

### 🔧 12. Estrutura esperada da integração

A arquitetura conceitual para a próxima etapa pode ser representada da seguinte forma:

```text
                    ┌─────────────────────┐
                    │    Novo paciente    │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │    Agente de ML     │
                    │                     │
                    │ Modelo treinado     │
                    │ Árvore de Decisão   │
                    └──────────┬──────────┘
                               │
                     Risco + classificação
                               │
                               ▼
                    ┌─────────────────────┐
                    │ Agente de protocolos│
                    │                     │
                    │ Regras operacionais │
                    └──────────┬──────────┘
                               │
                               ▼
                    ┌─────────────────────┐
                    │   Resposta final    │
                    │                     │
                    │ Risco previsto      │
                    │ Classificação       │
                    │ Protocolos sugeridos│
                    └─────────────────────┘
```

Essa representação é apenas a ponte conceitual entre as duas partes. A implementação da comunicação e dos agentes será realizada na Parte 2.

---

## 📁 Estrutura do projeto

A organização sugerida para o repositório é:

```text
CardioIA/
│
├── Cardio_IA.ipynb
├── README.md
│
└── Parte 2/
    └── ...
```

O arquivo principal da Parte 1 é:

```text
Cardio_IA.ipynb
```

Nele estão concentradas as etapas de geração/preparação dos dados, treinamento, avaliação, comparação dos modelos, previsão do novo caso e verificação dos protocolos.

---

## 🚀 Próximos passos — Parte 2

A partir deste ponto, o grupo pode dar continuidade ao projeto implementando:

- arquitetura multiagente;
- agente responsável pela análise do risco;
- agente responsável pela interpretação dos protocolos;
- comunicação entre os agentes;
- utilização do modelo de Machine Learning já treinado;
- integração da saída do modelo com os protocolos;
- geração da resposta final padronizada.

### Fluxo esperado

```text
DADOS DO NOVO CASO
        ↓
AGENTE DE MACHINE LEARNING
        ↓
RISCO PREVISTO
        ↓
AGENTE DE ANÁLISE / PROTOCOLOS
        ↓
PROTOCOLOS SUGERIDOS
        ↓
RESPOSTA FINAL DO SISTEMA
```

---

## ❤️ Status do projeto

### Parte 1 — Modelo Preditivo
**✅ Concluída**

- [x] Base de dados simulada
- [x] Definição da variável-alvo
- [x] Preparação dos dados
- [x] Separação treino/teste
- [x] Treinamento dos modelos
- [x] Avaliação dos modelos
- [x] Comparação entre modelos
- [x] Seleção da Árvore de Decisão
- [x] Matriz de confusão
- [x] Simulação de novo caso
- [x] Previsão de risco
- [x] Verificação dos protocolos
- [x] Preparação para integração

### Parte 2 — Sistema Multiagente
**⏳ Próxima etapa**

- [ ] Definição da arquitetura dos agentes
- [ ] Implementação do agente de Machine Learning
- [ ] Implementação do agente de protocolos
- [ ] Comunicação entre agentes
- [ ] Integração do modelo treinado
- [ ] Geração da resposta final
- [ ] Testes do sistema multiagente

---
