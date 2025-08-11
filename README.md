# Recomendação de Ofertas para Engajamento do Cliente

## Estrutura do Projeto e Fluxo de Trabalho

O fluxo de trabalho do projeto é organizado em várias etapas sequenciais, cada uma documentada e implementada em seus respectivos notebooks Jupyter.

### Dependências
Este projeto requer as seguintes bibliotecas Python. Você pode instalá-las usando pip:
 - pip install -r requirements.txt


#### 1. Engenharia de Features: Ofertas
- Propósito: Processar o conjunto de dados offers.json para extrair e transformar features relevantes específicas das ofertas.

- Processo: Inclui leitura de arquivos, entendimento dos dados e limpeza completa, envolvendo a transformação de colunas de canais, padronização de colunas relacionadas a ofertas e codificação do tipo de oferta.

#### 2. Engenharia de Features: Perfis de Clientes
- Propósito: Lidar com o conjunto de dados profile.json, com foco na extração e preparação de features demográficas e de relacionamento dos clientes.

- Considerações e Processo: Uma atenção especial é dada aos valores nulos em gender e credit_card_limit, que estão todos associados a age=118. O processo envolve formatar as datas de registro, distribuir aleatoriamente idades e rendas nulas para preservar a distribuição geral, e codificar gêneros não identificados em uma classe 'U'. As etapas de limpeza de dados incluem ajuste da coluna de gênero, criação de variáveis dummy para gêneros e geração e codificação de grupos de idade.

#### 3. Engenharia de Features: Transações
 - Propósito: Processar o conjunto de dados transactions.json, segmentando e preparando eventos relacionados a transações e ofertas.

 - Processo: As principais etapas envolvem unificar as colunas offer_id e transformá-las, resultando em tabelas limpas para cada tipo de evento

 #### 4. Geração da Mastertable e Análise Inicial 
  - Propósito: Este notebook combina todos os conjuntos de dados processados anteriormente em uma única e abrangente mastertable para análise e modelagem integradas. Ele também realiza análises exploratórias iniciais para derivar insights importantes.

  - Análise Realizada e Considerações: A análise inclui o exame da taxa de conversão de ofertas por faixa etária e tipo de oferta, ofertas concluídas por tipo de oferta, ofertas concluídas por faixa etária e renda, ofertas concluídas por gênero e ofertas concluídas por renda. As considerações e insights importantes revelados são:
    - Descontos são mais procurados que ofertas rápidas, sendo eficazes em clientes entre 40 e 60 anos.
    - Ofertas informativas não tiveram nenhuma conclusão, indicando que não vale a pena mantê-las na forma atual.
    - Clientes mais velhos e mais ricos procuram mais ofertas do que pessoas mais jovens.
    - Uma sugestão chave é melhorar as ofertas para pessoas mais jovens e de baixa renda.
    - Faixa Etária, Renda e Tipo da Oferta são features que causam mudança significativa.

#### 5. Modelagem
 - Propósito: Esta etapa concentra-se na construção dos modelos centrais para o sistema de recomendação, incluindo um modelo supervisionado para propensão a ofertas e agrupamento de clientes

 - Um modelo supervisionado é treinado para prever a propensão de compra de um tipo de oferta, $P(offerType)$, assim como a propensão de compra de uma oferta condicionada ao seu tipo, $P(offer | offerType)$.
    - Isso é alcançado usando o Teorema de Bayes: $P(offer) = P(offer | offerType) \times P(offerType)$, onde $P(offer | offerType)$ é uma solução frequentista e $P(offerType)$ é determinada pelo modelo supervisionado.
    - Um RandomForestModel é usado para esta tarefa supervisionada.
    - A otimização de Falsos Negativos é considerada, buscando um melhor ponto de corte (threshold), uma decisão que depende diretamente da estratégia de negócio e avalia se vale a pena enviar mais ofertas de baixa probabilidade para capturar mais clientes de alta probabilidade.

- Agrupamento de Clientes:
    - Um KMeansModel é criado para agrupar clientes com base em suas informações demográficas.

#### 6. Geração de Recomendações
 - Propósito: A etapa final envolve o uso dos modelos para gerar e compilar recomendações de ofertas personalizadas para os clientes

 - Processo: Este notebook gera scores de propensão e recomendações, cria recomendações intra-cluster, identifica as top ofertas mais frequentemente adquiridas pelos clientes por tipo de oferta, e então combina essas diversas fontes de recomendação (scores, intra-cluster e baseadas em popularidade) para formar um pacote de recomendação.