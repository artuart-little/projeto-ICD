# Análise das Relações Leitor-Livro: Características e Padrões de Consumo

## Introdução

### Motivação
A democratização da leitura e o acesso à literatura são temas fundamentais na sociedade contemporânea. No entanto, o consumo literário não é moldado apenas pelo interesse pessoal, mas por uma complexa teia de fatores de acessibilidade (física, digital, financeira e cultural). A motivação deste projeto nasceu da necessidade de entender como essas barreiras ou facilidades influenciam o comportamento real do leitor brasileiro.

### Objetivos da Análise
Este projeto aplica uma abordagem interdisciplinar obtida na disciplina de Introdução a Ciência de Dados para realizar uma análise das relações entre **leitores e livros**.
Planejamos entender a relação entre entre eles aplicada ao molde de um corpo/indivíduo e o espaço que a obra está inserida.

Nossos principais objetivos são:
- Mapear os padrões de engajamento (o que faz um livro ser muito lido, favoritado ou abandonado).
- Quantificar a acessibilidade financeira, comparando o impacto do custo de edições físicas (novas) versus edições digitais (e-books) na popularidade das obras.
- Identificar correlações ocultas, como a relação entre o volume da obra (quantidade de páginas) e a taxa de abandono, ou como o ano de publicação e a editora afetam a nota média perante a comunidade.

## Dados Usados

### Descrição do Dataset
O dataset construído reúne informações de, praticamente, 15.300 livros extraídos de plataformas digitais de leitura e comércio. Cada linha do arquivo CSV contém uma obra literária, cruzando suas métricas de engajamento social com seu valor no mercado.

Contando com registros coletados iterativamente, o dataset apresenta métricas de funil de cada obra, sendo elas: variáveis de categoria (Título, Autor, Editora), variáveis numéricas discretas (contagens de usuários que leram, abandonaram, etc.) e variáveis numéricas contínuas (Nota Média e Preços).

Observou-se inicialmente a presença de ruídos nos textos, como subtítulos de edições especiais, texto em variáveis que seriam tratadas como númericas e formatos que exigiram tratamento em etapas posteriores.

#### Dicionário do CSV
| Nome da Coluna | Descrição | Exemplo |
| :--- | :--- | :--- |
| **Título do Livro** | Nome completo do livro | Alchemised [Edição brasileira]|
| **Autor** | Nome do autor (ou autores) da obra | SenLinYu |
| **Ano** | Ano de publicação da edição correspondente | 2024 |
| **Editora** | Editora responsável pela publicação| Intrínseca |
| **Quantidade de Páginas**| Número de páginas informado na edição | 900 páginas|
| **Tempo de Leitura** | Estimativa média de tempo para ler o livro | 1d 6h 0m |
| **Nota Média** | Avaliação da comunidade (escala de 0 a 5) | 4.5 |
| **Preço Digital (R\$)** | Preço oficial do e-book na Google Play Livros | 39.90 |
| **Preço Físico (R\$)** | Média de preço de exemplares físicos novos | 65.50 |
| **Leram** | Número de usuários que marcaram "li" | 9155 |
| **Lendo** | Número de usuários que marcaram "estou lendo" | 4786 |
| **Querem** | Número de usuários que marcaram "quero ler" | 46897 |
| **Relendo** | Número de usuários que marcaram "relendo" | 43 |
| **Abandonos** | Número de usuários que registraram abandono.| 590 |
| **Resenhas** | Volume de críticas literárias publicadas | 2364 |
| **Favoritos** | Quantidade de inclusões como "livro favorito" | 1640 |
| **Avaliaram** | Número total de avaliações recebidas | 6826 |

### Exploração Inicial dos Dados

As fontes de dados foram:
- **Skoob:** Rede social para leitores do Brasil, utilizada para extrair o "comportamento social" em torno de uma obra (avaliações, abandonos, resenhas, status de leitura e metadados estruturais como páginas, ano e editora).
- **Google Books API & Estante Virtual:** Utilizadas para mapear a acessibilidade econômica das obras através da obtenção, respectivamente, do preço do e-book e o valor de mercado de exemplares físicos novos no Brasil.

A exploração inicial dos dados seguiram um pipeline em Python (Jupyter Notebook), utilizando bibliotecas como `requests`, `BeautifulSoup` e `pandas` para realização do Web Scrapping que permitiu:

- **Coleta de Link:** Utilizou-se requisições via `POST`  decodificando o formato interno do Next.js da API do Skoob para obter as URLs de cada uma das obras.
- **Extração de Métricas:** Requisições `GET` autenticadas por cookies de sessão varreram o HTML do Skoob, extraindo as métricas de cada obra por meio de classes CSS e IDs específicos.
- **Busca de Preços:** Foi desenvolvido um sistema de busca que, além de consumir a API do Google Cloud para buscar o preço da versão digital da obra, realiza o scraping da Estante Virtual para obter o preço da versão física, garantindo dados sobre diferentes formatos de consumo.
- **Filtro de Anomalias Financeiras:** Durante a coleta da Estante Virtual, o algoritmo foi programado para calcular a média de valores que um mesmo livro poderia apresentar na plataforma, rejeitando assim que estratégias de venda (como promoções, coleções, etc) distorcessem a média de preço de um livro individual. Além disso, filtrou-se explicitamente por `condition=novo` para não misturar preços de livros originais com os de sebos.

## 🔗 Acesso aos Dados

**Link do Drive:** https://drive.google.com/file/d/1XiZRasl5x5e-ncPhUdUKzYSsUZqe9ZkB/view?usp=drive_link
