# Painel de análise de vendas

Dashboard estático para a atividade de Fundamentos de IA e Análise de Dados. Usa HTML, CSS e JavaScript puro; não precisa de servidor, banco de dados ou compilação.

A interface usa tema retrô escuro, com a identidade **Platini Consultor**.

## Abrir e usar

1. Baixe ou clone o projeto.
2. Abra `public/index.html` no navegador. Os dados de demonstração já estão incorporados no JavaScript, então a página não precisa buscar o CSV local.
3. Use os filtros de data, categoria e região. A pesquisa da tabela também se aplica aos indicadores e gráficos para manter os resultados alinhados.
4. Selecione **Importar CSV** para trocar a base. O arquivo é lido localmente no navegador e não é enviado a um servidor.
5. Selecione **Exportar CSV** para baixar as linhas que correspondem aos filtros e à pesquisa, com a coluna calculada `faturamento_linha`.

O dashboard valida datas e números. Linhas com ID, data, produto, categoria, quantidade ou preço ausentes ou inválidos são descartadas e os motivos aparecem na página. Quantidade e preço iguais a zero são valores válidos; não são confundidos com dados ausentes. IDs repetidos não causam descarte: a quantidade de vendas é contada como número de IDs únicos, e cada linha continua representando um produto vendido.

## Dados disponíveis

O projeto já continha `vendas_hamburgueria.csv`, com 1.367 linhas, datas de 1º de julho a 30 de setembro de 2026 e colunas `id_venda`, `data`, `dia_semana`, `periodo`, `produto`, `categoria`, `quantidade`, `preco_unitario`, `forma_pagamento`, `canal`, `tempo_espera_min` e `avaliacao`. O arquivo original foi preservado. Uma cópia está em `public/dados-exemplo.csv`, e o mesmo conteúdo está incorporado em `public/script.js` para a demonstração abrir imediatamente.

Essa base é fornecida para a atividade e serve como demonstração; não foi criada uma base fictícia nova. Ela não contém `regiao`, portanto o filtro fica desativado e o gráfico regional explica essa ausência. Para comparar regiões, importe uma base com a coluna `regiao`; ela é opcional para os demais cálculos. Também não há doze meses na base disponível, por isso a evolução inicial cobre os três meses presentes.

### Formato para uma base própria

As colunas obrigatórias são `id_venda`, `data`, `produto`, `categoria`, `quantidade` e `preco_unitario`. `regiao` é opcional, assim como outras colunas, que são mantidas na tabela e no CSV exportado. Use uma linha por produto vendido, IDs de venda não vazios, datas em `AAAA-MM-DD` (também é aceito `DD/MM/AAAA`) e números não negativos. O importador aceita ponto decimal ou vírgula decimal. Números e valores monetários são apresentados no padrão brasileiro.

## Indicadores e gráficos

- **Faturamento da linha:** quantidade × preço unitário.
- **Faturamento total:** soma do faturamento das linhas filtradas.
- **Quantidade de vendas:** contagem de `id_venda` únicos entre as linhas filtradas.
- **Unidades vendidas:** soma das quantidades.
- **Ticket médio:** faturamento total ÷ quantidade de vendas. Se não houver venda, o resultado é R$ 0,00.
- **Faturamento mensal, por categoria e por região:** soma do faturamento das linhas correspondentes. Registros sem região, quando existirem, continuam nos totais e indicadores, mas não podem ser associados a uma região no gráfico.
- **Produtos por faturamento:** mostra até os dez produtos com maior faturamento na seleção atual.
- **Unidades por categoria:** soma as quantidades por categoria.
- **Faturamento por canal e por forma de pagamento:** disponíveis quando a base contém as colunas `canal` e `forma_pagamento`.

A pesquisa da tabela é combinada aos filtros de período, categoria e região e atualiza a tabela, os cartões e os gráficos. Nenhuma linha é removida por duplicidade; apenas registros que não podem ser calculados pelos motivos informados são descartados.

## Bibliotecas e internet

Os gráficos usam Chart.js **4.4.3** e a leitura principal de CSV usa Papa Parse **5.4.1**, carregados por CDN com versões fixadas. Essas bibliotecas e a fonte tipográfica remota exigem acesso à internet. O CSV de demonstração é incorporado na página para funcionar também em `file://`, e há um parser simples de reserva para carregar os dados quando Papa Parse estiver indisponível; sem internet, os gráficos não são desenhados.

## Publicar no Render pelo GitHub

Envie o projeto a um repositório no GitHub e, no Render, crie um serviço **Static Site** conectado a esse repositório. Configure:

- **Tipo de serviço:** Static Site
- **Branch:** a branch que contém o projeto
- **Root Directory:** vazio
- **Build Command:** `echo "Site pronto"`
- **Publish Directory:** `public`

O Render publica os arquivos estáticos dessa pasta; não há etapa de build nem serviço de backend.
