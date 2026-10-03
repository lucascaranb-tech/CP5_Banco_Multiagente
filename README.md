CP5 - Banco Multiagente

Banco digital fictício atendido por uma equipe de agentes de IA, construído com o OpenAI Agents SDK. Um agente de atendimento entende o pedido do cliente em linguagem natural e chama o agente especialista certo. Os dados são simulados e ficam em arquivos CSV.

Projeto do CP5 (FIAP).

Grupo: Lucas Caram, Mauricio Bertuci Saletti, Nicolas Andrade Rodrigues, Leonardo Fortini Marcelo e Rhuan Pacheco Carreri.

Agentes
Agente	Função
AgenteAtendimento	Triagem: identifica a intenção e chama o especialista (ou vários, se a mensagem pedir mais de uma coisa)
AgentePix	Saldo, extrato, contatos e PIX
AgenteInvestimentos	Consultar, aplicar e resgatar investimentos
AgenteCartao	Informa se o cliente tem cartão de crédito e o valor pré-aprovado
AgenteDuvidas	Dúvidas gerais sobre produtos, tarifas e regras do banco
Funcionalidades

Conta e PIX

Consulta de saldo da conta corrente
Listar, buscar e adicionar contatos de PIX
PIX para contatos salvos, com comprovante em imagem
Vários PIX na mesma mensagem

Investimentos

Três produtos: CDB Baixa Automática, CDB DI e Poupança
Aplicar: o dinheiro sai do saldo da conta
Resgatar: o dinheiro volta para a conta
Consultar o que está investido em cada produto

Extrato e comprovantes

Extrato com as últimas movimentações (PIX, aplicações, resgates e baixas automáticas), em texto e em imagem
Comprovantes em imagem para PIX, aplicação e resgate

Cartão de crédito

Consulta de cartão e valor pré-aprovado

Segurança

Guardrail de entrada: mensagens que mencionam senha, cpf, número do cartão ou cvv são bloqueadas antes de chegar aos agentes

Canais

Execução no notebook (Colab)
Bot no Telegram, com os comprovantes e o extrato enviados como foto (e como arquivo, se a foto falhar)
Regras dos investimentos
Produto	Uso nos pagamentos	Resgate
CDB Baixa Automática	Automático: se o saldo em conta não cobre um PIX, a diferença sai dele	Permitido, mas opcional
Poupança	Igual ao CDB Baixa Automática	Permitido, mas opcional
CDB DI	Nunca é usado automaticamente	Obrigatório para o dinheiro voltar à conta
Na baixa automática, o CDB Baixa Automática é usado primeiro e depois a Poupança.
Se o saldo em conta somado à baixa automática não cobre o PIX, ele é recusado, e o sistema avisa quando há valor no CDB DI que precisa ser resgatado.
Não existe depósito externo, nem direto na conta corrente nem nas aplicações. O cliente investe e resgata livremente, mas só com o saldo que já tem.
Dados (CSV)

Os arquivos ficam na pasta dados/ deste repositório. O notebook lê e grava em /content/drive/MyDrive/IA, então copie os arquivos para essa pasta do seu Google Drive.

Arquivo	Conteúdo
contatos_pix.csv	Contatos de PIX do cliente (obrigatório: o notebook não cria este arquivo)
saldo.csv	Saldo da conta corrente
investimentos.csv	Valor aplicado em cada produto
movimentacoes.csv	Histórico que alimenta o extrato
transacoes_pix.csv	Registro dos PIX realizados
cartao_credito.csv	Cartão e valor pré-aprovado

Com RESETAR_BASE = True (célula de base de dados), o notebook recria saldo, investimentos, movimentações e PIX com R$ 15.000,00 de saldo inicial e guarda cópias .bak_ dos arquivos anteriores. Depois da primeira execução, mude para False para o saldo persistir entre execuções.

Como executar
Abra CP5_Banco_Multiagente_c_c.ipynb no Google Colab.
Em Secrets (ícone de chave), crie OPENAI_API_KEY e, para o bot, TELEGRAM_BOT_TOKEN, ativando o acesso ao notebook.
Copie os CSVs de dados/ para MyDrive/IA no seu Drive.
Rode as células em ordem. A seção "Testando o fluxo completo" executa um roteiro de conversas de exemplo.

As chaves não ficam no código: são lidas dos Secrets do Colab.

Exemplos de mensagens
"Qual o meu saldo?"
"Faça um PIX de R$ 50 para a Ana"
"Quero aplicar R$ 5.000 no CDB DI"
"Quais são os meus investimentos?"
"Resgate R$ 1.000 do CDB DI"
"Me mostre o extrato"
"Eu tenho cartão de crédito?"
Tecnologias

Python, OpenAI Agents SDK (gpt-4o-mini), pandas, Pillow, python-telegram-bot e Google Colab.
