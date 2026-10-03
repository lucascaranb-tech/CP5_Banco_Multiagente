# 🏦 CP5 — Banco Multiagente com IA

Projeto desenvolvido para o **CP5 da FIAP**, com a proposta de criar um banco digital fictício em que o atendimento é realizado por uma equipe de agentes de IA.

A ideia central é utilizar o **OpenAI Agents SDK** para transformar uma mensagem em linguagem natural em uma intenção, encaminhando o cliente para o agente especialista responsável pela operação.

## 🤖 Como funciona

O fluxo principal é:

**Cliente → Agente de Atendimento → Handoff → Agente especialista → Tools → Dados persistidos**

O `AgenteAtendimento` funciona como uma camada de triagem. Ele identifica a intenção do cliente e realiza o handoff para o especialista adequado.

No notebook atual, estão implementados:

- 💸 **PIX e contatos** — listar, buscar e adicionar contatos e realizar PIX simulados;
- 💰 **Consulta de saldo** — leitura do saldo diretamente do CSV;
- ❓ **Dúvidas gerais** — agente especializado para perguntas sobre produtos, serviços, tarifas e regras do banco fictício;
- 🛡️ **Guardrail de entrada** — bloqueio de solicitações que contenham dados sensíveis, como senha, CPF e cartão de crédito;
- 🧠 **Memória de sessão** — utilização de `SQLiteSession` para manter o contexto entre chamadas;
- 💾 **Persistência em CSV** — saldo, contatos e transações permanecem registrados em arquivos, em vez de ficarem apenas em memória.

## 🔐 Segurança

A chave da OpenAI não fica escrita diretamente no código. O notebook utiliza o sistema de **Secrets do Google Colab** para recuperar a variável `OPENAI_API_KEY` durante a execução:

```python
from google.colab import userdata

os.environ["OPENAI_API_KEY"] = userdata.get("OPENAI_API_KEY")
```

Dessa forma, a chave não precisa ser publicada no repositório.

Além disso, o projeto possui um guardrail de entrada que bloqueia solicitações contendo termos relacionados a dados sensíveis e testa também um cenário simples de tentativa de prompt injection.

## 💾 Persistência dos dados

O projeto utiliza três arquivos CSV:

| Arquivo | Função |
|---|---|
| `saldo.csv` | Saldo atualizado do cliente |
| `contatos_pix.csv` | Contatos salvos para PIX |
| `transacoes_pix.csv` | Histórico das transações realizadas |

Ao realizar um PIX, o sistema verifica o contato, valida o saldo, atualiza o arquivo de saldo e registra a transação no histórico.

## 🧪 Testes realizados

O notebook contém testes para demonstrar o fluxo completo, incluindo:

1. Consulta dos contatos salvos;
2. Consulta do saldo;
3. Realização de PIX;
4. Conferência do saldo após a operação;
5. Adição de um novo contato;
6. Novo PIX para o contato adicionado;
7. Consulta de dúvidas gerais;
8. Teste do guardrail com tentativa de obtenção de dados sensíveis;
9. Conferência da persistência dos dados após as operações.

## 🧰 Tecnologias

- **Python**
- **OpenAI Agents SDK**
- **OpenAI API — `gpt-4o-mini`**
- **Pandas**
- **CSV** para persistência
- **SQLiteSession** para memória de sessão
- **Google Colab**

## ▶️ Como executar

O projeto foi desenvolvido para execução no **Google Colab**.

1. Abra o arquivo [`CP5_Banco_Multiagente.ipynb`](./CP5_Banco_Multiagente.ipynb);
2. Configure `OPENAI_API_KEY` nos Secrets do Google Colab;
3. Execute a instalação do `openai-agents`;
4. Faça o upload dos arquivos `saldo.csv`, `contatos_pix.csv` e `transacoes_pix.csv` para o ambiente do Colab;
5. Execute as células em ordem;
6. Rode os testes do fluxo completo e do guardrail.

> **Observação:** os arquivos CSV necessários para execução não foram anexados nesta conversa junto ao notebook. Caso estejam disponíveis no projeto original, eles devem ser colocados na mesma pasta do notebook antes da execução.

## 🎥 Demonstração

O projeto também conta com uma demonstração em vídeo mostrando os testes e o funcionamento do banco multiagente.

**Demo:** vídeo de testes do projeto — a ser adicionado ao repositório.

## 📌 Contexto do projeto

Este trabalho representa uma evolução do CP4, substituindo os dados mantidos apenas em memória por persistência em CSV e adicionando ferramentas de gerenciamento de contatos e memória de sessão.

A parte mais interessante do projeto foi transformar regras de negócio em ferramentas que os agentes conseguem utilizar de forma controlada, mantendo a separação de responsabilidades entre atendimento, especialista de PIX e especialista de dúvidas.

## 👥 Equipe

Projeto desenvolvido em grupo para a FIAP.

- Lucas Caram
- Mauricio Bertuci Saletti
- Nicolas Andrade Rodrigues
- Leonardo Fortini Marcelo
- Rhuan Pacheco Carreri

## 📱 Sobre a publicação

Este projeto também será apresentado no LinkedIn como parte do meu portfólio acadêmico e profissional, acompanhado de um vídeo de demonstração.

A proposta é mostrar, na prática, como **agentes de IA, handoffs, tools, guardrails, persistência e memória de sessão** podem ser combinados para construir um fluxo de atendimento bancário fictício.

---

**FIAP • CP5 • Inteligência Artificial • Python • OpenAI Agents SDK**

#IA #AgentesDeIA #Python #OpenAI #FIAP #Fintech