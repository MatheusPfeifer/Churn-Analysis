# Análise de Churn

## 1. Contexto
Um problema que acontece em praticamente todas as empresas que oferecem algum tipo de serviço é o Churn. O Churn é caracterizado basicamente quando um cliente que utilizava algum serviço para de utilizar. Por exemplo, um determinado cliente de uma operadora de telefone cancela o seu chip, é dito que esse cliente está Churn.

Pensando nessa maneira, toda empresa quer evitar de perder clientes, porque se uma empresa tiver uma alta taxa de Churn pode acontecer das receitas cairem drasticamente, além do mais é mais caro tentar adquirir novos clientes, através de campanhas de marketing por exemplo, do que manter os clientes antigos.

Uma das possível soluções é poder prever quais clientes podem ser classificados como candidatos a entrarem em Churn, a partir de dados de clientes antigos. Então, aí que entra o Machine Learning, utilizando aprendizado de máquina podemos treinar um algoritmo para identificar padrões em clientes que pararam de utilizar ou não os serviços, e aplicar para novos clientes. Com as previsões dos clientes que foram classificados como Churn, pode-se tentar oferecer algum tipo de benefício que fará com que o cliente não deixe de utilizar o serviço.

Trazendo para o contexto do nosso problema de negócio, estamos tentando predizer quais clientes poderão parar de utilizar o serviço de uma instituição bancária e oferecer alguma solução de negócio.

Os dados para o problema são encontrado em: https://www.kaggle.com/mervetorkan/churndataset

## 2. Plano de Solução
Como foi dito anteriormente, iremos utilizar Machine Learning para identificar os clientes que poderão entrar em Churn baseado em dados históricos. O processo de análise foi divido em:
- Entendimento do problema de negócio
- Obtenção dos dados
- Limpeza dos dados
- Análise exploratória dos dados
- Pré-processamento dos dados
- Criação do modelo
- Avaliação do modelo
- Entrega da solução

Para a entrega da solução, será fornecida uma tabela com os clientes que foram classificados como Churn, para a equipe de negócio poder avaliar qual a melhor solução a ser tomada para reter o cliente no banco.

## 3. Insights sobre os dados
Na pasta de script, será encontrado o arquivo com toda a análise dos dados e as principais conclusões tiradas. Aqui terá um resumo dos principais pontos sobre o perfil dos clientes que entraram em Churn:
- 20% dos clientes entraram em Churn;
- As mulheres tem maior taxa de Churn do que os homens. A taxa de Churn das mulheres chegam a 25%, enquanto dos homens é de 16,5%;
- Clientes com muitos produtos no banco tem maior taxa de Churn. Por exemplo, clientes que possuem 4 produtos taxa de Churn de 100% e com 3 produtos 83%, enquanto clientes com 1 produto tem Churn de 27,7% e com 2 produtos 7,6%;
- A Alemanha é o país que apresenta maior Churn, com mais de 30%, mesmo sendo o país com a menor quantidade de clientes;
- Clientes entre 50 e 60 anos são os que apresentam maior taxa de Churn, podendo chegar a mais de 50%;
- Cleintes com baixo score de créditos tem maior propensão a entrar em Churn.

## 4. Modelo de Machine Learning
Para avaliar o melhor modelo de Machine Learning, começei construindo um modelo simples de Regressão Logística para servir como baseline, e posteriormente fui fazendo alterações no conjunto de dados e verificando as melhoras ou pioras do modelo.

Já no primeiro modelo, foi verificado um problema comum em modelos de ML, é o desbalanceamento de classes, em que o modelo aprende muito bem uma classe com muitos dados (no caso pessoas que não entraram em churn - 80%) mas não apredende a classe com menos dados (pessoas que entraram em churn - 20%). Sendo assim, modelo acertou apenas 5% dos casos de Churn mas acertou 97% nos casos que não houve Churn, ficando com uma acurácia total de 79%. Então, após o primeiro modelo, realizei alguns pré-processamento nos dados que foram:
- OverSampling (SMOTE) - cria dados sintéticos utilizando estatística para balancear classe que tem menos dados
- Normalização dos dados - ajuda o treinamento do modelo

Estas duas alterações, fizeram com que o modelo (ainda regressão logística), fosse de 5% de acerto para 69% de acerto nos casos de churn, evidenciando a importância no tratamento dos dados em Machine Learning. Porém, a acurácia caiu um pouco de 79% para 72%, ou seja, o modelo está classificando alguns casos que não são de churn como sendo, mas isso não é problema visto que o nosso objetivo é identificar clientes em churn.

Seguindo o processo de criação de modelos, criei um modelo de RandomForest e XGBoost para fazer a classificação. Usei a biblioteca Pycaret, para construir vários modelo de Machine Learning automaticamente (AutoML), e abaixo está um resumo dos modelos que apresentaram melhor performance. A métrica que foi levada em conta para escolha do melhor modelo foi o Recall para casos com Churn, justamente para avaliar o comportamento dos casos de Churn que o modelo acertou.

   ![image](https://user-images.githubusercontent.com/66805980/130838843-b1f2cd3b-aeb8-42a5-b365-1b0a5a7c4aab.png)

Então, seguindo a lógica proposta em que melhor modelo é o que consegue prever melhor o churn (mesmo se tiver uma acurácia geral menor), o modelo de Linear Discrimant Analysis (LDA) teve o melhor resultado. Abaixo temos o comparativo dos acertos e erros do modelo.

   ![image](https://user-images.githubusercontent.com/66805980/130839170-f8ad57aa-c560-4640-8b67-c9330c9c8070.png)

Mesmo que tenhamos um alto número de clientes que não entrariam em Churn como sendo classificados em Churn, não teremos um impacto tão grande quanto classificar pessoas em Churn errado.

## 5. Resultado de negócio
Por último, será avaliado quanto que o modelo geraria de valor ao negócio. Supondo que um cliente gaste em média 10% do seu salário com no banco, seja gastos com cartão de crédito, tarifas de manutenção de conta, empréstimos bancários, seguros de cartão e vida, taxa de administração sobre investimento, anuidade de cartão, enfim. Segundo o nosso conjunto de dados, temos no total 2037 clientes que entraram em churn, o modelo conseguiu prever corretamente 1541 ocorrências de churn.

Esses 1541 clientes tem uma renda anual de $105,816,985.00 de dólares. Desta forma, **o banco poderia teria evitado de perder $10,581,698.50 por ano, ou seja, mais de 10 milhões de dólares o banco poderia ter de receita se fosse utilizado um modelo de Machine Learning para prever clientes em churn**. Isso considerando apenas 10%, sabemos que esse gasto é muito maior, principalmente se considerarmos gastos com cartão de crédito. Portanto, podemos considerar que o modelo encontrado conseguiu atender bem as demandas do banco, mas que claro conforme for surgindo mais dados podemos aprimorar ainda mais o modelo.

## 6. Conclusão
Sendo assim, com o final deste projeto, foi possível observar que uma empresa que aplica modelos de Machine Learning para classificar clientes em churn pode ser mais bem sucedida no mercado.
A respeito da solução temos diversos algoritmos de ML no mercado e passar por todos eles dentro de um projeto é praticamente impossível, para isso foi utilizado a biblioteca do Pycaret que aborda 18 algoritmos de classificação e obter o melhor para a nossa análise. Também foi possível observar o quanto um conjunto de dados com classes desbalanceadas pode impactar negativamente o modelo. Porém, no final foi possível entregar uma boa solução ao cliente.

Para próximos passos, poderia ser feito o deploy do modelo em nuvem ou uma solução utilizando streamlit ou flask por exemplo.
