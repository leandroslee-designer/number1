---
name: cadastrar-jogo-nas-lojas
description: Use quando o Leandro passar jogo(s) com plataforma e preços e pedir para inserir na planilha e nas lojas (Shopp Games e TCA Games). Acionar com frases como "inserir jogos na planilha e no site", "cadastrar esse jogo", "subir esses jogos", ou quando vier só nome do jogo + plataforma + preço primária/secundária. Cobre também pedido de nova variante (ex.: Secundária) em produto que já existe. Orquestra o ciclo completo e delega as regras de cada etapa às skills existentes.
---

# Cadastrar jogo nas lojas

Ciclo completo: loja mãe, espelho, planilha, conferência. Ler o `CLAUDE.md` da raiz antes (IDs, lojas, Make).
Nunca gravar chave, token ou senha em arquivo, commit ou resposta.

## Entrada esperada
Uma linha por jogo: `NOME PLATAFORMA primária X secundária Y`.
- Só um preço informado: criar só a Primária e confirmar antes de assumir a outra.
- "Variação de conta X no [jogo] que já tem no site": não criar produto. Achar o existente por título ou SKU e criar só a variante nova (`productVariantsBulkCreate`, opção `Estilo`).

## Passo 0: confirmar acessos (parar se falhar)
1. `get-shop-info` no conector Shopify. Se pedir login, avisar o Leandro para reautorizar em claude.ai > Configurações > Conectores e parar. Não tentar contornar.
2. Confirmar em qual loja o conector caiu (SG primeiro, TCA depois).
3. Planilha PRECOS_25 (OneDrive, pasta `TCA GAMES ONLINE`): só acessível pelo Make ou por arquivo que o Leandro suba. Se não houver caminho de escrita, entregar as linhas prontas para colar e dizer isso.

## Passo 1: pesquisar cada jogo
- Data de lançamento (pré-venda só se for futura), plataformas reais, gênero, página oficial (PlayStation Store BR ou Xbox).
- Não criar versão PS4 "por simetria".

## Passo 2: Shopp Games (loja mãe)
Seguir a skill `inserir-produto-shopify-api` (passo 0 de inspeção, productCreate, variantes, verificação). Pontos que mais erram:
- Status `DRAFT`. Só `ACTIVE` com confirmação explícita.
- Vendor `Sony` (PS4/PS5) ou `Microsoft` (Xbox).
- Descrição sem explicar Primária/Secundária.
- Preço com ponto na API (`149.90`).
- `sku` dentro de `inventoryItem`.
- Produto digital: `taxable false`, `inventoryPolicy CONTINUE`, `tracked false`, `requiresShipping false`.
- Definir SKU `SIGLA-PLATAFORMA-PRI` e `-SEC` e mostrar ao Leandro antes de criar. O mesmo SKU vale nas duas lojas e na planilha.

## Passo 3: TCA Games (espelho)
`switch-shop`, `get-shop-info`, reinspecionar o padrão da loja, trocar o domínio do link interno na descrição, repetir com SKU idêntico.

## Passo 4: planilha PRECOS_25
- Ler o cabeçalho e uma linha de exemplo antes de escrever. Não assumir colunas.
- Incluir SKU novo, ids (product_id, variant_id), preço atual e alvo, conforme o padrão da planilha.
- Rodar o cenário Make "PRECOS - sincronizar lojas" (5958495) só se o Leandro pedir. Ele altera preços nas duas lojas e consome operações.

## Passo 5: conferir
Skill `conferir-lojas-api`: somente leitura, confronta planilha e loja por SKU.

## Fecho
Responder em bullets curtos:
- Por jogo e por loja: criado, variante adicionada ou falhou.
- SKUs criados e o que entrou na planilha.
- Alt texts das capas, se as imagens ficarem para depois.
- Próximo passo (imagens, publicar, sincronizar).
