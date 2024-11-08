# beAnalyticteste
Repositório com o teste prático feito para a empresa beAnalytic

# Exportação dos Dados da API SteamDB

Este projeto tem como objetivo exportar dados da API pública da [SteamDB](https://steamdb.info/), uma plataforma que fornece informações sobre jogos da Steam, incluindo estatísticas, preços, análises e outros dados úteis. Através deste repositório, você encontrará o processo detalhado de como coletar esses dados, organizá-los e exportá-los para uso posterior.

## Tabela de Conteúdos

- [Sobre](#sobre)
- [O que este projeto faz](#oque-este-projeto-faz)
- [Modo de Implantação](#modo-de-implantação)
- [Conexão com o Google Sheets](#conexão-com-o-google-sheets)
- [Melhorias para o Futuro](#melhorias-para-o-futuro)

## Sobre

A API do SteamDB fornece dados detalhados sobre jogos da Steam, como preços, histórico de atualizações, estatísticas de usuários e muito mais. Com este projeto, você pode acessar essa API para obter as informações de que precisa e exportá-las de maneira estruturada, seja para análise, relatórios ou outros usos.

### O que este projeto faz:

1. Conecta-se à API pública da SteamDB através dos notebooks disponíveis na Google Cloud Platform.
2. Coleta informações sobre jogos da Steam, como:
   - Categorias dos Jogos
   - Categorias similares
   - Lista de todos os jogos disponíveis
   - Atualizações e alterações do jogo
   - Recompensas (quais jogos oferecem recompensas aos usuários)
  
3. Organiza e exporta os dados em um formato CSV.
  
### Informações de cada DataFrame
  
**df_categorias:** Contém informações sobre categorias de jogos da Steam. Ele é populado a partir da API da Steam, especificamente de um endpoint próprio para obter as categorias mais populares de jogos.

**df_multiplayer:** É uma filtragem de df_categorias, contendo apenas categorias que incluem o termo "Multi" no nome. Isso faz com que ele agrupe categorias relacionadas a modos multiplayer, para facilitar o tratamento específico desses tipos de jogos.

**df_jogos:** Armazena uma lista de jogos, incluindo colunas como idJogo (ID do jogo), Jogo (nome do jogo), DataUltimaModificacao (data da última modificação, convertida para timestamp), e numeroAlteracoesPreco (número de vezes que o preço foi alterado). Algumas colunas foram adicionadas no processo de tratamento para garantir mais insights sobre aquele dado.

**jogos_recentes e jogos_antigos:** Neste DataFrame, temos os jogos separados de acordo com época. O DataFrame 'jogos recentes' mostra os jogos lançados no último mês, enquanto o 'jogos antigos' representa os jogos disponibilizados no último ano. 

**df_recompensas:** Contém informações sobre recompensas associadas aos jogos, como bônus ou conquistas.

## Modo de Implantação

Através de um notebook no Google Cloud, foi criado todo o ETL (Extração, tratamento e carga dos dados). A base da API veio com dados em um formato fora do padrão,
o que exigiu o tratamento na base para garantir que os dados fiquem no formato mais limpo para, futuramente, consumo.

1. Após conexão com a API da SteamDB dentro do notebook, foi realizado todo processo de extração, desde entender os dados até o envio para um **bucket** dentro do Big Query.

   

3. Foi criado um dataset e um bucket para organização dos dados. O dataset atua como uma camada de organização para os dados dentro do BigQuery, ajudando a separar e gerenciar grandes volumes de dados, enquanto o bucket foi criado para armazenar os dados dentro do Google Cloud Storage.

![image](https://github.com/user-attachments/assets/a77398a0-89c3-4679-b7b4-4e030b0d91dc)

3. Após isso, os dados foram extraídos do bucket para uma tabela contendo as informações retiradas daquele endpoint.

   ![Captura de tela 2024-11-07 140649](https://github.com/user-attachments/assets/39fe7acd-77ba-4e17-9d3f-df8e118c0e50)

Com tudo feito, é possível verificar como ficou a tabela final, após todo o ETL finalizado.

![image](https://github.com/user-attachments/assets/bc6987c7-1f38-4649-bac4-28a2eac83f02)

   
## Conexão com o Google Sheets

Para a visualização dos dados em um formato de rápido acesso, foi utilizada a ferramenta Google Sheets para armazenamento dos dados. Utilizando o conector nativo para o BigQuery, foi possível fazer a ligação entre essas duas ferramentas.

![image](https://github.com/user-attachments/assets/d9715e33-ae3e-4457-a0d0-2ef3a9d671e4)

Assim como uma tabela no BigQuery, é possível olhar detalhadamente os dados pelo Google Sheets.

![image](https://github.com/user-attachments/assets/d8d626f7-eb57-4be4-bf33-ee8035239cc0)


## Melhorias para o Futuro

Para o futuro deste projeto, é esperado que o notebook utilizado para todo o processo de ETL rode **todos os dias** em horários específicos, utilizando as tecnologias Google Cloud Functions e Google Cloud Scheduler, oferecidos pelo Google. A ideia é que o projeto não faça mais a carga full toda vez que chama a API,
mas que seja utilizado de forma incremental, adicionando os dados do dia atual com os dados antigos para criar uma base sólida de dados históricos.

