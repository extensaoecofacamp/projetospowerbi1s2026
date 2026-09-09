# As Contas Nacionais Trimestrais (CNT)
## O que são e por que existem?
As contas trimestrais servem para dar um diagnóstico rápido da economia (o famoso PIB) sem ter que esperar o ano acabar

Elas são como um "exame de sangue" frequente, enquanto as contas anuais são um "check-up" completo e muito mais detalhado que demora mais para sair.
## A "Balança" da Economia (Oferta vs. Demanda)
  O IBGE usa uma ferramenta chamada Tabela de Recursos e Usos (TRU) para garantir que tudo o que foi produzido ou importado no país (Oferta) seja igual a tudo o que foi consumido, investido ou exportado (Demanda)

- Oferta: Produção nacional + Importações

- Demanda: Consumo das famílias + Gastos do governo + Investimentos das empresas (Máquinas e Construção) + Exportações

## De onde vêm os dados? (As Fontes)

Como o IBGE precisa de pressa, ele usa indicadores mensais e trimestrais que servem como "termômetros" de cada setor

- Agropecuária: Usa pesquisas sobre a previsão de safra (como o LSPA) e o abate de animais
- Indústria: A principal fonte é a Pesquisa Industrial Mensal (PIM-PF), que mede a produção física das fábricas
- Serviços: Usa a Pesquisa Mensal de Serviços (PMS) e dados de emprego da PNAD Contínua para medir setores como comércio, transportes e tecnologia
- Governo e Impostos: São usados dados do Tesouro Nacional (SIAFI) e da Receita Federal sobre a arrecadação
  
## Ajustes especiais

Para que os números façam sentido, o IBGE faz dois ajustes fundamentais:
- Ajuste Sazonal: Alguns eventos acontecem todo ano, como o aumento das vendas no Natal ou a colheita de certas safras O IBGE retira esse "efeito calendário" para que possamos comparar, por exemplo, o 4º trimestre com o 3º trimestre sem que o Natal distorça o crescimento real

- Ajuste ao Total Anual: Quando os dados anuais (mais completos) ficam prontos, os trimestres são recalculados para que a soma dos quatro pedaços bata exatamente com o total do ano

 ## Revisões: Por que os números mudam?

 É comum ver notícias de que o PIB de um trimestre passado foi revisado. Isso acontece porque as CNT são feitas com dados incompletos para garantir rapidez. À medida que informações mais precisas chegam, o IBGE atualiza os cálculos para refletir a realidade mais fiel possível


# PIB Trimestral do Brasil — Roteiro Power BI

**PIB TRIMESTRAL DO BRASIL**
Análise de Crescimento e Composição

*Roteiro completo para construção do dashboard no Power BI*

Fonte: IBGE/SIDRA • Série 1621 • A partir de 1996

---

## Visão Geral do Projeto

Este roteiro descreve, passo a passo, a construção de um dashboard de análise do Produto Interno Bruto (PIB) Trimestral do Brasil no Power BI. O projeto cobre desde a extração dos dados oficiais do IBGE até a publicação do painel interativo.

O dashboard é composto por onze visuais organizados em uma única página e permite ao usuário acompanhar a evolução do PIB, comparar setores e identificar tendências de crescimento ou retração ao longo do tempo.

### Estrutura do Projeto

O trabalho está organizado em sete etapas sequenciais. Cada etapa depende da anterior — por isso é importante seguir a ordem indicada:

| Etapa | O que é feito | Resultado |
| --- | --- | --- |
| 1. Extração de dados | Acesso ao portal SIDRA/IBGE e cópia do link da API | Link da API pronto para uso |
| 2. Power Query | Conexão, limpeza e transformação dos dados brutos | Tabela PIB trimestral limpa |
| 3. Modelagem | Criação das tabelas de suporte e relacionamentos | Modelo de dados estruturado |
| 4. Medidas DAX | Cálculo de variações, índices e participações | Todas as métricas do dashboard |
| 5. Visuais | Inserção e configuração dos gráficos e tabelas | Dashboard visualmente montado |
| 6. Interações | Controle de quais visuais respondem aos filtros | Comportamento correto dos filtros |
| 7. Finalização | Formatação, organização e publicação | Dashboard publicado e atualizado |

### Dados Utilizados

Os dados vêm da Pesquisa Trimestral do PIB do IBGE, disponibilizados pelo sistema SIDRA (Sistema IBGE de Recuperação Automática) na tabela de código 1621. A série cobre todos os trimestres desde o primeiro trimestre de 1996 e é atualizada trimestralmente pelo IBGE.

São utilizados nove indicadores, que representam as duas grandes óticas de medição do PIB:

| Código | Indicador | Ótica |
| --- | --- | --- |
| 90707 | PIB a preços de mercado | — |
| 90687 | Agropecuária - total | Oferta |
| 90691 | Indústria - total | Oferta |
| 90696 | Serviços - total | Oferta |
| 93404 | Despesa de consumo das famílias | Demanda |
| 93405 | Despesa de consumo da administração pública | Demanda |
| 93406 | Formação bruta de capital fixo | Demanda |
| 93407 | Exportação de bens e serviços | Demanda |
| 93408 | Importação de bens e serviços (-) | Demanda |

> **Dica:** Os códigos numéricos de cada indicador são essenciais para escrever as medidas DAX com precisão. Eles são usados extensivamente na Seção 4 para filtrar exatamente o valor correto dentro da tabela de dados.

---

## 1. Extração de Dados — SIDRA/IBGE

O primeiro passo é obter os dados diretamente no portal SIDRA do IBGE. Em vez de baixar um arquivo, vamos copiar o link da API — isso permite que o Power BI atualize os dados automaticamente no futuro, sempre que você clicar em Atualizar.

### 1.1 Acessar a tabela correta

- Abra o navegador e acesse: https://sidra.ibge.gov.br
- No menu de navegação, localize a seção **Contas Nacionais Trimestrais** e clique nela.
- Na lista de tabelas disponíveis, localize a tabela **1621 — Produto interno bruto a preços de mercado, impostos, líquidos de subsídios, sobre produtos acumulado nos últimos quatro trimestres** e clique nela.
- Clique no ícone de link (representado por uma caixa com uma seta diagonal) localizado ao lado do título da tabela. Isso abre o painel de configuração da consulta via API.

### 1.2 Configurar a consulta

- Na seção **Variáveis**, mantenha todas as opções selecionadas.
- Na seção **Setores e subsetores**, clique em **Selecionar todos** para incluir todos os nove indicadores listados na tabela da introdução.
- Na seção **Trimestre**, clique em **Selecionar todos** para incluir todos os trimestres disponíveis desde 1996.
- Role a página até o final e clique no botão **Links de Compartilhar**.
- Copie o primeiro link exibido, identificado como **Parâmetros para a API**. Esse link será colado no Power BI na próxima etapa.

> **Dica:** O link da API é um endereço web que, quando acessado, retorna os dados em formato estruturado (JSON). O Power BI consegue ler esse formato diretamente, o que elimina a necessidade de baixar planilhas manualmente a cada atualização do IBGE.

---

## 2. Tratamento dos Dados no Power Query

Com o link da API em mãos, vamos conectar o Power BI à fonte de dados e preparar a tabela para uso. O Power Query é o editor de transformação de dados do Power BI — é aqui que limpamos, reorganizamos e tipamos os dados antes de qualquer cálculo.

### 2.1 Conectar à fonte de dados

- Abra o Power BI Desktop e crie um novo relatório em branco.
- No menu superior, acesse: **Início → Obter Dados → Web**.
- Na janela que abrir, cole o link copiado do SIDRA no campo URL e clique em **OK**.
- Se aparecer uma mensagem de erro, clique em **OK** novamente — a conexão será estabelecida normalmente na segunda tentativa.
- O Power Query Editor abrirá automaticamente com uma pré-visualização dos dados carregados.

### 2.2 Usar a primeira linha como cabeçalho

- Com a tabela carregada, clique em **Usar a Primeira Linha como Cabeçalho** na aba **Página Inicial**.

Os dados do SIDRA chegam sem cabeçalho definido — o Power BI numera as colunas como Coluna1, Coluna2 etc. Esse passo substitui esses nomes genéricos pelos nomes reais das variáveis do IBGE, como `Trimestre`, `Setores e subsetores` e `Valor`.

### 2.3 Renomear a consulta

- No painel lateral direito, em **Configurações de Consulta**, localize o campo **Nome** e altere o valor para: `PIB trimestral`.

Esse nome será o identificador da tabela em todo o modelo — ele aparece no painel de dados, nos relacionamentos e nas fórmulas DAX. Definir um nome claro agora evita confusão nas etapas seguintes.

### 2.4 Remover colunas desnecessárias

A tabela bruta do SIDRA inclui várias colunas que repetem a mesma informação em todas as linhas — por exemplo, a coluna `Brasil`, que contém o valor "Brasil" em todas as linhas, e `Nível Territorial (Código)`, que sempre vale 1. Essas colunas aumentam o tamanho do arquivo sem contribuir para a análise.

Mantenha apenas as cinco colunas a seguir e remova todas as demais clicando com o botão direito no cabeçalho da coluna e selecionando **Remover**:

| Coluna a manter | Função no projeto |
| --- | --- |
| Valor | Contém os números do PIB — base de todos os cálculos |
| Trimestre (Código) | Código no formato AAAA0Q usado para criar a data trimestral |
| Trimestre | Nome legível do período, usado em visuais quando necessário |
| Setores e subsetores (Código) | Código numérico do setor, usado como filtro nas medidas DAX |
| Setores e subsetores | Nome do setor, exibido nos eixos dos gráficos e nas matrizes |

### 2.5 Definir os tipos de dados

O tipo de dado de cada coluna determina como o Power BI interpreta e processa a informação. Uma coluna de texto tratada como número — ou vice-versa — gera erros silenciosos que só aparecem nos cálculos.

| Coluna | Tipo correto | Motivo |
| --- | --- | --- |
| Valor | Número Decimal | Permite somas, médias e divisões nas medidas DAX |
| Trimestre (Código) | Texto | É um código alfanumérico, não um número somável |
| Trimestre | Texto | Nome descritivo — não realiza cálculos |
| Setores e subsetores (Código) | Texto | Código numérico usado como identificador, não como valor |
| Setores e subsetores | Texto | Nome do setor para exibição em visuais |

- Para alterar o tipo de qualquer coluna, clique no ícone à esquerda do nome da coluna (que mostra ABC, 123 ou outro símbolo) e selecione o tipo desejado.
- Para a coluna **Valor** especificamente, siga este caminho para evitar erros de interpretação decimal: clique no ícone da coluna → **Usando a Localidade...** → Tipo de Dados: **Número Decimal** → Localidade: **Inglês (Estados Unidos)**. Esse cuidado é necessário porque os dados do IBGE usam ponto como separador decimal no padrão americano, e sem essa configuração o Power BI pode interpretar os valores incorretamente.

### 2.6 Criar a coluna de data trimestral

Para que a tabela de dados se conecte corretamente à tabela calendário que criaremos na próxima etapa, precisamos de uma coluna de data real. O problema é que o IBGE não fornece datas — ele fornece um código no formato AAAA0Q (por exemplo: `199601` para o primeiro trimestre de 1996).

A fórmula a seguir lê esse código, extrai o ano e o trimestre, e converte para uma data real representando o primeiro dia do trimestre correspondente (01/01, 01/04, 01/07 ou 01/10):

- Na aba **Adicionar Coluna**, clique em **Coluna Personalizada**.
- No campo **Nome da Nova Coluna**, digite: `Data Trimestral`
- No campo **Fórmula de Coluna Personalizada**, cole o código abaixo:

```powerquery-m
let
    Ano = Text.Start(Text.From([#"Trimestre (Código)"]), 4),
    Mes = if Text.End(Text.From([#"Trimestre (Código)"]), 2) = "01" then "01"
          else if Text.End(Text.From([#"Trimestre (Código)"]), 2) = "02" then "04"
          else if Text.End(Text.From([#"Trimestre (Código)"]), 2) = "03" then "07"
          else "10"
in
    Date.FromText(Ano & "-" & Mes & "-01")
```

- Clique em **OK**. A coluna **Data Trimestral** aparecerá com datas no formato 01/01/AAAA, representando o início de cada trimestre.
- Clique em **Fechar e Aplicar** para salvar todas as transformações e carregar os dados no modelo do Power BI.

> **Dica:** A coluna Data Trimestral é o elo entre a tabela de dados e a tabela calendário. Sem ela, as funções de inteligência de tempo do DAX — como `DATEADD` e `SAMEPERIODLASTYEAR` — não funcionam.

---

## 3. Modelagem — Tabelas e Relacionamentos

Com os dados importados e tratados, é hora de construir a estrutura que sustenta todo o dashboard. Nesta etapa criamos três tabelas adicionais e definimos como elas se conectam à tabela principal. Esse conjunto de tabelas e relacionamentos é chamado de **modelo de dados**.

Um modelo bem construído é o que permite que filtros aplicados em um visual se propaguem corretamente para os demais e que as fórmulas DAX calculem sempre o valor certo no contexto certo.

### 3.1 Tabela Calendário — `d_Calendario`

A tabela calendário é uma tabela auxiliar que lista todos os dias do período de análise, com colunas adicionais que organizam as datas por ano, mês e trimestre. Ela é obrigatória para o uso das funções de inteligência de tempo do DAX.

Para criá-la, vá ao ícone de **Exibição de Tabela** no painel esquerdo, certifique-se de que nenhuma tabela esteja selecionada e clique em **Nova Tabela** na faixa superior. Cole o código a seguir na barra de fórmulas:

```dax
d_Calendario =
VAR DataInicial = DATE(1996, 01, 01)
VAR DataFinal   = DATE(YEAR(TODAY()), 12, 31)
RETURN
ADDCOLUMNS(
    CALENDAR(DataInicial, DataFinal),
    "ano",      YEAR([Date]),
    "ano trim", YEAR([Date]) * 100 + QUARTER([Date]),
    "mes nome", FORMAT([Date], "MMMM"),
    "mes num",  MONTH([Date]),
    "trim/ano", QUARTER([Date]) & "º Trim " & YEAR([Date])
)
```

A fórmula cria uma linha para cada dia entre 01/01/1996 e 31/12 do ano atual, e adiciona colunas calculadas com o ano, o número e o nome do trimestre. A coluna `trim/ano` (por exemplo: "1º Trim 2023") é usada como eixo nos gráficos de evolução trimestral.

### 3.2 Tabela de Medidas

As medidas DAX que criaremos na Seção 4 precisam de um lugar para ficar armazenadas. Em vez de espalhá-las entre as tabelas de dados, a boa prática é concentrá-las em uma tabela vazia dedicada — isso facilita a organização e a manutenção.

- Ainda na **Exibição de Tabela**, sem nenhuma tabela selecionada, clique em **Nova Tabela**.
- Na barra de fórmulas, digite exatamente:

```dax
Medidas =
```

- e pressione Enter. A tabela `Medidas` será criada e aparecerá vazia no painel de dados. Todas as métricas do dashboard serão adicionadas a ela na Seção 4.

### 3.3 Tabela Auxiliar `tb_cascata`

O gráfico de cascata (Composição do PIB) precisa que os componentes apareçam em uma ordem específica e com rótulos curtos. O Power BI, por padrão, ordena os itens alfabeticamente — o que quebraria a lógica de leitura do gráfico.

A solução é criar uma pequena tabela manual com duas colunas: o nome curto de cada componente e um número que define a ordem de exibição. Essa tabela é usada pela medida DAX `valor_cascata` para mapear cada barra do gráfico ao valor correto.

- Na **Exibição de Tabela**, sem nenhuma tabela selecionada, acesse: **Página Inicial → Inserir Dados**.
- Crie a tabela com as duas colunas e os seis valores a seguir:

| componente | ordem |
| --- | --- |
| famílias | 1 |
| governo | 2 |
| fbcf | 3 |
| exportações | 4 |
| importações | 5 |
| Total | 6 |

- Nomeie a tabela como `tb_cascata` e clique em **Carregar**. Esta tabela não se relaciona com as demais — ela é referenciada diretamente pela fórmula DAX da medida `valor_cascata`.

### 3.4 Relacionamento entre as tabelas

Com as tabelas criadas, precisamos conectá-las para que os filtros do dashboard se propaguem corretamente. O relacionamento é feito na **Exibição de Modelo** — acessível pelo ícone de diagrama no painel esquerdo.

- Clique no ícone de **Exibição de Modelo** no painel esquerdo.
- Localize as tabelas `d_Calendario` e `PIB trimestral` no diagrama.
- Clique e arraste o campo `Date` da tabela `d_Calendario` até o campo `Data Trimestral` da tabela `PIB trimestral`.
- O Power BI criará automaticamente um relacionamento de um-para-muitos (1:*), indicando que cada data do calendário pode corresponder a múltiplas linhas na tabela de dados. Confirme que a direção do relacionamento está de `d_Calendario` para `PIB trimestral`.
- Salve o arquivo com **Ctrl+S**.

> **Nota:** A tabela `PIB valores`, que contém os dados brutos antes do tratamento no Power Query, também aparece no modelo. Se ela não for utilizada em nenhum visual, pode ser ocultada, mas o relacionamento com o calendário deve ser mantido para que as medidas DAX funcionem corretamente.

---

## 4. Criação das Medidas DAX

As medidas DAX são fórmulas dinâmicas que calculam os indicadores do dashboard em resposta aos filtros aplicados pelo usuário. Elas são diferentes de colunas calculadas: enquanto colunas calculam um valor fixo para cada linha da tabela, medidas recalculam o resultado toda vez que o contexto muda — por exemplo, quando o usuário seleciona um ano diferente no filtro.

Todas as medidas são criadas na tabela `Medidas`. Para criar uma nova medida: clique com o botão direito sobre a tabela `Medidas` no painel de dados → **Nova Medida**. A barra de fórmulas se abrirá para você digitar o código DAX.

As medidas estão organizadas em grupos por função. Crie-as na ordem apresentada, pois as medidas de um grupo frequentemente dependem das medidas do grupo anterior.

### 4.1 Medidas base de soma

Essas são as medidas fundamentais. Todo o restante do cálculo parte delas.

#### SOMA total

Soma todos os valores da tabela no contexto de filtro atual. É a medida mais genérica — retorna o total de qualquer combinação de setores e períodos que estiver selecionada:

```dax
SOMA total = SUM('PIB trimestral'[Valor])
```

#### PIB total

Isola o PIB a preços de mercado (código 90707), que é o agregado geral — a soma de todos os componentes. Esta medida ignora qualquer filtro de setor e sempre retorna o PIB completo:

```dax
PIB total =
CALCULATE(
    [SOMA total],
    'PIB trimestral'[Setores e subsetores (Código)] = "90707"
)
```

### 4.2 Medidas para os cartões do dashboard

Os quatro cartões no topo do dashboard exibem sempre o valor mais recente disponível nos dados, independentemente de qualquer filtro de ano aplicado pelo usuário. Para isso, usamos medidas que buscam internamente o último período disponível.

#### PIB Atual

Retorna o valor do PIB do trimestre mais recente disponível na tabela de dados. Como usa `LASTDATE` internamente, esse valor não muda quando o usuário filtra por ano — o cartão sempre mostra o dado mais atualizado:

```dax
PIB Atual =
CALCULATE(
    [PIB total],
    LASTDATE('d_Calendario'[Date])
)
```

#### pib_br

Versão auxiliar do PIB total sem restrição de data. Usada como denominador nas fórmulas de participação percentual:

```dax
pib_br =
CALCULATE(
    [SOMA total],
    'PIB trimestral'[Setores e subsetores (Código)] = "90707"
)
```

### 4.3 Medidas de acumulado e período anterior

Para calcular variações percentuais, precisamos ter o valor do período de referência — seja o trimestre anterior, o mesmo trimestre do ano passado ou os últimos quatro trimestres acumulados.

#### PIB anterior

PIB do trimestre imediatamente anterior ao período filtrado. Usada como base de comparação para a variação trimestral:

```dax
PIB anterior =
CALCULATE(
    [PIB total],
    DATEADD('d_Calendario'[Date], -1, QUARTER)
)
```

#### PIB ano anterior

PIB do mesmo trimestre do ano anterior. Usada para calcular a variação interanual:

```dax
PIB ano anterior =
CALCULATE(
    [PIB total],
    SAMEPERIODLASTYEAR('d_Calendario'[Date])
)
```

#### PIB 4T

Soma dos últimos quatro trimestres acumulados a partir do período atual. Equivale ao PIB anualizado:

```dax
PIB 4T =
CALCULATE(
    [PIB total],
    DATESINPERIOD('d_Calendario'[Date], LASTDATE('d_Calendario'[Date]), -4, QUARTER)
)
```

#### PIB 4T anterior

Soma dos quatro trimestres imediatamente anteriores ao intervalo de PIB 4T. Permite comparar dois períodos anualizados consecutivos:

```dax
PIB 4T anterior =
CALCULATE(
    [PIB total],
    DATESINPERIOD('d_Calendario'[Date], LASTDATE('d_Calendario'[Date]), -8, QUARTER),
    NOT DATESINPERIOD('d_Calendario'[Date], LASTDATE('d_Calendario'[Date]), -4, QUARTER)
)
```

### 4.4 Taxas de variação

Com as medidas de período em mãos, calculamos as variações percentuais. Crie as medidas a seguir e, em seguida, formate todas como **Percentual** com uma casa decimal: selecione a medida → aba **Ferramentas de Medida** → **Formato** → **Percentual**.

Existem duas versões para cada variação: uma para uso nos gráficos (responde ao filtro de ano) e uma para uso nos cartões (sempre fixa no último trimestre disponível). Isso é necessário porque a medida `PIB Atual` do cartão não responde a filtros — e as medidas de variação do cartão precisam seguir a mesma lógica.

#### VAR % trim anterior — para gráficos

Variação percentual do PIB em relação ao trimestre imediatamente anterior. Usada nas colunas do gráfico de evolução trimestral:

```dax
VAR % trim anterior =
DIVIDE(
    [PIB total] - [PIB anterior],
    [PIB anterior]
)
```

#### VAR Trim Atual 2 — para o cartão

Mesma variação trimestral, mas calculada sempre com base no último trimestre disponível. É essa medida que vai no cartão "Var Trimestre Anterior":

```dax
VAR Trim Atual 2 =
VAR UltimaData = LASTDATE('d_Calendario'[Date])
VAR PIBAtual   = CALCULATE([PIB total], UltimaData)
VAR PIBAnt     = CALCULATE([PIB total], DATEADD(UltimaData, -1, QUARTER))
RETURN
DIVIDE(PIBAtual - PIBAnt, PIBAnt)
```

#### VAR % ano anterior — para gráficos

Variação percentual em relação ao mesmo trimestre do ano anterior. Usada nos gráficos de evolução:

```dax
VAR % ano anterior =
DIVIDE(
    [PIB total] - [PIB ano anterior],
    [PIB ano anterior]
)
```

#### VAR Ano Atual card — para o cartão

Variação interanual fixada no último trimestre disponível. É essa medida que vai no cartão "Var Ano Anterior":

```dax
VAR Ano Atual card =
VAR UltimaData = LASTDATE('d_Calendario'[Date])
VAR PIBAtual   = CALCULATE([PIB total], UltimaData)
VAR PIBAntAno  = CALCULATE([PIB total], SAMEPERIODLASTYEAR(UltimaData))
RETURN
DIVIDE(PIBAtual - PIBAntAno, PIBAntAno)
```

#### var_valor_4t_total — linha do gráfico de evolução

Variação do acumulado de 4 trimestres em relação ao acumulado dos 4 trimestres anteriores. Exibida como a linha no gráfico de "Evolução e Variação Trimestral":

```dax
var_valor_4t_total =
DIVIDE(
    [PIB 4T] - [PIB 4T anterior],
    [PIB 4T anterior]
)
```

#### VAR Acum 4T Atual — para o cartão

Variação do acumulado 4T fixada no período mais recente. É essa medida que vai no cartão "Var Acumulada":

```dax
VAR Acum 4T Atual =
VAR UltimaData = LASTDATE('d_Calendario'[Date])
VAR PIB4T      = CALCULATE([PIB 4T], UltimaData)
VAR PIB4TAnt   = CALCULATE([PIB 4T anterior], UltimaData)
RETURN
DIVIDE(PIB4T - PIB4TAnt, PIB4TAnt)
```

### 4.5 Índices setoriais base 100

O gráfico de "Evolução dos Índices Setoriais" compara a trajetória de cada setor ao longo do tempo usando o primeiro trimestre de 1996 como base (valor 100). Isso permite comparar setores com volumes absolutos muito diferentes na mesma escala.

#### indice_100

Índice do PIB total. O valor 100 representa o PIB do primeiro trimestre de 1996 — valores acima de 100 indicam crescimento acumulado desde então:

```dax
indice_100 =
VAR base =
    CALCULATE(
        [PIB total],
        ALL('d_Calendario'),
        'PIB trimestral'[Trimestre (Código)] = 199601
    )
RETURN
DIVIDE([PIB total], base) * 100
```

#### Agro, Indústria e Serviços

Mesma lógica do `indice_100`, mas aplicada a cada setor individualmente. Altere o código do setor em cada uma:

```dax
Agro =
VAR VA_agro =
    CALCULATE([SOMA total], 'PIB trimestral'[Setores e subsetores (Código)] = "90687")
VAR base =
    CALCULATE([SOMA total], ALL('d_Calendario'),
        'PIB trimestral'[Setores e subsetores (Código)] = "90687",
        'PIB trimestral'[Trimestre (Código)] = 199601)
RETURN DIVIDE(VA_agro, base) * 100

-- Indústria: substitua "90687" por "90691" e nomeie como Indústria
-- Serviços:  substitua "90687" por "90696" e nomeie como Servicos
```

### 4.6 Participação percentual no PIB

Essas medidas calculam o peso de cada componente no PIB total. São usadas no gráfico de cascata e permitem responder: qual fatia do PIB veio do consumo das famílias? E do investimento?

#### participacao — medida genérica

Divide o valor de qualquer setor filtrado pelo PIB total. Útil quando o contexto de filtro já está definido pelo visual:

```dax
participacao = DIVIDE([SOMA total], [pib_br])
```

#### consumo_familias e demais componentes

Para os componentes específicos da demanda, a medida isola o setor pelo código e divide pelo PIB total. Crie uma medida para cada componente, alterando apenas o código numérico:

```dax
consumo_familias =
VAR v = CALCULATE([SOMA total],
    'PIB trimestral'[Setores e subsetores (Código)] = "93404")
RETURN DIVIDE(v, [pib_br])

-- consumo_governo:  código "93405"
-- exportação:       código "93407"
-- importação:       código "93408"
-- Investimento (Formação bruta de capital fixo): código "93406"
```

### 4.7 Medida para o gráfico de cascata

O gráfico de cascata exibe seis barras correspondentes aos cinco componentes da demanda mais o total. A medida `valor_cascata` usa a tabela `tb_cascata` para saber qual barra está sendo renderizada e retorna o valor de participação correto para ela:

```dax
valor_cascata =
VAR comp = SELECTEDVALUE(tb_cascata[componente])
RETURN
SWITCH(comp,
    "famílias",    CALCULATE([SOMA total],
                     'PIB trimestral'[Setores e subsetores (Código)] = "93404") / [pib_br],
    "governo",     CALCULATE([SOMA total],
                     'PIB trimestral'[Setores e subsetores (Código)] = "93405") / [pib_br],
    "fbcf",        CALCULATE([SOMA total],
                     'PIB trimestral'[Setores e subsetores (Código)] = "93406") / [pib_br],
    "exportações", CALCULATE([SOMA total],
                     'PIB trimestral'[Setores e subsetores (Código)] = "93407") / [pib_br],
    "importações", CALCULATE([SOMA total],
                     'PIB trimestral'[Setores e subsetores (Código)] = "93408") / [pib_br] * -1,
    "Total",       1,
    BLANK()
)
```

O fator `* -1` na linha de importações é necessário porque as importações reduzem o PIB pela ótica da demanda — o gráfico de cascata precisa do valor negativo para exibir a barra apontando para baixo.

### 4.8 Medida de variação para as matrizes

As duas tabelas de calor (matrizes) usam uma medida de variação trimestral genérica que se adapta a qualquer setor filtrado pelo visual:

#### variacao

```dax
variacao =
VAR atual    = [SOMA total]
VAR anterior = CALCULATE([SOMA total],
                 DATEADD('d_Calendario'[Date], -1, QUARTER))
RETURN
DIVIDE(atual - anterior, anterior)
```

#### Indice PIB

Índice de evolução do PIB total com base 100 no primeiro trimestre de 1996. Usado como linha de referência no gráfico de índices setoriais:

```dax
Indice PIB =
VAR base =
    CALCULATE([PIB total], ALL('d_Calendario'),
        'PIB trimestral'[Trimestre (Código)] = 199601)
RETURN
DIVIDE([PIB total], base) * 100
```

> **Dica:** Organize as medidas em pastas dentro da tabela `Medidas` para facilitar a navegação: selecione a medida → aba **Ferramentas de Medida** → **Pasta de Exibição**. Crie as pastas "PIBs totais" (para `PIB anterior`, `PIB ano anterior`, `PIB total` e `SOMA total`) e "Taxas de variação" (para `VAR % ano anterior` e `VAR % trim anterior`). As medidas dos cartões ficam na raiz da tabela.

---

## 5. Construção dos Visuais do Dashboard

Com o modelo de dados e as medidas prontos, vamos montar os onze elementos visuais da página "Dashboard Final". A ordem sugerida segue uma lógica de cima para baixo e da esquerda para a direita — o mesmo caminho que o olho percorre ao ler o dashboard.

### 5.1 Configurar a página

- Clique com o botão direito em uma área vazia da tela → **Configurações de Página**.
- Em **Tipo**, selecione **Personalizado** e defina: Largura: `1280` e Altura: `720`. Esse formato corresponde à proporção 16:9 e é ideal para apresentações e telas de computador.
- Em **Plano de Fundo**, configure a cor como cinza muito claro (`#F3F4F6`). Os painéis de cada visual terão cores individuais — o fundo da página apenas preenche os espaços entre eles.

### 5.2 Cabeçalho

O cabeçalho ocupa toda a faixa superior da página e serve como identidade visual do dashboard.

- Acesse: **Inserir → Caixa de Texto**.
- Na primeira linha, digite `PIB TRIMESTRAL DO BRASIL`. Selecione o texto e aplique: fonte Arial, tamanho 24, negrito, cor branco.
- Na segunda linha, digite `Análise de crescimento e composição`. Aplique: fonte Arial, tamanho 14, cor azul claro.
- No painel **Formatar seu visual → Geral → Propriedades**, defina o plano de fundo com a cor `#0D1B3E` (azul escuro). Isso cria o contraste entre o cabeçalho e o resto da página.

### 5.3 Cartões de indicadores

Os quatro cartões ficam logo abaixo do cabeçalho e exibem sempre os indicadores do trimestre mais recente disponível, independentemente do filtro de ano selecionado.

#### Cartão: PIB Atual

- **Inserir → Cartão**.
- No campo **Campos**, arraste a medida `PIB Atual`.
- Em **Formatar seu visual → Rótulo de Chamada**, defina a unidade de exibição como **Milhões**.
- Em **Rótulo de Categoria**, digite: `PIB Atual`.
- Ative a borda do visual com cor discreta.

#### Cartão: Var Trimestre Anterior

- **Inserir → Cartão**.
- No campo **Campos**, arraste a medida `VAR Trim Atual 2`.
- Em **Rótulo de Categoria**, digite: `Var Trimestre Anterior`.
- Formate o valor como **Percentual** com uma casa decimal.

#### Cartão: Var Ano Anterior

- Repita o processo do cartão anterior, usando a medida `VAR Ano Atual card`.
- Em **Rótulo de Categoria**, digite: `Var Ano Anterior`.

#### Cartão: Var Acumulada

- Repita o processo, usando a medida `VAR Acum 4T Atual`.
- Em **Rótulo de Categoria**, digite: `Var Acumulada`.

### 5.4 Gráfico de cascata — Composição do PIB (%)

Este gráfico exibe a participação de cada componente da demanda no PIB do período selecionado. As barras positivas (consumo, investimento, exportações) somam o PIB, enquanto a barra das importações subtrai — o resultado final é o Total, que equivale a 100%.

- **Inserir → Gráfico de Cascata**.
- No campo **Categoria**, arraste o campo `componente` da tabela `tb_cascata`.
- No campo **Valores Y**, arraste a medida `valor_cascata`.
- Em **Formatar → Cores dos Dados**, configure: Aumentar → Verde, Diminuir → Vermelho, Total → Azul.
- Defina o título como: `Composição do PIB (%)`.
- Formate o eixo Y como **Percentual**.

### 5.5 Gráfico combinado — Evolução e Variação Trimestral do PIB

Este gráfico sobrepõe barras e uma linha: as barras mostram a variação em relação ao trimestre anterior e a linha mostra a variação do acumulado de quatro trimestres. Essa combinação revela tanto a oscilação de curto prazo quanto a tendência de médio prazo.

- **Inserir → Gráfico de Linhas e Colunas Agrupadas**.
- No eixo X (Compartilhado), arraste o campo `trim/ano` da tabela `d_Calendario`.
- Em **Valores da Coluna**, arraste a medida `VAR % trim anterior`.
- Em **Valores da Linha**, arraste a medida `var_valor_4t_total`.
- Formate ambos os eixos Y como **Percentual**.
- Defina o título como: `Evolução e Variação Trimestral do PIB`.

### 5.6 Gráfico de linhas — Evolução dos Índices Setoriais por Ano

Este gráfico mostra como cada setor da economia cresceu desde 1996, usando o índice base 100. Como os valores absolutos de cada setor são muito diferentes entre si, o índice permite a comparação direta das trajetórias de crescimento.

- **Inserir → Gráfico de Linhas**.
- No eixo X, arraste o campo `Date` da tabela `d_Calendario` e selecione o nível **Ano** na hierarquia de datas.
- Em **Valores Y**, adicione as quatro medidas: `indice_100`, `Agro`, `Indústria` e `Servicos`.
- Configure as cores das séries: `indice_100` → azul, `Agro` → verde, `Indústria` → laranja, `Servicos` → roxo.
- Para renomear as séries na legenda, clique duas vezes sobre o nome de cada medida no campo **Valores** e edite o rótulo.
- Defina o título como: `Evolução dos Índices Setoriais por Ano`.

### 5.7 Matriz de variação — Ótica da Oferta

Esta tabela de calor exibe a variação trimestral dos três grandes setores produtivos (Agropecuária, Indústria e Serviços). Células verdes indicam crescimento, células vermelhas indicam retração.

- **Inserir → Matriz**.
- Em **Linhas**, arraste o campo `Date` da tabela `d_Calendario` e selecione os níveis **Ano** e **Trimestre** da hierarquia.
- Em **Colunas**, arraste o campo `Componente Curto` da tabela `PIB trimestral`.
- Em **Valores**, arraste a medida `variacao`.
- No painel **Filtros** deste visual, adicione o campo `Setores e subsetores` e selecione apenas: Agropecuária - total, Indústria - total e Serviços - total.
- Para aplicar as cores: selecione a matriz → **Formatar → Células → Cor de Fundo → Formatação Condicional → Escala de 3 Cores**. Configure: mínimo em vermelho (`#C0392B`), centro em branco (`#FFFFFF`) e máximo em verde (`#27AE60`).

### 5.8 Matriz de variação — Ótica da Demanda

Mesma estrutura da matriz anterior, mas exibindo os componentes de demanda: consumo das famílias, consumo do governo, investimento (formação bruta de capital fixo), exportações e importações.

- **Inserir → Matriz**.
- Em **Linhas**, repita a configuração da matriz anterior (Ano e Trimestre da tabela `d_Calendario`).
- Em **Colunas**, arraste o campo `Setores e subsetores` da tabela `PIB trimestral`.
- Em **Valores**, arraste a medida `variacao`.
- No painel **Filtros** deste visual, selecione apenas: Despesa de consumo da administração pública, Despesa de consumo das famílias, Exportação de bens e serviços, Formação bruta de capital fixo e Importação de bens e serviços (-).
- Aplique a mesma formatação condicional de cores descrita na etapa 5.7.

### 5.9 Segmentação de dados — Filtro por Ano

A segmentação de dados é o elemento de controle principal do dashboard. O usuário clica em um ou mais anos para filtrar os visuais que respondem a esse filtro. Os visuais que não respondem — como o gráfico histórico e o cartão PIB Atual — permanecem fixos.

- **Inserir → Segmentação de Dados**.
- No campo, arraste o campo `Date` da tabela `d_Calendario` e selecione o nível **Ano**.
- Em **Formatar seu visual → Configurações de Segmentação → Estilo**, selecione **Bloco**. Isso cria os botões de ano que aparecem no dashboard.
- Posicione o elemento no canto inferior direito da página.

---

## 6. Configuração das Interações entre Visuais

Por padrão, qualquer clique em um visual do Power BI filtra todos os outros na mesma página. Esse comportamento é desejável para a maioria dos visuais — mas não para todos. Nesta etapa configuramos quais visuais respondem ao filtro de ano e quais permanecem estáticos.

A lógica é a seguinte: o gráfico de evolução histórica dos índices setoriais precisa mostrar sempre a série completa desde 1996, independentemente do ano selecionado, pois sua função é mostrar a trajetória de longo prazo. O cartão PIB Atual também deve permanecer fixo, porque a medida que o alimenta já busca internamente sempre o dado mais recente.

### 6.1 Comportamento de cada visual

| Visual | Tipo | Reage ao filtro de ano? | Motivo |
| --- | --- | --- | --- |
| PIB Atual | Cartão | Não — bloqueado | A medida usa LASTDATE internamente |
| Var Trimestre Anterior | Cartão | Sim | Exibe a variação do ano filtrado |
| Var Ano Anterior | Cartão | Sim | Exibe a variação do ano filtrado |
| Var Acumulada | Cartão | Sim | Exibe a variação do ano filtrado |
| Composição do PIB (%) | Cascata | Sim | Mostra a composição do período selecionado |
| Evolução e Variação Trimestral | Gráfico combinado | Sim | Exibe os trimestres do período selecionado |
| Índices Setoriais por Ano | Gráfico de linhas | Não — bloqueado | Deve manter a série histórica completa |
| Matriz Oferta | Matriz | Sim | Filtra as linhas pelo ano selecionado |
| Matriz Demanda | Matriz | Sim | Filtra as linhas pelo ano selecionado |
| Cabeçalho | Caixa de texto | Não — bloqueado | Texto fixo, não contém dados |

### 6.2 Como configurar as interações

- Clique na Segmentação de Dados (filtro de ano) para selecioná-la.
- No menu superior, acesse: **Formato → Editar Interações**. Um conjunto de ícones de controle aparecerá acima de cada visual da página.
- Clique no ícone de bloquear (círculo com barra) sobre os três visuais que devem permanecer estáticos: cartão PIB Atual, gráfico de linhas "Evolução dos Índices Setoriais por Ano" e a caixa de texto do cabeçalho.
- Nos demais visuais, o ícone de filtro deve permanecer ativo (é o padrão).
- Quando terminar, clique novamente em **Editar Interações** para sair do modo de edição.

> **Dica:** Após configurar as interações, faça o teste: clique em anos diferentes no slicer e observe se o gráfico histórico permanece estático enquanto as matrizes e os cartões de variação se atualizam. Se algum visual se comportar de forma inesperada, repita o processo de Editar Interações para aquele elemento específico.

---

## 7. Formatação e Publicação

Com todos os visuais montados e as interações configuradas, esta última etapa cuida da apresentação e da disponibilização do dashboard.

### 7.1 Bordas e alinhamento

- Para adicionar borda a um visual: selecione-o → **Formatar seu visual → Geral → Efeitos → Borda** → ative com cor `#1F4E79` e espessura de 1 pixel.
- Para alinhar múltiplos visuais ao mesmo tempo: segure **Ctrl**, clique em cada visual desejado e acesse o menu **Formatar → Alinhar** para distribuí-los horizontalmente ou verticalmente de forma uniforme.

### 7.2 Formatação condicional das matrizes

Caso as cores das matrizes não tenham sido aplicadas na Seção 5, siga este caminho para cada uma delas:

- Selecione a matriz → **Formatar seu visual → Valores de célula → Cor de Fundo** → ative a **Formatação Condicional**.
- Configure a escala de três cores: mínimo vermelho (`#C0392B`), centro branco (`#FFFFFF`), máximo verde (`#27AE60`).
- O Power BI calculará automaticamente o gradiente com base nos valores mínimo e máximo do contexto filtrado.

### 7.3 Organização das medidas em pastas

Conforme o número de medidas cresce, o painel de dados pode ficar desorganizado. O Power BI permite agrupar medidas em pastas temáticas dentro de cada tabela:

- Selecione a medida no painel de dados → aba **Ferramentas de Medida** → campo **Pasta de Exibição**.
- Crie a pasta `PIBs totais` e mova para ela: `PIB anterior`, `PIB ano anterior`, `PIB total` e `SOMA total`.
- Crie a pasta `Taxas de variação` e mova para ela: `VAR % ano anterior` e `VAR % trim anterior`.
- Deixe as medidas dos cartões (`PIB Atual`, `VAR Trim Atual 2`, `VAR Ano Atual card` e `VAR Acum 4T Atual`) na raiz da tabela `Medidas`, pois são as mais acessadas.

### 7.4 Salvar e publicar

- Salve o arquivo com **Ctrl+S**. Use o nome: `Trabalho_Dashboard_Power_Bi.pbix`.
- Para publicar no Power BI Service: **Página Inicial → Publicar** → selecione o workspace de destino. O Service é a plataforma online do Power BI, onde o dashboard pode ser acessado pelo navegador e compartilhado com outras pessoas.
- No Power BI Service, acesse o conjunto de dados publicado → **Atualização Agendada** → configure a frequência de atualização.

> **Dica:** O IBGE divulga os dados do PIB trimestral quatro vezes por ano, geralmente em março, junho, setembro e dezembro. Configure a atualização automática para ocorrer mensalmente — assim o dashboard estará sempre pronto quando os novos dados forem publicados, sem nenhuma intervenção manual.

---

**Roteiro concluído**

Fonte: IBGE / SIDRA — Tabela 1621 — Contas Nacionais Trimestrais (Referência 2010)
