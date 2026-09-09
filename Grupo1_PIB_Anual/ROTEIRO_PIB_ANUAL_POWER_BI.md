# Análise do PIB com Power BI e SIDRA/IBGE

**Grupo 1: Elisa Monteiro De Souza Furtado, Fabricio De Castro e Marina Patelli Miotto**

---

## Objetivo do Roteiro

Este roteiro tem como objetivo orientar a análise do PIB anual pelas óticas da **produção**, da **despesa** e da **renda**, utilizando painéis de *dashboard* desenvolvidos na ferramenta Power BI. A base de dados será extraída do **Sistema IBGE de Recuperação Automática (SIDRA)**, disponível em: [https://sidra.ibge.gov.br/](https://sidra.ibge.gov.br/)

Para o tratamento e a organização dos dados, será adotada a metodologia **ETL** (*Extract, Transform, Load*), amplamente utilizada em processos de análise e integração de dados:

- **Extract (Extração):** coleta de dados brutos provenientes de fontes diversas, como bancos de dados, planilhas, APIs ou sistemas institucionais.
- **Transform (Transformação):** processamento e tratamento dos dados, envolvendo limpeza, padronização, organização e conversão das informações de acordo com regras específicas, tornando-os adequados para análise.
- **Load (Carregamento):** armazenamento dos dados já tratados em um destino final — como um Data Warehouse ou Data Lake —, possibilitando sua utilização em análises, relatórios e dashboards.

---

## Sumário

1. [Extração dos Dados no SIDRA](#1-extração-dos-dados-no-sidra)
   - 1.1 [Acessando o Portal do SIDRA](#11-acessando-o-portal-do-sidra)
   - 1.2 [Selecionando a Tabela](#12-selecionando-a-tabela)
   - 1.3 [Extraindo os Dados via API](#13-extraindo-os-dados-via-api)
2. [Importando os Dados no Power BI](#2-importando-os-dados-no-power-bi)
3. [Tratamento dos Dados no Power Query](#3-tratamento-dos-dados-no-power-query)
   - 3.1 [Renomeando a Consulta](#31-renomeando-a-consulta)
   - 3.2 [Promovendo o Cabeçalho](#32-promovendo-o-cabeçalho)
   - 3.3 [Removendo Colunas Desnecessárias](#33-removendo-colunas-desnecessárias)
   - 3.4 [Ajustando os Tipos de Dados](#34-ajustando-os-tipos-de-dados)
   - 3.5 [Transformando Trimestre em Data](#35-transformando-trimestre-em-data)
   - 3.6 [Aplicando as Alterações](#36-aplicando-as-alterações)
4. [Criando a Tabela de Datas (Calendário)](#4-criando-a-tabela-de-datas-calendário)
5. [Modelagem de Dados — Relacionamentos entre Tabelas](#5-modelagem-de-dados--relacionamentos-entre-tabelas)
6. [Criando as Tabelas de Medidas e Títulos](#6-criando-as-tabelas-de-medidas-e-títulos)
7. [Criando as Medidas em DAX](#7-criando-as-medidas-em-dax)
   - 7.1 [Medida Base: Soma dos Valores](#71-medida-base-soma-dos-valores)
   - 7.2 [Valor do PIB a Preços de Mercado](#72-valor-do-pib-a-preços-de-mercado)
   - 7.3 [Taxa de Crescimento do PIB](#73-taxa-de-crescimento-do-pib)
   - 7.4 [Valores Agregados dos Setores (Ótica da Oferta)](#74-valores-agregados-dos-setores-ótica-da-oferta)
   - 7.5 [Números-Índice dos Setores](#75-números-índice-dos-setores)
   - 7.6 [Taxa de Crescimento dos Subsetores](#76-taxa-de-crescimento-dos-subsetores)
   - 7.7 [Participação Relativa dos Setores no PIB (Shares)](#77-participação-relativa-dos-setores-no-pib-shares)
8. [Construindo os Dashboards](#8-construindo-os-dashboards)
   - 8.1 [Título do Relatório](#81-título-do-relatório)
   - 8.2 [Segmentação de Dados por Ano](#82-segmentação-de-dados-por-ano)
   - 8.3 [Cartão: Valor do PIB](#83-cartão-valor-do-pib)
   - 8.4 [Cartão: Taxa de Crescimento do PIB](#84-cartão-taxa-de-crescimento-do-pib)
   - 8.5 [Gráfico de Linhas: Evolução do PIB](#85-gráfico-de-linhas-evolução-do-pib)
   - 8.6 [Gráfico de Colunas: Variação por Índice Setorial](#86-gráfico-de-colunas-variação-por-índice-setorial)
   - 8.7 [Gráfico de Colunas: PIB pela Ótica da Oferta (Shares)](#87-gráfico-de-colunas-pib-pela-ótica-da-oferta-shares)
   - 8.8 [Gráfico de Barras: Crescimento por Subsetor Industrial](#88-gráfico-de-barras-crescimento-por-subsetor-industrial)
   - 8.9 [Gráfico de Barras: Crescimento por Subsetor de Serviços](#89-gráfico-de-barras-crescimento-por-subsetor-de-serviços)
   - 8.10 [Gráfico de Colunas: PIB pela Ótica da Demanda](#810-gráfico-de-colunas-pib-pela-ótica-da-demanda)
9. [Títulos Dinâmicos](#9-títulos-dinâmicos)
10. [Configurando as Interações entre Visualizações](#10-configurando-as-interações-entre-visualizações)

---

## 1. Extração dos Dados no SIDRA

### 1.1 Acessando o Portal do SIDRA

### Objetivo

Acessar o portal do Sistema IBGE de Recuperação Automática (SIDRA) para localizar a base de dados das Contas Nacionais Trimestrais, que será utilizada ao longo de todo o roteiro.

### Passo a Passo

1. Abra um navegador de internet (Chrome, Firefox, Edge ou similar).
2. Acesse o endereço: [https://sidra.ibge.gov.br/](https://sidra.ibge.gov.br/)
3. Na página inicial do SIDRA, você encontrará diversas opções de pesquisas e bases de dados, organizadas em abas ou categorias temáticas — como IPCA, INPC, IPP, entre outras.
4. Localize e clique na opção **"Contas Nacionais Trimestrais (CNT) – Referência 2010"**.

### Resultado Esperado

Ao selecionar a base CNT, a página apresentará o título da pesquisa e, logo abaixo, a **data da última atualização disponível**. Esse dado é importante para registrar a versão dos dados utilizada no trabalho.

> **Dica**
>
> Antes de prosseguir, anote a data de atualização exibida pelo SIDRA. Em trabalhos acadêmicos, é uma boa prática registrar a data de acesso e a versão dos dados utilizados.

> **Observação**
>
> Ao lado do título da pesquisa CNT, você encontrará três ícones:
> - **Relação de tabelas da pesquisa:** abre a lista de todas as tabelas disponíveis nessa base de dados (é esse o ícone que usaremos).
> - **Calendário de divulgação:** apresenta as datas previstas para publicação de novos resultados ao longo do ano vigente.
> - **Ir para a página da pesquisa:** redireciona para a página principal da pesquisa no site do IBGE.

---

### 1.2 Selecionando a Tabela

### Objetivo

Localizar e acessar a **Tabela 6612**, que contém os valores encadeados do PIB a preços de 1995, base de referência para este roteiro.

### Motivo

O SIDRA organiza os dados em tabelas temáticas numeradas. Para este roteiro, utilizaremos especificamente a **Tabela 6612 – Valores encadeados a preços de 1995**, que apresenta a série histórica do PIB e de seus componentes de forma comparável ao longo do tempo.

### Passo a Passo

1. Na página das Contas Nacionais Trimestrais (CNT), clique no ícone **"Relação de tabelas da pesquisa"**, localizado ao lado do título da base de dados.
2. Uma lista de tabelas será exibida. Localize a **Tabela 6612 – Valores encadeados a preços de 1995** e clique sobre ela para acessá-la.

### Resultado Esperado

A página da Tabela 6612 será carregada, exibindo um formulário de seleção de variáveis, períodos e desagregações disponíveis para consulta.

---

### 1.3 Extraindo os Dados via API

### Objetivo

Selecionar os dados desejados e obter o link da API do SIDRA, que será utilizado para importar os dados diretamente no Power BI.

### Motivo

Em vez de baixar manualmente um arquivo (como CSV ou Excel), utilizaremos a API do SIDRA. Isso permite que o Power BI acesse os dados diretamente do servidor do IBGE, tornando o processo mais automatizado e reproduzível.

### Passo a Passo

1. Na página da Tabela 6612, selecione **todos** os setores e subsetores disponíveis. Para isso, marque a opção de seleção geral ou selecione individualmente todos os itens listados na seção de "Setores e subsetores".
2. Selecione **todos** os trimestres disponíveis na seção de períodos.
3. Após realizar as seleções, localize, no **canto inferior direito** da página, um conjunto de três botões.
4. Clique no botão **central**, denominado **"Links de compartilhar"**.
5. Uma janela se abrirá com opções de compartilhamento. Copie o conteúdo da primeira opção, chamada **"Parâmetros para a API"**.

### Resultado Esperado

Você terá copiado para a área de transferência do seu computador um link de URL longo, que representa os parâmetros de consulta à API do SIDRA. Esse link será colado diretamente no Power BI na próxima etapa.

> **Importante**
>
> Certifique-se de que todos os setores e subsetores estão selecionados antes de copiar o link. Caso algum item seja omitido, os dados importados no Power BI estarão incompletos.

---

## 2. Importando os Dados no Power BI

### Objetivo

Abrir o Power BI e conectá-lo à API do SIDRA para importar os dados da Tabela 6612 diretamente da fonte.

### Motivo

Esta etapa corresponde à fase de **Extract (Extração)** da metodologia ETL. Ao utilizar a URL da API em vez de um arquivo estático, garantimos que os dados podem ser atualizados automaticamente sempre que o IBGE publicar novas informações.

### Passo a Passo

1. Abra o **Power BI Desktop** no seu computador. Caso ainda não o tenha instalado, ele pode ser baixado gratuitamente no site da Microsoft: [https://powerbi.microsoft.com/pt-br/desktop/](https://powerbi.microsoft.com/pt-br/desktop/)
2. Na tela inicial do Power BI, selecione a opção **"Relatório em branco"**.
3. Uma tela de edição de relatório será aberta. Na área central, você verá uma tela em branco com o texto **"Adicionar dados ao seu relatório"** e alguns botões de ação.
4. Abaixo dos botões principais dessa área central, localize e clique na opção **"Obter dados de outra fonte"**.
5. Uma janela pop-up chamada **"Obter dados"** será aberta. No lado esquerdo dessa janela, há uma lista de categorias. Clique na categoria **"Outro"**.
6. No painel do lado direito da mesma janela, será exibida uma lista de conectores disponíveis. Localize e selecione a opção **"Web"**.
7. Clique no botão **"Conectar"**, localizado no canto inferior direito da janela.
8. Uma nova janela pop-up chamada **"Da Web"** será aberta. Verifique se a opção **"Básico"** está selecionada (ela deve aparecer como uma bolinha marcada à esquerda da palavra "Básico").
9. No campo de texto em branco abaixo, cole o link da API do SIDRA que você copiou anteriormente.
10. Clique em **"OK"**.

### Resultado Esperado

O Power BI processará a URL e estabelecerá conexão com o servidor do IBGE. Em seguida, ele abrirá automaticamente o **Editor do Power Query**, que é o ambiente de transformação de dados. Isso indica que a extração foi realizada com sucesso.

### Possíveis Problemas

- **Mensagem de erro de conexão:** verifique se o link da API foi copiado corretamente e se você está conectado à internet.
- **Janela de autenticação:** caso o Power BI solicite credenciais de acesso, selecione a opção **"Anônimo"** (os dados do SIDRA são públicos e não requerem login).
- **Dados não carregados:** caso a janela do Power Query abra vazia ou exiba uma mensagem de erro, retorne ao SIDRA e copie novamente o link da API.

---

## 3. Tratamento dos Dados no Power Query

O **Power Query** é uma ferramenta integrada ao Power BI (e também ao Excel) voltada para a **conexão, limpeza, transformação e organização de dados** provenientes de diversas fontes. Ele é o principal ambiente para a fase de **Transform (Transformação)** do processo ETL, permitindo que os dados sejam preparados para análise sem a necessidade de programação avançada.

Ao abrir o Power Query, você verá uma interface dividida em três áreas principais:
- **Painel esquerdo ("Consultas"):** lista todas as tabelas/consultas carregadas.
- **Área central:** exibe uma pré-visualização dos dados da consulta selecionada, no formato de uma tabela com linhas e colunas.
- **Painel direito ("Configurações de Consulta"):** exibe o nome da consulta e o histórico de todas as etapas de transformação aplicadas até o momento.

---

### 3.1 Renomeando a Consulta

### Objetivo

Renomear a consulta recém-criada para "PIB", facilitando sua identificação ao longo do projeto.

### Motivo

Por padrão, o Power BI atribui nomes genéricos às consultas (geralmente derivados da URL). Renomear a consulta para um nome claro e intuitivo é uma boa prática que facilita a organização do projeto, especialmente quando há múltiplas tabelas.

### Passo a Passo

1. No **painel direito** do Power Query, localize a seção **"Configurações de Consulta"** (ou "Config. Consulta").
2. Dentro dessa seção, há um campo chamado **"Nome"**, que exibe o nome atual da consulta.
3. Clique sobre o conteúdo desse campo, apague o texto existente e digite: `PIB`
4. Pressione **Enter** para confirmar.

### Resultado Esperado

O nome da consulta será atualizado para "PIB" tanto no painel direito quanto no painel esquerdo (lista de consultas).

---

### 3.2 Promovendo o Cabeçalho

### Objetivo

Transformar a primeira linha de dados da tabela em cabeçalho, fazendo com que o Power BI reconheça os nomes das variáveis corretamente.

### Motivo

Bases de dados de fontes oficiais como o IBGE frequentemente retornam os dados com os nomes das variáveis (como "Trimestre", "Valor", "Setores") na primeira linha, e não como cabeçalho da tabela. Se essa correção não for feita, o Power BI utilizará nomes genéricos como "Coluna 1", "Coluna 2" para identificar as variáveis, o que impossibilita a realização de cálculos e a criação de visualizações corretas.

### Passo a Passo

1. Na parte superior do Power Query, localize a aba **"Página Inicial"** (a primeira aba do menu de navegação).
2. Dentro dessa aba, no grupo de opções central, clique no botão **"Usar a Primeira Linha como Cabeçalho"**.

> **Observação**
>
> Este botão pode aparecer como um ícone com uma tabela e uma seta apontando para cima, acompanhado do texto "Usar a Primeira Linha como Cabeçalho". Em algumas versões do Power BI, ele pode estar dentro do menu suspenso do botão "Transformar" ou no grupo "Transformar" da aba "Página Inicial".

### Resultado Esperado

Os nomes das colunas na área de pré-visualização da tabela deixarão de exibir "Coluna 1", "Coluna 2", etc., e passarão a exibir os nomes reais das variáveis, como "Trimestre", "Valor", "Setores e subsetores", entre outros.

### Como Verificar

Observe a linha de cabeçalho na parte superior da tabela (a linha cinza com os nomes das colunas). Se os nomes exibidos correspondem a variáveis reconhecíveis (como "Ano", "Trimestre", "Valor"), a etapa foi executada corretamente.

---

### 3.3 Removendo Colunas Desnecessárias

### Objetivo

Eliminar as colunas que não serão utilizadas na análise, mantendo apenas as variáveis relevantes para o projeto.

### Motivo

Bases de dados do IBGE frequentemente contêm colunas que repetem a mesma informação em todas as linhas — como "Nível Territorial" (que apenas repete "País") ou "Brasil (Código)" (que apenas repete o código do Brasil). Essas colunas são redundantes e aumentam desnecessariamente o tamanho do arquivo, tornando o processamento mais lento e o painel menos organizado.

### Passo a Passo

Para **cada coluna** que não será utilizada, siga estes passos:

1. Na pré-visualização da tabela, localize o **cabeçalho** (nome) da coluna que deseja remover.
2. Clique com o **botão direito do mouse** sobre o nome dessa coluna.
3. No menu de contexto que surgirá, selecione a opção **"Remover"**.
4. A coluna será eliminada da tabela.

Repita esse processo para todas as colunas desnecessárias.

### Quais Colunas Remover

Remova todas as colunas **exceto** as cinco listadas abaixo, que são as únicas necessárias para a análise:

| Coluna a Manter | Finalidade |
|---|---|
| `Valor` | Contém o valor numérico do PIB e de seus componentes. |
| `Trimestre (Código)` | Código numérico que identifica o trimestre (ex.: 201901 para o 1º trimestre de 2019). |
| `Trimestre` | Descrição textual do trimestre (ex.: "1º trimestre 2019"). |
| `Setores e subsetores (Código)` | Código numérico que identifica o setor econômico. |
| `Setores e subsetores` | Nome descritivo do setor econômico (ex.: "Agropecuária", "Indústria"). |

> **Dica**
>
> Você pode remover colunas como "Unidade de Medida" e "Variável" sem prejuízo à análise, desde que registre essas informações no título do dashboard ou em algum outro local visível (por exemplo: "Contas Nacionais — em Milhões de Reais, Preços de 1995").

### Resultado Esperado

Ao concluir esta etapa, a tabela deverá conter **apenas cinco colunas**: `Valor`, `Trimestre (Código)`, `Trimestre`, `Setores e subsetores (Código)` e `Setores e subsetores`.

---

### 3.4 Ajustando os Tipos de Dados

### Objetivo

Configurar o tipo de dado correto para cada coluna, garantindo que o Power BI interprete cada variável da forma adequada.

### Motivo

O Power Query atribui automaticamente um tipo de dado a cada coluna (texto, número inteiro, número decimal, data, etc.). No entanto, essa atribuição automática frequentemente resulta em erros — especialmente com dados de fontes externas. Coluna classificada com o tipo errado pode impedir a realização de cálculos, distorcer a ordem cronológica dos gráficos ou gerar erros em fórmulas DAX.

### Como Alterar o Tipo de uma Coluna

1. Na pré-visualização da tabela, observe que ao lado esquerdo do nome de cada coluna há um **ícone** que indica o tipo de dado atual:
   - `ABC` ou `123 ABC`: texto
   - `1.2`: número decimal
   - `123`: número inteiro
   - Ícone de calendário: data
2. Para alterar o tipo, clique sobre esse ícone ao lado do nome da coluna desejada.
3. Um menu suspenso será exibido com as opções de tipos disponíveis.
4. Selecione o tipo adequado conforme a tabela abaixo.

### Tipos de Dados Recomendados

| Coluna | Tipo de Dado Correto | Motivo |
|---|---|---|
| `Valor` | Número Decimal | Permite realizar cálculos matemáticos e criar medidas econômicas com precisão decimal. |
| `Trimestre (Código)` | Número Inteiro ou Texto | Serve como indexador de tempo. Pode ser mantido como texto se não for utilizado em cálculos. |
| `Trimestre` | Texto | Descrição textual do período; não é utilizada diretamente em cálculos. |
| `Setores e subsetores (Código)` | Texto ou Número Inteiro | Código de classificação oficial. Manter como texto preserva a integridade dos códigos (evita arredondamentos). |
| `Setores e subsetores` | Texto | Nome descritivo da atividade econômica. |

> **Importante**
>
> A configuração do tipo de dado da coluna `Valor` requer um passo adicional, descrito a seguir, devido à diferença entre os formatos numéricos brasileiro e americano.

### Ajustando a Localidade da Coluna "Valor"

A coluna `Valor` contém números que utilizam **ponto** como separador decimal (padrão americano). Por padrão, o Power BI configurado em português interpreta o **ponto** como separador de milhar — o que pode distorcer completamente os valores.

Para corrigir isso:

1. Clique no **ícone de tipo de dado** ao lado do nome da coluna `Valor` (o ícone que indica o tipo atual).
2. No menu suspenso, selecione a opção **"Usando a Localidade..."** (em vez de selecionar diretamente um tipo de dado).
3. Uma nova janela será aberta com dois campos:
   - **Tipo de Dados:** selecione `Número Decimal`.
   - **Localidade:** selecione `Inglês (Estados Unidos)`.
4. Clique em **"OK"** para confirmar.

> **Por que alterar a localidade?**
>
> O padrão americano utiliza **ponto** para decimais e **vírgula** para milhar (ex.: 1,500.50). O padrão brasileiro faz o inverso: **vírgula** para decimais e **ponto** para milhar (ex.: 1.500,50). Como os dados do IBGE são extraídos em formato americano, é necessário instruir o Power BI a interpretar os números nesse padrão. Sem essa configuração, os valores podem ser completamente distorcidos.

### Resultado Esperado

Após ajustar todos os tipos, as colunas deverão exibir os ícones corretos em seus cabeçalhos. A coluna `Valor`, em particular, deverá exibir valores numéricos sem erros.

### Como Verificar

Clique na coluna `Valor` e observe os valores exibidos na pré-visualização. Se os números fazem sentido em termos de magnitude (valores na casa dos milhares ou milhões, conforme esperado para o PIB), a configuração foi realizada corretamente. Se a coluna exibir erros (`Error`) ou valores discrepantes, revise o tipo e a localidade definidos.

---

### 3.5 Transformando Trimestre em Data

### Objetivo

Criar uma nova coluna do tipo **Data** a partir da coluna `Trimestre`, convertendo o formato textual (ex.: "1º trimestre 2019") em um valor de data reconhecido pelo Power BI (ex.: 01/01/2019).

### Motivo

O Power BI e a linguagem DAX (utilizada para criar medidas e cálculos) exigem que variáveis temporais estejam no formato **Data** para que funções de inteligência de tempo — como cálculos de variação anual, médias móveis e acumulados — possam ser utilizadas. Sem essa conversão, o sistema não consegue ordenar os períodos corretamente nem aplicar filtros temporais de forma eficiente.

### Passo a Passo

1. Na aba **"Adicionar Coluna"** (localizada no menu superior do Power Query, ao lado de "Página Inicial" e "Transformar"), clique em **"Coluna Personalizada"**.
2. Uma janela chamada **"Coluna Personalizada"** será aberta. No campo **"Nome da nova coluna"**, insira o nome que desejar para a nova coluna (sugestão: `Data`).
3. No campo **"Fórmula de coluna personalizada"**, apague o conteúdo existente e insira a seguinte fórmula:

```
let
    Trimestre = Number.From(Text.Start([Trimestre], 1)),
    Ano = Number.From(Text.End([Trimestre], 4)),
    Mes = (Trimestre - 1) * 3 + 1
in
    #date(Ano, Mes, 1)
```

4. Clique em **"OK"** para confirmar.

> **Como funciona esta fórmula?**
>
> A fórmula extrai duas informações do texto da coluna `Trimestre`:
> - `Text.Start([Trimestre], 1)` captura o **primeiro caractere** do texto, que corresponde ao número do trimestre (1, 2, 3 ou 4).
> - `Text.End([Trimestre], 4)` captura os **últimos quatro caracteres** do texto, que correspondem ao ano (ex.: "2019").
>
> Em seguida, converte o número do trimestre no mês correspondente ao seu início:
> - 1º trimestre → mês 1 (janeiro)
> - 2º trimestre → mês 4 (abril)
> - 3º trimestre → mês 7 (julho)
> - 4º trimestre → mês 10 (outubro)
>
> Por fim, a função `#date(Ano, Mes, 1)` constrói uma data usando o ano extraído, o mês calculado e o dia 1.

### Resultado Esperado

Uma nova coluna será criada ao final da tabela, exibindo datas no formato `DD/MM/AAAA` (ou `AAAA-MM-DD`, dependendo das configurações regionais do sistema). Para o 1º trimestre de 2019, por exemplo, o valor exibido deverá ser `01/01/2019`.

### Possíveis Problemas

- **Erro na fórmula:** verifique se o nome da coluna de origem está exatamente como `[Trimestre]` (com colchetes e com a grafia idêntica à do cabeçalho da tabela).
- **Valores incorretos:** caso os meses não correspondam ao esperado, confirme que a coluna `Trimestre` está no formato textual esperado (ex.: "1º trimestre 2019").

---

### 3.6 Aplicando as Alterações

### Objetivo

Concluir as transformações no Power Query e carregar os dados tratados de volta para o Power BI.

### Passo a Passo

1. Na aba **"Página Inicial"** do Power Query (menu superior), localize o botão **"Fechar e Aplicar"**.
2. Clique em **"Fechar e Aplicar"**.

### Resultado Esperado

O Power Query será fechado e você retornará à tela principal do Power BI. Os dados tratados estarão disponíveis no painel de campos (painel lateral direito), sob o nome da tabela **"PIB"**. Isso conclui a fase de **Transform (Transformação)** da metodologia ETL.

> **Atenção**
>
> Caso apareça uma mensagem de erro ao fechar o Power Query, revise as etapas anteriores, especialmente os tipos de dados e a fórmula da coluna de data. Erros nessas etapas impedem o carregamento dos dados.

---

## 4. Criando a Tabela de Datas (Calendário)

### Objetivo

Criar uma tabela de datas auxiliar chamada `d_calendário`, que será utilizada para habilitar as funções de inteligência de tempo do Power BI.

### Motivo

Para realizar análises temporais — como comparar o PIB de um trimestre com o mesmo trimestre do ano anterior, ou calcular taxas de variação anual — o Power BI exige que exista uma **tabela de datas dedicada**, conectada à tabela de dados principal. Essa tabela deve conter uma linha para cada dia do período analisado e colunas derivadas como ano, mês, trimestre e semestre.

A criação dessa tabela é feita diretamente no Power BI por meio da linguagem **DAX** (*Data Analysis Expressions*), sem necessidade de carregar um arquivo externo.

### Passo a Passo

1. Na tela principal do Power BI, localize o menu superior e clique na aba **"Modelagem"**.
2. Dentro da aba "Modelagem", clique no botão **"Nova Tabela"**.
3. Uma barra de fórmulas será exibida na parte superior da tela, aguardando a entrada de uma expressão DAX.
4. Apague o conteúdo padrão (geralmente `Tabela =`) e insira a seguinte fórmula:

```dax
d_calendário = ADDCOLUMNS(
    CALENDARAUTO(),
    "Ano", YEAR([Date]),
    "Nº Mês", MONTH([Date]),
    "Mês", FORMAT([Date], "MMM"),
    "Trimestre", CONCATENATE(QUARTER([Date]), "º Trim"),
    "Ano/Trim", YEAR([Date]) & "/" & CONCATENATE(QUARTER([Date]), "º Trim"),
    "Semestre", IF(MONTH([Date]) > 6, "2º Semestre", "1º Semestre"),
    "Ano/Semestre", YEAR([Date]) & "/" & IF(MONTH([Date]) > 6, "2º Semestre", "1º Semestre"),
    "Ano/Mês", YEAR([Date]) & "/" & FORMAT([Date], "MM")
)
```

5. Pressione **Enter** ou clique no ícone de confirmação (✓) na barra de fórmulas para criar a tabela.

> **Como funciona esta fórmula?**
>
> - `CALENDARAUTO()`: gera automaticamente uma sequência de datas abrangendo todo o período presente nos dados carregados no modelo. Cada linha representa um dia.
> - `ADDCOLUMNS(...)`: adiciona colunas derivadas à tabela de datas, como:
>   - `"Ano"`: extrai o ano de cada data.
>   - `"Nº Mês"`: extrai o número do mês (1 a 12).
>   - `"Mês"`: formata o mês como abreviação de três letras (jan, fev, mar...).
>   - `"Trimestre"`: identifica o trimestre no formato "1º Trim", "2º Trim", etc.
>   - `"Ano/Trim"`: combina ano e trimestre no formato "2019/1º Trim".
>   - `"Semestre"`: classifica o período como "1º Semestre" ou "2º Semestre".
>   - `"Ano/Semestre"`: combina ano e semestre no formato "2019/1º Semestre".
>   - `"Ano/Mês"`: combina ano e mês no formato "2019/01".

### Resultado Esperado

Uma nova tabela chamada `d_calendário` será exibida no painel de campos (painel lateral direito da tela principal do Power BI). Ela conterá uma coluna `Date` com uma data para cada dia do período dos dados, além das colunas derivadas criadas pela fórmula.

### Como Verificar

No painel de campos (lateral direita), clique na seta ao lado do nome `d_calendário` para expandir a tabela. Você deverá ver as colunas: `Date`, `Ano`, `Nº Mês`, `Mês`, `Trimestre`, `Ano/Trim`, `Semestre`, `Ano/Semestre` e `Ano/Mês`.

---

## 5. Modelagem de Dados — Relacionamentos entre Tabelas

### Objetivo

Conectar a tabela `d_calendário` à tabela `PIB` por meio de um relacionamento entre colunas de data, permitindo que o Power BI utilize a tabela de datas como eixo temporal para filtros e cálculos.

### Motivo

Sem o relacionamento entre as tabelas, o Power BI não consegue aplicar filtros temporais (como "mostrar apenas dados de 2019") de forma coordenada. O relacionamento informa ao sistema que a coluna `Date` da tabela `d_calendário` e a coluna `Data` (criada no Power Query) da tabela `PIB` representam a mesma dimensão temporal.

### Passo a Passo

1. No painel lateral esquerdo do Power BI, localize os ícones de navegação. Clique no ícone que representa a **"Exibição de Modelo"** (geralmente o terceiro ícone de cima para baixo, com o símbolo de um diagrama com retângulos conectados por linhas).
2. A tela de modelagem será aberta, exibindo as tabelas disponíveis como blocos com os nomes das colunas listados internamente.
3. Localize o bloco da tabela **`d_calendário`** e, dentro dele, a coluna **`Date`**.
4. Localize o bloco da tabela **`PIB`** e, dentro dele, a coluna de data criada no Power Query (denominada **`Data`** ou o nome que você atribuiu na etapa 3.5).
5. Clique e **arraste** a coluna `Date` da tabela `d_calendário` até a coluna `Data` da tabela `PIB` (ou vice-versa). Solte o botão do mouse sobre a coluna de destino.
6. Uma linha de relacionamento será desenhada entre as duas tabelas, indicando que a conexão foi estabelecida.

### Relacionamento a Criar

| Tabela de Origem | Coluna | Tabela de Destino | Coluna |
|---|---|---|---|
| `d_calendário` | `Date` | `PIB` | `Data` |

### Resultado Esperado

Na tela de modelagem, você verá uma linha conectando os dois blocos de tabela. Essa linha representa o relacionamento criado. O Power BI geralmente configura automaticamente o tipo de relacionamento como "muitos para um" (*many-to-one*), com a tabela `d_calendário` no lado "um" (cada data aparece uma vez) e a tabela `PIB` no lado "muitos" (a mesma data pode aparecer em múltiplas linhas, para diferentes setores).

### Como Verificar

Após criar o relacionamento, retorne à tela principal de relatório (primeiro ícone do painel lateral esquerdo). Crie um visual simples de teste: adicione uma segmentação de dados com a coluna `Ano` da tabela `d_calendário` e um cartão com a medida `Soma_valor` (que será criada na próxima etapa). Se ao selecionar um ano na segmentação o valor no cartão for atualizado, o relacionamento está funcionando corretamente.

---

## 6. Criando as Tabelas de Medidas e Títulos

### Objetivo

Criar duas tabelas auxiliares vazias — uma para organizar todas as medidas DAX criadas no projeto e outra para armazenar os títulos dinâmicos dos gráficos.

### Motivo

Por padrão, o Power BI armazena cada medida criada na tabela à qual ela foi associada. Quando o projeto contém muitas medidas distribuídas em diferentes tabelas, a localização e o gerenciamento dessas medidas tornam-se confusos. A criação de tabelas dedicadas para medidas e títulos é uma **boa prática de organização** que facilita a manutenção do projeto.

### Criando a Tabela de Medidas

1. Na tela principal do Power BI, clique na aba **"Modelagem"** no menu superior.
2. Clique no botão **"Nova Tabela"**.
3. Na barra de fórmulas, insira:
   ```
   Medidas = {BLANK()}
   ```
4. Pressione **Enter** para confirmar.
5. A tabela `Medidas` será criada com uma única linha em branco. Todas as medidas DAX que criarmos a partir de agora serão adicionadas a essa tabela.

> **Observação**
>
> A fórmula `{BLANK()}` cria uma tabela com uma única célula em branco. Isso é suficiente para que a tabela exista como contêiner de medidas. Ela não aparecerá nos gráficos e não afetará a análise.

### Criando a Tabela de Títulos

1. Ainda na aba **"Modelagem"**, clique novamente em **"Nova Tabela"**.
2. Na barra de fórmulas, insira:
   ```
   Títulos = {BLANK()}
   ```
3. Pressione **Enter** para confirmar.

Essa tabela será usada para armazenar as medidas DAX que geram os **títulos dinâmicos** dos gráficos (criados na Seção 9).

### Resultado Esperado

No painel de campos (lateral direita), aparecerão duas novas tabelas: **`Medidas`** e **`Títulos`**. Ambas estarão vazias por enquanto, mas servirão como repositório para as medidas criadas nas próximas etapas.

---

## 7. Criando as Medidas em DAX

**DAX** (*Data Analysis Expressions*) é a linguagem de fórmulas utilizada pelo Power BI para criar medidas, colunas calculadas e tabelas calculadas. As medidas em DAX são cálculos que são executados **dinamicamente** com base nos filtros e contextos ativos no relatório — ou seja, o resultado de uma medida muda automaticamente quando o usuário filtra o dashboard por ano, setor, etc.

### Como Criar uma Nova Medida

Para todas as medidas descritas nesta seção, siga este procedimento padrão:

1. No painel de campos (lateral direita), localize a tabela **`Medidas`**.
2. Clique com o **botão direito do mouse** sobre o nome da tabela `Medidas`.
3. No menu de contexto, selecione **"Nova Medida"**.
4. Uma barra de fórmulas será exibida na parte superior da tela, com o cursor posicionado para entrada de texto.
5. Apague o conteúdo padrão (geralmente `Medida =`) e insira a fórmula indicada para cada medida.
6. Pressione **Enter** ou clique no ícone de confirmação (✓) para salvar a medida.

---

### 7.1 Medida Base: Soma dos Valores

### Objetivo

Criar uma medida que soma todos os valores da coluna `Valor` da tabela `PIB`, sem nenhum filtro aplicado.

### Fórmula

```dax
Soma_valor = SUM(PIB[Valor])
```

### Explicação

- `SUM(PIB[Valor])`: soma todos os valores da coluna `Valor` da tabela `PIB`. No contexto de um gráfico ou tabela com filtros, a soma considera apenas os valores visíveis após a aplicação dos filtros.

Esta medida é a **base para todos os outros cálculos** do projeto. Ela representa a soma de todos os componentes do PIB presentes na base de dados para o contexto selecionado.

> **Atenção**
>
> O nome `PIB` entre colchetes refere-se à tabela importada do SIDRA (renomeada como "PIB" no Power Query). Certifique-se de que o nome digitado na fórmula corresponde exatamente ao nome da tabela no painel de campos.

---

### 7.2 Valor do PIB a Preços de Mercado

### Objetivo

Criar uma medida que filtra apenas o **PIB a preços de mercado** (excluindo os componentes setoriais), utilizando o código de variável correspondente.

### Fórmula

```dax
PIB_valor =
CALCULATE(
    [Soma_valor],
    PIB[Variável (Código)] = "90707"
)
```

### Explicação

- `CALCULATE(...)`: é uma das funções mais importantes do DAX. Ela avalia uma expressão (no caso, `[Soma_valor]`) dentro de um contexto de filtro modificado.
- `PIB[Variável (Código)] = "90707"`: aplica um filtro que seleciona apenas as linhas em que o código da variável é "90707", que corresponde ao PIB a preços de mercado na classificação do IBGE.

> **Nota Técnica**
>
> O código `90707` é o identificador oficial do IBGE para o "PIB a preços de mercado" na Tabela 6612 do SIDRA. Sem esse filtro, a medida somaria todos os componentes do PIB (agropecuária, indústria, serviços, impostos, etc.), resultando em um valor muito superior ao PIB total.

---

### 7.3 Taxa de Crescimento do PIB

### Objetivo

Calcular a taxa de variação anual do PIB em relação ao ano anterior.

### Motivo

A taxa de crescimento do PIB é um dos indicadores econômicos mais relevantes. Ela é calculada como `(PIB atual − PIB anterior) / PIB anterior`, representando a variação percentual em relação ao período anterior.

### Fórmula

```dax
Tx_crescimento% =
VAR PIB_anterior = CALCULATE(
    [PIB_valor],
    DATEADD('d_calendário'[Date], -1, YEAR)
)
RETURN
DIVIDE(
    ([PIB_valor] - PIB_anterior),
    PIB_anterior,
    " "
)
```

### Explicação

- `VAR PIB_anterior`: declara uma variável temporária chamada `PIB_anterior`, que armazena o valor do PIB calculado para o ano anterior.
- `DATEADD('d_calendário'[Date], -1, YEAR)`: desloca a data atual em **−1 ano**, ou seja, retorna os dados do mesmo período do ano anterior.
- `DIVIDE(numerador, denominador, alternativa)`: realiza a divisão de forma segura. Se o denominador for zero ou nulo, retorna a `alternativa` (neste caso, um espaço em branco `" "`), evitando erros no visual.

### Formatação

Após criar esta medida, é necessário formatá-la como porcentagem:

1. Selecione a medida `Tx_crescimento%` no painel de campos (clique sobre ela uma vez).
2. No menu superior, a aba **"Ferramentas de Medida"** será exibida automaticamente.
3. Dentro dessa aba, localize o campo de formatação (geralmente exibe "Geral" por padrão).
4. Clique no menu suspenso e selecione **"Porcentagem"**.
5. Ao lado, ajuste as casas decimais para **1**.

---

### 7.4 Valores Agregados dos Setores (Ótica da Oferta)

### Objetivo

Criar medidas que isolam o **valor agregado** de cada um dos três grandes setores da economia pela ótica da oferta: Agropecuária, Indústria e Serviços.

### Motivo

A ótica da produção (ou oferta) decompõe o PIB pelo valor adicionado em cada setor. Para comparar a participação relativa e a evolução de cada setor, é necessário calcular o valor agregado de cada um separadamente.

### Valor Agregado da Agropecuária

```dax
VA_agro =
CALCULATE(
    [Soma_valor],
    PIB[Setores e subsetores (Código)] = "90687"
)
```

### Valor Agregado da Indústria

```dax
VA_indústria =
CALCULATE(
    [Soma_valor],
    PIB[Setores e subsetores (Código)] = "90691"
)
```

### Valor Agregado dos Serviços

```dax
VA_serviços =
CALCULATE(
    [Soma_valor],
    PIB[Setores e subsetores (Código)] = "90696"
)
```

### Explicação

Cada medida utiliza `CALCULATE` para filtrar a coluna `Setores e subsetores (Código)` pelo código correspondente a cada setor. Os códigos utilizados são os identificadores oficiais do IBGE na Tabela 6612:

| Código | Setor |
|---|---|
| `90687` | Agropecuária |
| `90691` | Indústria |
| `90696` | Serviços |

> **Atenção**
>
> Nos nomes das medidas `VA_indústria` e `VA_serviços`, a referência à tabela dentro da fórmula deve ser `PIB` (não `f_PIB`). O original continha o texto `f_PIB[Setores e subsetores (Código)]`, que parece ser um erro tipográfico. A referência correta é `PIB[Setores e subsetores (Código)]`.

---

### 7.5 Números-Índice dos Setores

### Objetivo

Criar medidas que calculam o **número-índice** de cada setor, tomando como base o primeiro ano disponível na série histórica (igual a 100).

### Motivo

O número-índice permite comparar a evolução de cada setor ao longo do tempo de forma relativa, independentemente das diferenças de magnitude entre setores. Com o primeiro ano como base 100, é possível visualizar facilmente qual setor cresceu mais desde o início da série.

### Número-Índice da Agropecuária

```dax
Indice_Agro =
VAR ano_base =
    CALCULATE(
        MIN(d_calendário[Ano]),
        ALL(d_calendário)
    )
VAR Agro_base =
    CALCULATE(
        [VA_agro],
        ALL(d_calendário),
        d_calendário[Ano] = ano_base
    )
RETURN
DIVIDE(
    [VA_agro],
    Agro_base
) * 100
```

### Número-Índice da Indústria

```dax
Indice_Industria =
VAR ano_base =
    CALCULATE(
        MIN(d_calendário[Ano]),
        ALL(d_calendário)
    )
VAR Industria_base =
    CALCULATE(
        [VA_indústria],
        ALL(d_calendário),
        d_calendário[Ano] = ano_base
    )
RETURN
DIVIDE(
    [VA_indústria],
    Industria_base
) * 100
```

### Número-Índice dos Serviços

```dax
Indice_Serviços =
VAR ano_base =
    CALCULATE(
        MIN(d_calendário[Ano]),
        ALL(d_calendário)
    )
VAR servicos_base =
    CALCULATE(
        [VA_serviços],
        ALL(d_calendário),
        d_calendário[Ano] = ano_base
    )
RETURN
DIVIDE(
    [VA_serviços],
    servicos_base
) * 100
```

### Explicação

- `VAR ano_base`: calcula o menor ano presente na tabela `d_calendário`, ignorando quaisquer filtros ativos (`ALL(d_calendário)` remove todos os filtros da tabela de datas).
- `VAR [setor]_base`: calcula o valor agregado do setor no `ano_base`, também removendo todos os filtros de data. Esse é o valor de referência (denominador da divisão).
- `DIVIDE([VA_setor], [setor]_base) * 100`: divide o valor atual do setor pelo valor no ano-base e multiplica por 100, resultando em um índice onde o ano-base é igual a 100.

---

### 7.6 Taxa de Crescimento dos Subsetores

### Objetivo

Criar uma medida que calcula a taxa de variação anual para **todos os subsetores** da tabela PIB, permitindo comparar o desempenho de cada subsetor em um determinado ano.

### Fórmula

```dax
Tx_Crescimento_Setores% =
VAR PIB_anterior = CALCULATE(
    [Soma_valor],
    DATEADD('d_calendário'[Date], -1, YEAR)
)
RETURN
DIVIDE(
    ([Soma_valor] - PIB_anterior),
    PIB_anterior,
    " "
)
```

### Explicação

A lógica é idêntica à da medida `Tx_crescimento%` (Seção 7.3), mas utiliza `[Soma_valor]` em vez de `[PIB_valor]`. Isso significa que a taxa é calculada para **qualquer agrupamento de dados** que estiver no contexto do visual — incluindo subsetores individuais quando o gráfico estiver filtrado por setor.

### Formatação

Após criar esta medida:

1. Selecione a medida `Tx_Crescimento_Setores%` no painel de campos.
2. Na aba **"Ferramentas de Medida"**, altere o formato para **"Porcentagem"** com **1** casa decimal.

---

### 7.7 Participação Relativa dos Setores no PIB (Shares)

### Objetivo

Criar medidas que calculam a **participação percentual** (share) de cada componente no PIB total.

### Motivo

Os shares permitem visualizar a **estrutura** do PIB, ou seja, qual proporção do total é representada por cada componente. Isso é especialmente relevante para a ótica da demanda (consumo das famílias, governo, investimento e setor externo) e para a comparação entre setores produtivos.

### Formatação Comum a Todos os Shares

Após criar **cada uma** das medidas de share descritas abaixo, formate-a como porcentagem com 1 casa decimal (mesmo procedimento descrito na Seção 7.3).

---

#### Share da Agropecuária no PIB

```dax
Share%agro =
DIVIDE(
    [VA_agro],
    [PIB_valor],
    " "
)
```

#### Share da Indústria no PIB

```dax
Share%ind =
DIVIDE(
    [VA_indústria],
    [PIB_valor],
    " "
)
```

#### Share dos Serviços no PIB

```dax
Share%serv =
DIVIDE(
    [VA_serviços],
    [PIB_valor],
    " "
)
```

---

#### Share do Consumo das Famílias no PIB

```dax
Share%familias =
VAR Consumo_familias =
    CALCULATE(
        [Soma_valor],
        PIB[Setores e subsetores (Código)] = "93404"
    )
RETURN
DIVIDE(
    Consumo_familias,
    [PIB_valor],
    " "
)
```

#### Share do Gasto do Governo no PIB

```dax
Share%governo =
VAR gasto_governo =
    CALCULATE(
        [Soma_valor],
        PIB[Setores e subsetores (Código)] = "93405"
    )
RETURN
DIVIDE(
    gasto_governo,
    [PIB_valor],
    " "
)
```

#### Share do Investimento (FBCF) no PIB

```dax
Share%investimento =
VAR FBCF =
    CALCULATE(
        [Soma_valor],
        PIB[Setores e subsetores (Código)] = "93406"
    )
RETURN
DIVIDE(
    FBCF,
    [PIB_valor],
    " "
)
```

#### Share da Balança Comercial no PIB

```dax
Share%Balança_comercial =
VAR exportacoes =
    CALCULATE(
        [Soma_valor],
        PIB[Setores e subsetores (Código)] = "93407"
    )
VAR importacoes =
    CALCULATE(
        [Soma_valor],
        PIB[Setores e subsetores (Código)] = "93408"
    )
VAR saldo = exportacoes - importacoes
RETURN
DIVIDE(
    saldo,
    [PIB_valor],
    " "
)
```

### Explicação dos Códigos (Ótica da Demanda)

| Código | Componente |
|---|---|
| `93404` | Consumo das famílias |
| `93405` | Consumo do governo |
| `93406` | Formação Bruta de Capital Fixo (FBCF) / Investimento |
| `93407` | Exportações |
| `93408` | Importações |

> **Nota Técnica**
>
> Para o share da balança comercial, o saldo é calculado como `exportações − importações`. Como as importações representam uma saída de recursos da economia doméstica, o saldo pode ser negativo em anos de déficit comercial, o que é economicamente esperado e correto.

> **Observação sobre os shares da ótica da demanda**
>
> Diferentemente dos shares setoriais (agro, indústria, serviços), os componentes da demanda (famílias, governo, FBCF, balança comercial) não possuem medidas de valor agregado calculadas previamente. Por isso, essas fórmulas utilizam uma variável local (`VAR`) para calcular o valor do componente diretamente dentro da medida de share. Essa abordagem é equivalente ao resultado que seria obtido criando medidas separadas de valor agregado para cada componente.

---

## 8. Construindo os Dashboards

Com todas as medidas criadas, podemos agora construir os painéis visuais do relatório. Os dashboards serão criados na tela principal do Power BI, na área de edição de relatório (a tela em branco que aparece ao abrir o Power BI na visualização de "Relatório").

> **Dica**
>
> Para adicionar qualquer visualização ao relatório, localize o painel **"Visualizações"**, situado no lado direito da tela (abaixo do painel de campos). Esse painel exibe ícones representando os diferentes tipos de gráficos e visuais disponíveis. Clique no ícone desejado para inserir um visual em branco na tela. Em seguida, use o painel de campos para arrastar as medidas e dimensões para os campos do visual.

---

### 8.1 Título do Relatório

### Objetivo

Inserir um título textual no relatório para identificar o conteúdo do dashboard.

### Passo a Passo

1. No menu superior do Power BI, clique na aba **"Inserir"**.
2. Dentro dessa aba, clique no botão **"Caixa de Texto"**.
3. Uma caixa de texto em branco será inserida na tela do relatório. Clique dentro dela e digite:

   ```
   Análise do PIB Anual pela Ótica da Demanda e da Oferta
   ```

4. Selecione o texto digitado.
5. Na barra de formatação que aparecerá acima da caixa de texto, configure:
   - **Fonte:** Segoe UI
   - **Tamanho:** 16
   - **Estilo:** Negrito (clique no ícone "N" ou pressione Ctrl+B)
   - **Alinhamento:** Centralizado (clique no ícone de centralização ou pressione Ctrl+E)
6. Clique fora da caixa de texto para deselecionar e visualizar o resultado.
7. Posicione a caixa de texto na parte superior da tela de relatório, arrastando-a pela borda.

---

### 8.2 Segmentação de Dados por Ano

### Objetivo

Criar um filtro interativo (segmentação de dados) que permita ao usuário selecionar um ano específico e atualizar automaticamente todos os visuais do relatório.

### Motivo

A segmentação de dados é o principal mecanismo de interação do usuário com o dashboard. Ao selecionar um ano, todos os visuais conectados à tabela `d_calendário` serão filtrados para exibir apenas os dados daquele ano.

### Passo a Passo

1. No painel **"Visualizações"** (lateral direita), clique no ícone de **"Segmentação de Dados"** (geralmente representado por um funil ou um retângulo com linhas horizontais).
2. Um visual de segmentação em branco será inserido na tela.
3. No painel de campos (lateral direita), expanda a tabela **`d_calendário`**.
4. Dentro dela, expanda o campo **`Date`** (pode aparecer como uma hierarquia).
5. Arraste o campo **`Ano`** para o campo **"Campo"** da segmentação (ou clique na caixinha ao lado de `Ano` com o visual de segmentação selecionado).
6. Com o visual de segmentação ainda selecionado, acesse o painel **"Visualizações"** e clique na aba **"Formatar seu visual"** (ícone de rolo de pintura).
7. Dentro das opções de formatação, localize a seção **"Configurações de Segmentação"** (ou "Segmentação de Dados").
8. No campo **"Estilo"**, selecione a opção **"Suspenso"** (dropdown).

### Resultado Esperado

A segmentação exibirá um menu suspenso com os anos disponíveis na base de dados. Ao selecionar um ano, os demais visuais do relatório serão atualizados automaticamente para refletir os dados daquele período.

---

### 8.3 Cartão: Valor do PIB

### Objetivo

Exibir o valor total do PIB (em reais, a preços de 1995) para o período selecionado na segmentação de dados.

### Passo a Passo

1. No painel **"Visualizações"**, clique no ícone de **"Cartão"** (um retângulo com um número grande centralizado).
2. Um visual de cartão em branco será inserido na tela.
3. No painel de campos, localize a medida **`PIB_valor`** na tabela `Medidas`.
4. Arraste `PIB_valor` para o campo **"Campos"** do visual de cartão.

### Resultado Esperado

O cartão exibirá o valor numérico do PIB a preços de mercado para o período selecionado. Quando nenhum ano estiver selecionado na segmentação, o valor exibido corresponderá à soma de todos os anos disponíveis.

---

### 8.4 Cartão: Taxa de Crescimento do PIB

### Objetivo

Exibir a taxa de variação anual do PIB em relação ao ano anterior.

### Passo a Passo

1. No painel **"Visualizações"**, clique novamente no ícone de **"Cartão"**.
2. Arraste a medida **`Tx_crescimento%`** para o campo **"Campos"** do novo visual de cartão.

### Resultado Esperado

O cartão exibirá a taxa de crescimento do PIB no formato de porcentagem (ex.: 2,3%). O valor será atualizado conforme o ano selecionado na segmentação.

---

### 8.5 Gráfico de Linhas: Evolução do PIB

### Objetivo

Criar um gráfico de linha que mostre a evolução histórica do PIB ao longo dos anos.

### Passo a Passo

1. No painel **"Visualizações"**, clique no ícone de **"Gráfico de Linhas"** (representado por uma linha diagonal com pontos).
2. Um visual de gráfico de linhas em branco será inserido na tela.
3. Configure os campos do gráfico:
   - **Eixo X:** arraste `d_calendário` → `Date` → Hierarquia → **`Ano`** para o campo "Eixo X".
   - **Eixo Y:** arraste a medida **`PIB_valor`** para o campo "Eixo Y".
4. Para renomear o rótulo do eixo Y sem alterar o nome da medida, clique duas vezes sobre o nome `PIB_valor` na área de campos do visual e substitua pelo texto **`PIB`**.

### Resultado Esperado

O gráfico exibirá uma linha representando a evolução do PIB ao longo do tempo, com os anos no eixo horizontal e os valores no eixo vertical.

---

### 8.6 Gráfico de Colunas: Variação por Índice Setorial

### Objetivo

Criar um gráfico de colunas empilhadas que compare a evolução dos índices dos três setores (Agropecuária, Indústria e Serviços) ao longo dos anos.

### Passo a Passo

1. No painel **"Visualizações"**, clique no ícone de **"Gráfico de Colunas Empilhadas"**.
2. Configure os campos:
   - **Eixo X:** arraste `d_calendário` → `Date` → Hierarquia → **`Ano`**.
   - **Eixo Y:** arraste as medidas **`Indice_Agro`**, **`Indice_Industria`** e **`Indice_Serviços`** para o campo "Eixo Y" (arraste uma de cada vez).
3. Para renomear os rótulos sem alterar os nomes das medidas, clique duas vezes sobre cada medida na área de campos do visual e substitua pelos nomes: **`Agropecuária`**, **`Indústria`** e **`Serviços`**.

---

### 8.7 Gráfico de Colunas: PIB pela Ótica da Oferta (Shares)

### Objetivo

Criar um gráfico de colunas empilhadas que mostre a participação percentual dos três setores no PIB ao longo do tempo (shares pela ótica da oferta).

### Passo a Passo

1. No painel **"Visualizações"**, clique no ícone de **"Gráfico de Colunas Empilhadas"**.
2. Configure os campos:
   - **Eixo X:** arraste `d_calendário` → `Date` → Hierarquia → **`Ano`**.
   - **Eixo Y:** arraste as medidas **`Share%agro`**, **`Share%ind`** e **`Share%serv`**.
3. Renomeie os rótulos na área de campos do visual para: **`Agropecuária`**, **`Indústria`** e **`Serviços`**.

---

### 8.8 Gráfico de Barras: Crescimento por Subsetor Industrial

### Objetivo

Criar um gráfico de barras empilhadas que exiba a taxa de crescimento dos subsetores **industriais** para o ano selecionado.

### Passo a Passo

1. No painel **"Visualizações"**, clique no ícone de **"Gráfico de Barras Empilhadas"** (barras na horizontal).
2. Configure os campos:
   - **Eixo Y (categorias):** arraste o campo **`Setores e subsetores`** da tabela `PIB`.
   - **Eixo X (valores):** arraste a medida **`Tx_Crescimento_Setores%`**.
3. Com o visual selecionado, localize o painel **"Filtros"** (lateral direita, acima do painel de campos). Se não estiver visível, clique no ícone de funil ou acesse-o pelo painel de visualizações.
4. Em **"Filtros neste visual"**, clique sobre o campo `Setores e subsetores` para expandi-lo.
5. Selecione o tipo de filtro **"Filtragem básica"**.
6. Na lista de subsetores exibida, marque **apenas** os seguintes itens:
   - Construção
   - Eletricidade e gás, água, esgoto e atividades de gestão de resíduos
7. Ainda no painel de filtros, adicione um filtro para o campo **`Ano`** (da tabela `d_calendário`):
   - Tipo de filtro: **"N Superior"**
   - Em "Mostrar itens", selecione: **"Superior"**
   - Quantidade: **1**
   - Em "Por valor", selecione: **"Média de Ano"**

> **Por que aplicar o filtro "N Superior" no campo Ano?**
>
> Esse filtro garante que o gráfico exiba automaticamente os dados do ano mais recente disponível na base, mesmo sem que o usuário precise selecionar um ano na segmentação. Com essa configuração, o visual sempre mostrará o último ano com dados disponíveis.

---

### 8.9 Gráfico de Barras: Crescimento por Subsetor de Serviços

### Objetivo

Criar um gráfico de barras empilhadas que exiba a taxa de crescimento dos subsetores de **serviços** para o ano selecionado.

### Passo a Passo

Repita o mesmo procedimento da Seção 8.8, mas na etapa de filtragem dos subsetores, selecione **apenas** os seguintes itens:

- Atividades financeiras
- Atividades imobiliárias
- Comércio
- Informação e comunicação
- Transporte, armazenagem e correio
- Outras atividades de serviços

Mantenha o mesmo filtro de "N Superior" para o campo `Ano`.

---

### 8.10 Gráfico de Colunas: PIB pela Ótica da Demanda

### Objetivo

Criar um gráfico de colunas empilhadas que mostre a participação percentual dos componentes da demanda no PIB (consumo das famílias, governo, investimento e balança comercial) ao longo dos anos.

### Passo a Passo

1. No painel **"Visualizações"**, clique no ícone de **"Gráfico de Colunas Empilhadas"**.
2. Configure os campos:
   - **Eixo X:** arraste `d_calendário` → `Date` → Hierarquia → **`Ano`**.
   - **Eixo Y:** arraste as medidas **`Share%familias`**, **`Share%investimento`**, **`Share%Balança_comercial`** e **`Share%governo`**.
3. Renomeie os rótulos na área de campos do visual para: **`Famílias`**, **`Investimento`**, **`Balança Comercial`** e **`Governo`**.

---

## 9. Títulos Dinâmicos

### Objetivo

Criar títulos para os gráficos que se atualizam automaticamente conforme o ano selecionado na segmentação de dados.

### Motivo

Quando o usuário filtra o dashboard para um determinado ano, os valores nos gráficos se atualizam, mas os títulos permanecem estáticos (por exemplo, "Evolução do PIB" não muda para "Evolução do PIB — 2020" automaticamente). Os **títulos dinâmicos** resolvem esse problema, conectando o texto do título à seleção ativa na segmentação de dados.

### Criando as Medidas de Título

As medidas de título dinâmico são criadas na tabela **`Títulos`**. Para cada medida, siga o procedimento padrão de criação de medidas (clique com o botão direito na tabela `Títulos` → "Nova Medida").

#### Medida: Título do PIB

```dax
TítuloPIB =
"PIB | "
&
SELECTEDVALUE(
    'd_calendário'[Ano],
    "Todos os anos"
)
```

#### Medida: Título da Taxa de Crescimento

```dax
TítuloTx_Crescimento =
"Taxa de Crescimento do PIB | "
&
SELECTEDVALUE(
    'd_calendário'[Ano],
    "Todos os anos"
)
```

#### Medida: Título Subsetor Industrial

```dax
TítuloSubsetor_Ind =
"Crescimento por Subsetor Industrial | "
&
SELECTEDVALUE(
    'd_calendário'[Ano],
    "Todos os anos"
)
```

#### Medida: Título Subsetor de Serviços

```dax
TítuloSubsetor_Serv =
"Crescimento por Subsetor de Serviços | "
&
SELECTEDVALUE(
    'd_calendário'[Ano],
    "Todos os anos"
)
```

### Explicação da Fórmula

- `"PIB | "`: parte estática do título, que sempre permanecerá igual.
- `&`: operador de concatenação de texto no DAX (equivalente ao `+` para texto).
- `SELECTEDVALUE('d_calendário'[Ano], "Todos os anos")`: retorna o valor do campo `Ano` que está selecionado na segmentação. Se nenhum ano estiver selecionado (ou mais de um estiver selecionado), retorna o valor alternativo `"Todos os anos"`.

### Aplicando o Título Dinâmico a um Gráfico

Para vincular uma medida de título a um gráfico existente:

1. Clique sobre o gráfico ao qual deseja aplicar o título dinâmico.
2. No painel **"Visualizações"** (lateral direita), clique na aba **"Formatar seu visual"** (ícone de rolo de pintura).
3. Dentro das opções de formatação, localize e expanda a seção **"Geral"**.
4. Dentro de "Geral", expanda a seção **"Título"**.
5. Ao lado do campo de texto do título, localize o ícone **"fx"** (formatação condicional) e clique sobre ele.
6. Uma janela chamada **"Título — Estilo do Formato"** será aberta. Configure:
   - **Estilo do formato:** selecione `"Valor do campo"`.
   - **Em que campo devemos basear isso?:** expanda a tabela `Títulos` e selecione a medida de título correspondente ao gráfico (ex.: `TítuloPIB` para o gráfico de evolução do PIB).
7. Clique em **"OK"** para confirmar.

### Resultado Esperado

O título do gráfico deixará de ser um texto fixo e passará a exibir o resultado da medida de título dinâmico. Quando nenhum ano for selecionado, o título exibirá, por exemplo, "PIB | Todos os anos". Ao selecionar o ano 2020 na segmentação, o título será atualizado automaticamente para "PIB | 2020".

---

## 10. Configurando as Interações entre Visualizações

### Objetivo

Configurar quais visuais respondem ou não à seleção na segmentação de dados por ano, garantindo que apenas os gráficos relevantes sejam filtrados.

### Motivo

Por padrão, quando o usuário seleciona um valor em uma segmentação, **todos** os visuais da página são filtrados. No entanto, alguns gráficos — especialmente aqueles que exibem uma série histórica completa (como o gráfico de linhas da evolução do PIB) — não devem ser filtrados pela segmentação de ano, pois perderiam o contexto temporal que os torna informativos. A configuração de interações resolve esse conflito.

### Passo a Passo

1. Clique **uma vez** sobre o visual de **segmentação de dados** (o filtro de ano criado na Seção 8.2) para selecioná-lo.
2. No menu superior do Power BI, clique na aba **"Formato"** (ou "Formatar").
3. Dentro dessa aba, localize e clique no botão **"Editar Interações"**.
4. Após clicar nesse botão, você notará que ícones de controle surgirão sobre cada um dos outros visuais na página. Esses ícones permitem configurar como cada visual responde à segmentação selecionada. Para cada visual, há geralmente duas opções:
   - **Ícone de filtro (funil):** o visual **será filtrado** pela segmentação.
   - **Ícone de proibido (círculo com barra):** o visual **não será filtrado** pela segmentação.
5. Para cada gráfico que exibe uma série histórica completa (e portanto **não deve** ser filtrado pela segmentação de ano), clique sobre o **ícone de proibido** exibido sobre esse visual.
6. Para os visuais que **devem** ser filtrados (cartões, gráficos pontuais por ano, etc.), certifique-se de que o ícone de filtro esteja ativo.
7. Após configurar as interações de todos os visuais, clique novamente no botão **"Editar Interações"** para sair do modo de edição e salvar as configurações.

### Quais Visuais Bloquear

Como regra geral, bloqueie a interação com a segmentação de anos para os visuais que **mostram toda a série histórica** (como o gráfico de linha de evolução do PIB e o gráfico de índices setoriais). Mantenha a interação ativa para os visuais que exibem **valores pontuais** de um ano específico (como os cartões de valor do PIB e taxa de crescimento).

### Resultado Esperado

Ao selecionar um ano na segmentação, apenas os visuais configurados para interagir serão atualizados. Os visuais bloqueados continuarão exibindo a série histórica completa, permitindo uma visualização contextualizada dos dados.

---

## Observações Finais

> **Importante**
>
> Certifique-se de salvar o arquivo do Power BI regularmente ao longo do processo (Ctrl+S ou Arquivo → Salvar). O Power BI Desktop salva arquivos no formato `.pbix`.

> **Dica**
>
> Para organizar melhor o layout do dashboard, utilize a função de alinhamento disponível na aba **"Formato"** → **"Alinhar"**. Ela permite alinhar e distribuir os visuais de forma uniforme na tela.

> **Atenção**
>
> Caso alguma medida retorne erro ou resultado inesperado, verifique: (1) se o nome da tabela referenciado na fórmula DAX corresponde exatamente ao nome exibido no painel de campos; (2) se o relacionamento entre `d_calendário` e `PIB` está configurado corretamente; e (3) se os tipos de dados das colunas foram definidos conforme descrito na Seção 3.4.

---

*Roteiro elaborado para uso didático — FACEU, Grupo 1.*
