# Análise das Relações Leitor-Livro: Características e Padrões de Consumo

## Visão Geral do Projeto

Este projeto visa realizar uma análise aprofundada das relações entre **leitores e livros**, com o objetivo de compreender as características dos leitores e os padrões dos livros que eles consomem, leem, abandonam ou favoritam. Aplicamos uma abordagem interdisciplinar, inspirada no conceito de um "corpo ou indivíduo" como molde analítico, para mapear influências contextuais e sociais no comportamento de leitura.

Focamos em plataformas digitais de leitura e comércio, priorizando:
- **Skoob**: Principal fonte para dados sobre interações reais de leitores (avaliações, favoritos, abandonos e resenhas).
- **Amazon**: Complementar para análise de custos e acessibilidade econômica dos livros.

A acessibilidade (física, digital, financeira e cultural) é um fator central, pois interfere diretamente nas decisões de consumo literário.

## Abordagem de Coleta de Dados

Adotamos uma estratégia multi-plataforma para capturar dados ricos e diversificados:

### Fontes Principais
- **Skoob**: Extração de dados públicos sobre perfis de usuários, listas de leitura, avaliações (notas de 1 a 5), status de leitura (lido, lendo, abandonado, quer ler) e prateleiras personalizadas.
- **Amazon**: Coleta de preços, classificações, disponibilidade (e-book, físico, audiolivro) e metadados de livros (gênero, autor, ano de publicação).

### Metodologia de Coleta
1. **Web Scraping Ético**: Utilização de ferramentas como BeautifulSoup e Selenium.
2. **Pré-processamento**: Limpeza de dados para remoção de duplicatas, normalização de títulos/autores e tratamento de valores ausentes.
3. **Amostragem**: Foco inicial nos top 100 livros de diferentes contextos.

## Objetivos e Expectativas

- Identificar perfis de leitores (idade, gênero, localização) e correlações com gêneros literários.
- Analisar padrões de abandono e favoritamento, considerando custo e acessibilidade.

## Explicando o dataset
O dataset reúne informações sobre livros cadastrados na plataforma Skoob, uma espécie de rede social brasileira para leitores. Cada linha representa um livro e as colunas representam título, autor, número de páginas, tempo estimado de leitura, nota média, as quantidades de usuários que marcaram o livro em diferentes status (leram, estão lendo, querem ler, estão relendo, abandonaram), há também contagens de resenhas, favoritos e avaliações. Esse conjunto pode ser usado para análises sobre popularidade, preferências de leitura, correlação entre número de páginas e nota, ou para entender quais livros estão em alta na comunidade.

## Processo de coleta dos dados

A coleta foi feita em duas etapas:
  1. Obtenção dos links dos livros
    Utilizou-se a API pública do Skoob **/top-books** para pegar os 100 livros mais lidos do momento.
    Também se usou uma busca via **POST** na página de pesquisa para obter uma lista adicional de livros (apenas 36, para teste).
    Os links foram salvos em um arquivo texto (links_todos_livros.txt).

  2. Raspagem dos dados de cada livro
    Para cada link, foi feita uma requisição **GET** com cabeçalhos autenticados (cookie válido) para a página do livro.
    O HTML foi processado com **BeautifulSoup** e os dados foram extraídos usando classes CSS específicas que aparecem na estrutura do site.
    Os resultados foram salvos incrementalmente em um arquivo CSV, com pausas entre as requisições para evitar sobrecarga no servidor.

Todo o processo foi executado em Python (Jupyter Notebook), utilizando as bibliotecas **requests, BeautifulSoup, pandas e json**.

## Descrição das Colunas

| Nome da coluna       | Descrição                                                                 | Exemplo                     |
----------------------------------------------------------------------------------------------------------------------------------
| Título do Livro      | Nome completo do livro, conforme aparece na página.                       | Alchemised [Edição brasileira] |
| Autor                | Nome do autor (ou autores) do livro.                                      | SenLinYu                    |
| Quantidade de Páginas| Número de páginas informado na edição.                                    | 900 páginas                 |
| Tempo de Leitura     | Estimativa de tempo para ler o livro, calculada pelo Skoob.               | 1d 6h 0m                    |
| Nota Média           | Média das notas dadas pelos usuários (de 0 a 5).                          | 4.5                         |
| Leram                | Número de usuários que marcaram o livro como "li".                        | 9155                        |
| Lendo                | Número de usuários que marcaram "estou lendo".                            | 4786                        |
| Querem               | Número de usuários que marcaram "quero ler".                              | 46897                       |
| Relendo              | Número de usuários que marcaram "relendo".                                | 43                          |
| Abandonos            | Número de usuários que abandonaram a leitura.                             | 590                         |
| Resenhas             | Número de resenhas escritas para o livro.                                 | 2364                        |
| Favoritos            | Número de usuários que adicionaram o livro aos favoritos.                 | 1640                        |
| Avaliaram            | Número de usuários que avaliaram o livro (deram nota).                    | 6826                        |

##Link do Drive
