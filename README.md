# Análise das Relações Leitor-Livro: Características e Padrões de Consumo

## Introdução

### Motivação
A democratização da leitura e o acesso à literatura são temas fundamentais na sociedade contemporânea. No entanto, o consumo literário não é moldado apenas pelo interesse pessoal, mas por uma complexa teia de fatores de acessibilidade (física, digital, financeira e cultural). A motivação deste projeto nasceu da necessidade de entender como essas barreiras ou facilidades influenciam o comportamento real do leitor brasileiro. 

### Objetivos da Análise
Este projeto aplica uma abordagem interdisciplinar da Ciência de Dados para realizar uma análise aprofundada das relações entre **leitores e livros**. Nossos principais objetivos são:
- Mapear os padrões de engajamento (o que faz um livro ser muito lido, favoritado ou abandonado).
- Quantificar a acessibilidade financeira, comparando o impacto do custo de edições físicas (novas) versus edições digitais (e-books) na popularidade das obras.
- Identificar correlações ocultas, como a relação entre o volume da obra (quantidade de páginas) e a taxa de abandono, ou como o ano de publicação e a editora afetam a nota média perante a comunidade.

## Dados Usados

### Descrição do Dataset
O dataset construído reúne informações ricas sobre livros extraídos de plataformas digitais de leitura e comércio. Cada linha do arquivo CSV representa uma obra literária única, cruzando métricas de engajamento social com valores reais de mercado. 

As fontes de dados foram:
- **Skoob:** RRede social para leitores do Brasil, utilizada para extrair o "comportamento social" em torno de uma obra (avaliações, abandonos, resenhas, status de leitura e metadados estruturais como páginas, ano e editora).
- **Google Books API & Estante Virtual:** Utilizadas para mapear a acessibilidade econômica das obras através da obtenção, respectivamente, do preço do e-book e o valor de mercado de exemplares físicos novos no Brasil.

### Exploração Inicial dos Dados
A base de dados conta com milhares de registros coletados iterativamente. A exploração inicial revela dados de natureza mista: variáveis categóricas (Título, Autor, Editora), variáveis numéricas discretas (contagens de usuários que leram, abandonaram, etc.) e variáveis numéricas contínuas (Nota Média e Preços). Observou-se inicialmente a presença de ruídos nos textos (como subtítulos de edições especiais) e formatos numéricos em padrão brasileiro, que exigiram tratamento em etapas posteriores.

## Pré-processamento e Metodologia de Coleta

A coleta e preparação dos dados seguiram um pipeline em Python (Jupyter Notebook), utilizando bibliotecas como `requests`, `BeautifulSoup` e `pandas`.

### 1. Web Scraping Ético e Coleta Híbrida
- **Mapeamento:** Utilizou-se a API pública do Skoob (`/top-books`) e requisições via `POST` decodificando o formato interno do *Next.js* para obter as URLs-alvo.
- **Extração Social:** Requisições `GET` autenticadas por cookies de sessão varreram o HTML do Skoob, extraindo as métricas por meio de classes CSS e IDs específicos.
- **Busca Dupla de Preços:** Foi desenvolvido um sistema de fallback/busca híbrida. O script consome a API do Google Cloud para buscar a versão digital e realiza o scraping da Estante Virtual para a versão física, garantindo dados sobre diferentes formatos de consumo.

### 2. Limpeza e Transformação dos Dados
Decisões técnicas fundamentais foram tomadas para garantir a qualidade analítica da base:
- **Pesquisa de Títulos - Autor:** Títulos contendo tags como `[Edição de Colecionador]` ou subtítulos longos após dois pontos (`:`) foram higienizados via Expressões Regulares antes de serem enviados às APIs de preço. Além disso, a pesquisa de cada uma das obrasfdoi realizada misturando o Título e seu Autor como forma de evitar que diferentes obras se associassem em um mesmo cálculo. Essas ações evitaram erros de "livro não encontrado" ou uso de obras não desejadas e aumentaram drasticamente a taxa de sucesso da busca de valores. 
- **Filtro de Anomalias Financeiras:** Durante a coleta da Estante Virtual, o algoritmo foi programado para calcular a média de valores de um mesmo livro, rejeitando assim que estratégias de venda (como promoções, coleções, etc) distorcessem a média de preço de um livro individual. Além disso, filtrou-se explicitamente por `condition=novo` para não misturar preços de livros originais com os de sebos.
- **Normalização Numérica:** As colunas de popularidade vieram da web em formato string (ex: `"1.500"` ou textos com quebras de linha `\xa0`). Esses dados foram iterados, limpos e convertidos para tipos numéricos (`int` e `float`) do Pandas, padronizando os decimais para o formato computacional internacional (ponto em vez de vírgula).
- **Persistência Incremental:** O salvamento foi configurado em lotes (modo `append`), garantindo que falhas de conexão não causassem perda de dados e permitindo que livros não encontrados fossem registrados como `NaN` para posterior análise de dados faltantes.

## 📊 Dicionário de Dados

| Nome da Coluna | Descrição | Exemplo |
| :--- | :--- | :--- |
| **Título do Livro** | Nome completo do livro, higienizado. | Alchemised |
| **Autor** | Nome do autor (ou autores) da obra. | SenLinYu |
| **Ano** | Ano de publicação da edição correspondente. | 2024 |
| **Editora** | Editora responsável pela publicação. | Intrínseca |
| **Quantidade de Páginas**| Número de páginas informado na edição. | 900 |
| **Tempo de Leitura** | Estimativa média de tempo para ler o livro. | 1d 6h 0m |
| **Nota Média** | Avaliação da comunidade (escala de 0 a 5). | 4.5 |
| **Preço Digital (R$)** | Preço oficial do e-book na Google Play Livros. | 39.90 |
| **Preço Físico (R$)** | Média de preço de exemplares físicos novos. | 65.50 |
| **Leram** | Número de usuários que marcaram "li". | 9155 |
| **Lendo** | Número de usuários que marcaram "estou lendo". | 4786 |
| **Querem** | Número de usuários que marcaram "quero ler". | 46897 |
| **Relendo** | Número de usuários que marcaram "relendo". | 43 |
| **Abandonos** | Número de usuários que registraram abandono. | 590 |
| **Resenhas** | Volume de críticas literárias publicadas. | 2364 |
| **Favoritos** | Quantidade de inclusões como "livro favorito". | 1640 |
| **Avaliaram** | Número total de avaliações (ratings) recebidas. | 6826 |

## 🔗 Acesso aos Dados

**Link do Drive:** https://drive.google.com/file/d/1XiZRasl5x5e-ncPhUdUKzYSsUZqe9ZkB/view?usp=drive_link
