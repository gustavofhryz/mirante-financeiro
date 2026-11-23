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

## Como configurar os servidores MCP

Os dois servidores são usados no **Claude Desktop**, no computador. O Claude Desktop é o programa instalado, diferente do Claude no navegador.

### Pluggy (documentação)

1. No Claude Desktop, abra **Plugins** na barra lateral.
2. Clique em **Adicionar** e escolha **Adicionar conector personalizado**.
3. No campo de URL, cole: `https://mcp.pluggy.ai/mcp`
4. Dê o nome `Pluggy` e clique em **Continuar**.
5. Escolha **Sem login**, porque a documentação é pública. Clique em **Adicionar**.
6. Confirme que o conector aparece como **Conectado**.

Teste: em uma conversa nova, pergunte "o que é um Item na integração da Pluggy?". A resposta deve citar a documentação.

### Plaid (dados de teste)

O servidor da Plaid usa o `uv`, um gerenciador de pacotes Python, e é configurado no arquivo do Claude Desktop.

1. Instale o `uv` pelo PowerShell do Windows:
   ```
   winget install --id=astral-sh.uv -e
   ```
   Feche e reabra o PowerShell, e confira com `uvx --version`.

2. No Claude Desktop, abra **Configurações > Aplicativo para desktop > Extensões** e clique em **Editar configuração**. O arquivo `claude_desktop_config.json` será aberto.

3. Adicione o bloco abaixo dentro de `"mcpServers"`, substituindo os valores pelos do seu ambiente **Sandbox** (na Plaid, em **Build > Keys**):

   ```json
   {
     "mcpServers": {
       "plaid": {
         "command": "uvx",
         "args": ["mcp-server-plaid"],
         "env": {
           "PLAID_CLIENT_ID": "SEU_CLIENT_ID",
           "PLAID_SECRET": "SEU_SANDBOX_SECRET"
         }
       }
     }
   }
   ```

4. Salve o arquivo e feche o Claude Desktop pela bandeja do sistema (botão direito no ícone, **Sair**). Abra de novo.

5. Em **Extensões > Servidores MCP locais**, confira que o servidor `plaid` aparece como **Em execução**.

**Importante:** Client ID e Secret ficam apenas no arquivo de configuração do seu computador. Nunca os coloque no repositório, em prints ou no chat. Se o Secret for exposto, gere outro na Plaid (**Rotate**) e atualize o arquivo.

## A especificação enviada ao Lovable

Usei o prompt do guia (páginas 25 a 28), preenchido com o nome do app (Mirante), o nome da assistente (Mia) e a marca. Anexei o arquivo `transacoes_ficticias_plaid.json`.

No modo chat, respondi duas perguntas do Lovable:
- A lógica roda no frontend, sem backend.
- O valor de US$ 8.500,00 é tratado como saldo inicial, e o saldo atual é calculado a partir das movimentações.

**Ajustes feitos depois da primeira versão:**
- Corrigi o saldo atual, que exibia o saldo inicial (US$ 8.500,00) em vez do saldo calculado (US$ 13.614,91).
- Corrigi um erro de execução na rolagem automática do chat (`scrollIntoView` dentro de `useEffect`).

## Limitações

- Dados fictícios, sem conexão com um banco real.
- Sem pagamentos, transferências ou operações de escrita.
- Etapa 2 (backend com a Plaid) ainda não ativada.
