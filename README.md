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
