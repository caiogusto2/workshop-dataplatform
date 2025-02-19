# Oracle Analytics Cloud



A seguir o passo a passo do laboratório.
### **Recursos e Suporte**:

- **Download do Arquivo para o Laboratório**: [Painel das Embarcações Brasileiras](https://objectstorage.us-ashburn-1.oraclecloud.com/n/idi1o0a010nx/b/Fast_Track/o/Lab%20Analytics%20-%20Embarca%C3%A7%C3%B5es%20Brasil.dva)
- **Documentação da Oracle Cloud**: [Getting started with Oracle Analytics Cloud](https://docs.oracle.com/en/cloud/paas/analytics-cloud/index.html)
- **Tutoriais**: [Oracle Analytics Cloud - Explore Funcionalidades com Tutoriais](https://docs.oracle.com/en/cloud/paas/analytics-cloud/tutorials.html)

### _**Aproveite sua experiência na Oracle Cloud!**_



## 📌 Introdução

>**O Oracle Analytics Cloud - OAC aprimora as análises dos seus dados através das funcionalidades estatísticas e IA, com sugestões de visualizações que enriquecem o seu Painel Analítico.** 

O OAC complementa a plataforma de Self-Service Analytics e conta com o motor de Business Intelligence proveniente do OBIEE, que permite construção de Modelos Dimensionais, Hierarquias e outras estruturas que otimizam o consumo dos dados, em Relatórios e Dashboards.

A filosofia da solução gira ao redor dos conceitos de Augmented Analytics, tema muito recorrente em análises de companhias especializadas, como o Gartner. Ele se resume em enriquecer as análises com conceitos de Inteligência Artificial e Machine Learning, dando acesso a abordagens estatísticas avançadas para qualquer perfil de usuário e não apenas aos que possuem vasto conhecimento sobre o tema. Nossa solução é classificada como **‘Líder’ no Quadrante Mágico do Gartner**, principal referência para avaliação e comparação de tecnologias hoje em dia.

<br>
### **Objetivos**

O objetivo deste workshop é demonstrar de forma prática como utilizar a ferramenta do Oracle Analytics Cloud e algumas funcionalidade de AI&ML embarcadas no OAC. Durante o workshop, você aprenderá a criar visualizações, adicionar estatísticas nas análises em um cenário que dados abertos da Marinha Brasileira (https://dados.gov.br/dados/conjuntos-dados/embarcacoes).

### _**Aproveite sua experiência na Oracle Cloud!**_


## 1️⃣ Criação do Oracle Analytics Cloud

Clique no menu de hambúrger do canto superior esquerdo da tela, na sequência navegue até a página de gestão do Oracle Analytics Cloud.

![Analytics Cloud Acess](images/AcessoAnalytics.png)

Na página de gestão do Oracle Analytics Cloud, clique em **Create Instance**.
  
![Create Analytics Cloud](images/CreateOAC.png)

Dê um nome a instância do Analytics Cloud e mantenha as outras configurações como na imagem a seguir. Ao finalizar clique em **Create**:

![Configurando Analytics Cloud](images/CreateOAC1.png)
![Configurando Analytics Cloud](images/CreateOAC2.png)

Aguarde até a conclusão da criação: 
- Ícone amarelo = criando; 
- Ícone verde = pronto para uso;
![Green OAC](images/CreateOAC3.png)

Após a criação do OAC, você está pronto para prosseguir para o próximo laboratório.

## 2️⃣ Acessar OAC 

1. Clique no menu de hambúrger do canto superior esquerdo da tela, na sequência navegue até a página de gestão do Oracle Analytics Cloud.
   ![Analytics Cloud Acess](images/AcessoOAC.png)

2. Selecione a instância do OAC criada anteriormente. Verifique se a Região e o Compartimento estão certos.
   ![Analytics Cloud Homepage](images/OCI-Analytics.png)

3. Agora abra o ambiente clique no botão **Analytics Homepage**.
   ![Analytics Cloud Homepage](images/AcessoOAC1.png)
   ![Analytics Cloud Homepage](images/AcessoOAC2.png)


<br>

4. Após o acesso da Homepage do OAC, **faça o download do arquivo .dva** com o material do laboratório: [Painel das Embarcações Brasileiras](https://objectstorage.us-ashburn-1.oraclecloud.com/n/idi1o0a010nx/b/Fast_Track/o/Lab%20Analytics%20-%20Embarca%C3%A7%C3%B5es%20Brasil.dva)

5. Em seguida, clique nos 3 pontos, ao lado do ícone do perfil. Selecione **'Import Workbook'**.
   ![Import do Arquivo](images/Import1.png)

6. Escolha o arquivo .dva que acabou de baixar no passo 3, para importar.
   ![Import do Arquivo](images/Import2.png)
   ![Import do Arquivo](images/Import3.png)

7. Altere o idioma da ferramenta, clique no Ícone do Perfil, selecione Perfil. Depois só escolher o Idioma e a Configuração Regional do OAC para o desejado. 
   ![Perfil - Idioma](images/Perfil.png)
   ![Perfil - Idioma](images/Perfil2.png)

8. Após a importação você terá acesso ao dataset (Conjunto de Dados) **Brasil-Embarcações** e ao workbook **Lab Analytics - Embarcações Brasil**, se quiser dar uma olhada como o painel vai ficar no final só entrar nele. 
   ![Homepage depois do import](images/Import4.png)

## 3️⃣ Criação do Dashboard - Visualizações

1. Na Homepage do OAC, selecione o Menu de Hamburguer (Canto Superior Esquerdo), Selecione Catálogo >> Minhas Pastas e selecione **Lab Analytics - Embarcações Brasil** (Workbook).
![Homepage - Catálogo](images/Homepage_Catalog.png)

2. Irá abrir o workbook (Pasta de Trabalho), uma tela com os paineis de dados com informações das Embarcações da Marinha Brasileira. No canto superior direito tem um lápis para abrir a aba de edição do Painel de Dados.  
![Editar Workbook](images/EditarWorkbook.png)

3. Na aba **Visualizar**, você encontra os gráficos para serem editados com os DADOS disponíveis na primeira coluna (VERMELHO), a Gramática do Gráfico na segunda coluna - primeiro ícone (VERDE) e as Propriedades do gráfico, na segunda coluna - segundo ícone (AZUL). E a possibilidade de criar outros gráficos.
![Aba Visualizar](images/TelaGeral.png)

4. Adiocione mais uma tela. No canto inferior em um símbolo de '+', ao lado da abas 'Geral', 'Ano a Ano', 'Autoinsight'. Clique nele para adicionar uma Tela.
   ![Tela 4](images/Tela2.png) 

<br>

### **Estatística - Previsão (Forecast)**
5. Segure **_CTRL+Clique_** nos campos **ANO e QUANTIDADE** na primeira coluna no primeiro ícone (Dados), e arraste os dois para a tela em branco. 
   ![Previsão](images/Previsao0.png)

6. Verifique que é um gráfico de **Linha**.
   ![Previsão](images/Previsao.png) 

7. Selecione a visualização que deseja adiconar estatística. Clique com o _botão direito_, selecione **Adionar Estatísticas**. E escolha a opção **Previsão**.
   ![Previsão](images/Previsao2.png) 
   ![Previsão](images/Previsao3.png) 
   ![Previsão](images/Previsao4.png) 

8. Alterar o tipo de Visualização, selecione na Gramática do Gráfico (Segunda Coluna) o ícone to tipo de gráfico, irá expandir e mostrar todas as possibilidades de visualização com os dados que estão disponíveis no gráfico. Observação: Para ver todos os tipos de visualizações  
   ![Alterar Visualização](images/AlterarViz.png) 
   ![Alterar Visualização](images/AlterarViz2.png) 

9.  (OPCIONAL) Personalizar a propriedade do gráfico, deixando o valor e o ponto no gráfico visível. 
   ![Propriedades da Visualização](images/Prop1.png) 
   ![Propriedades da Visualização](images/Prop2.png) 


## 4️⃣ One-Click Explain & Autoinsights
1. Adicione mais uma tela. No canto inferior tem um símbolo de '+', ao lado da aba 'Tela 1' ou 'Geral'. Clique nele para adicionar uma segunda Tela.
   ![Tela 2](images/Tela2.png) 

2. Renomeie o nome da Tela 2 para **'Autoinsights'**, Abas no canto inferior do Painel. 

3. Selecione a **Lâmpada Laranja** no canto superior direito próximo ao Ícone do Perfil. Irá abrir uma aba com várias sugestões de gráficos e visualizações.
   ![Autoinsights](images/Autoinsight1.png)
   ![Autoinsights](images/Autoinsight2.png)  

4. Aproveite para selecionar algumas visualizações, como o Mapa **Geographical Density of Records**, clique no símbolo '+' ou arraste e solte na tela do Painel. 
   ![Autoinsights](images/Autoinsight3.png) 

5. Repita a mesma ação para o gráfico de Barras **Top 10 SIGLA by QUANTIDADE**.
   ![Autoinsights](images/Autoinsight4.png) 

6. Sua tela de Autoinsights ficará dessa forma: 
   ![Autoinsights](images/Autoinsight5.png) 

7. Agora, a funcionalidade que explica seu dado. Clique com o _botão direito_ em cima do dado **ESTADO**. E selecione **'Explicar ESTADO'**
   ![One-Click Explain](images/Explain10.png)

8. Vai abrir um pop up, com __Fatos Básicos, Drivers Chave, Segmentos e Anomalias__ sobre o dado selecionado.
   ![One-Click Explain](images/Explain11.png)

9. Navegue pelas Abas do Explain para ver quais visualizações e quais tipos de análises são disponibilizadas com 1-Clique.
   ![One-Click Explain](images/Explain12.png)
   ![One-Click Explain](images/Explain13.png)
   ![One-Click Explain](images/Explain14.png)

10. Repita para o dado **EMBARCAÇÃO**, clique com o _botão direito_ em cima do dado **EMBARCAÇÃO**. E selecione **'Explicar EMBARCAÇÃO'**
   ![One-Click Explain](images/Explain1.png)
   ![One-Click Explain](images/Explain2.png)
   ![One-Click Explain](images/Explain3.png)
   ![One-Click Explain](images/Explain4.png)

11. Adicione algumas das visualizações disponibilizadas no seu Painel, selecione o **'Check Verde'**, no canto superior das visualizações. 
   ![One-Click Explain](images/Explain5.png)
   ![One-Click Explain](images/Explain6.png)

12. Agora selecione o botão **'Adicionar Selecionado'** e veja as visualizações disponíveis no seu Painel.
   ![One-Click Explain](images/Explain7.png)

13. Salve seu trabalho. Selecione o ícone do disquete no campo direito superior. E volte a Homepage.
![Salvar Painel](images/Save2.png)
![Voltar a Homepage](images/Save3.png)

14. Navegue pelo Painel pronto que foi importado na primeira tarefa. O Workbook (Pasta de Trabalho) **Lab Analytics - Embarcações Brasil**. 
   ![Homepage depois do import](images/Import4.png)

<br>

🎉🎉 Parabéns, você terminou os Laboratórios de **OAC - Oracle Analytics Cloud com sucesso!!** 🎉🎉

<br>




## 👥 Agradecimentos

- **Autores** - Gabriela Miyazima
- **Autor Contribuinte** - Caio Oliveira, Isabelle Anjos
- **Última Atualização Por/Data** - Janeiro 2025

## 🛡️ Declaração de Porto Seguro (Safe Harbor)

O texto a seguir tem como objetivo traçar a orientação dos nossos produtos em geral. É destinado somente a fins informativos e não pode ser incorporado a um contrato. Ele não representa um compromisso de entrega de qualquer tipo de material, código ou funcionalidade e não deve ser considerado em decisões de compra. O desenvolvimento, a liberação, a data de disponibilidade e a precificação de quaisquer funcionalidades ou recursos descritos para produtos da Oracle estão sujeitos a mudanças e são de critério exclusivo da Oracle Corporation.

Esta é a tradução de uma apresentação em inglês preparada para a sede da Oracle nos Estados Unidos. A tradução é realizada como cortesia e não está isenta de erros. Os recursos e funcionalidades podem não estar disponíveis em todos os países e idiomas. Caso tenha dúvidas, entre em contato com o representante de vendas da Oracle. 
