# Mirante Financeiro

Chat de demonstração para consultar contas, saldos e transações com a assistente Mia.
Todos os dados são fictícios e vêm da Plaid Sandbox.

**App publicado:** https://mirante-balance-viewer.lovable.app/

**Data de referência dos dados:** 20/11/2026

## Onde o cliente já está
O chat aparece dentro de um app de banco digital, como uma aba de conversa em que o cliente consulta as próprias finanças sem sair do aplicativo.

## O que o assistente responde
- Contas, saldos e dinheiro disponível
- Últimas transações
- Resumo de gastos por categoria
- Recusa de perguntas fora dos dados (ex.: previsão do tempo)

## De onde vem cada dado
- **Saldos e transações:** arquivo `transacoes_ficticias_plaid.json`, com 30 registros de 30/09/2026 a 20/11/2026, gerados a partir da Plaid Sandbox.
- **Categorias:** atribuídas no arquivo para permitir o resumo por categoria. A Plaid não fornece essa categoria neste recorte.
- **Cálculos:** saldo atual = saldo inicial + entradas − saídas. Valores positivos são saídas e valores negativos são entradas.

## Como configurei os servidores MCP
- **Pluggy:** conector personalizado no Claude Desktop, com a URL `https://mcp.pluggy.ai/mcp`, sem autenticação. Usado para consultar a documentação.
- **Plaid:** servidor `mcp-server-plaid`, executado com `uvx`. Client ID e Secret do ambiente Sandbox ficam no bloco `env` do arquivo de configuração do Claude Desktop, que existe apenas no meu computador. As chaves não estão neste repositório.

## Especificação enviada ao Lovable
Usei o prompt do guia (páginas 25 a 28), preenchido com o nome do app (Mirante), o nome da assistente (Mia) e a marca. Anexei o arquivo de transações. No modo chat, respondi duas perguntas: a lógica roda no frontend, sem backend, e o valor de US$ 8.500,00 é tratado como saldo inicial, com o saldo atual calculado a partir das movimentações.

**Ajustes feitos depois da primeira versão:**
- Corrigi o saldo atual, que exibia o saldo inicial (US$ 8.500,00) em vez do saldo calculado (US$ 13.614,91).
- Corrigi um erro de execução na rolagem automática do chat (`scrollIntoView` dentro de `useEffect`).

## Melhoria escolhida
Respostas por período: a Mia passa a aceitar perguntas como "quanto gastei em alimentação em outubro?", filtrando as transações pelas datas citadas.

## Limitações
- Dados fictícios, sem conexão com um banco real.
- Sem pagamentos, transferências ou operações de escrita.
- Etapa 2 (backend com a Plaid) ainda não ativada.
