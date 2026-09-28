# Contexto fixo: Shopp Games e TCA Games

Leia isto antes de inserir jogos ou mexer em preços. Nunca gravar chaves, tokens ou senhas neste arquivo.

## Planilha
- Arquivo: PRECOS_25 (Excel no OneDrive)
- Pasta local do Leandro: `C:\Users\Leandro\OneDrive\TCA GAMES ONLINE`
- Essa pasta é do PC dele. A sessão em nuvem não enxerga o caminho. O acesso daqui é só pelo Make (conexão "OneDrive PRECOS").
- Todo SKU novo criado nas lojas precisa entrar na PRECOS, senão vira órfão no próximo ciclo de sincronização.

## Lojas
- Shopp Games (SG): shoppgames.com, loja mãe, cadastrar primeiro
- TCA Games (TCA): www.lojatcagames.com.br (myshopify: kxff0f-y8.myshopify.com), espelho da SG
- WordPress legado: lojatcagames.com
- SKU idêntico nas duas lojas. Padrão: `SIGLA-PLATAFORMA-PRI` e `SIGLA-PLATAFORMA-SEC`

## Make (us2.make.com, time "My Team", id 2710689)
Cenários sob demanda:
- 5958495 PRECOS - sincronizar lojas (PRECOS_25 para SG e TCA)
- 6407641 PRECOS - sincronizar WordPress
- 5952576 VITRINE - publicar dados.json
Conexões e chaves (só os nomes, valores ficam no Make):
- Conexões: Shopify SG (10454480), Shopify TCA (10454638), OneDrive PRECOS (10441109), HostGator vitrine (10441934)
- Chaves: "Shopify SG token (API Key)" (210282), "Shopify TCA token (API Key)" (210283), "WordPress lojatcagames" (230106)
- O MCP do Make não devolve os valores das chaves.

## Conector Shopify (MCP)
- Cada loja exige autenticação própria. Se pedir login numa sessão não interativa, o Leandro reautoriza em claude.ai > Configurações > Conectores.
- Em 28/09/2026 não havia `.env` nem variável de Shopify no container da sessão. Se o Leandro guardar chave em `.env`, ele precisa estar dentro do ambiente da sessão (secrets do ambiente em nuvem), e não commitado no repo.
- Sempre rodar `get-shop-info` antes de escrever, para confirmar em qual loja o conector está.

## Fluxo de cadastro de jogo
Usar a skill `inserir-produto-shopify-api`. Resumo das regras dele:
- Descrição nunca explica Primária/Secundária
- Vendor: `Sony` (PS4/PS5) ou `Microsoft` (Xbox)
- Status `DRAFT` até ele confirmar
- Pesquisar data de lançamento antes de decidir pré-venda
- Ordem: SG primeiro, TCA depois
