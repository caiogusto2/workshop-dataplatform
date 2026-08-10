# Oracle 26ai Autonomous Database

## 🎯 **Objetivos**

Demonstrar de forma prática como utilizar algumas das funcionalidades do Oracle Autonomous Database com o objetivo de atender workloads de data warehouse

O que você aprenderá:

- Criar e configurar novo schemas e usuários no Oracle Autonomous DB
- Realizar e orquestrar transformações utilizando procedures e oracle scheduler
- Importar e configurar aplicação Select AI
- Configurar e testar data redaction
- Configurar ORDS para consultas sobre datasets 

>### ⚠️ **ATENÇÃO**: Recomendamos que antes de iniciar esse workshop, conclua o hands on de AI Data Platform

### _**Aproveite sua experiência na Oracle Cloud!**_

## 📌 Introdução

>**Este hands-on apresenta, de forma prática, como o Oracle Autonomous Database 26ai pode complementar a AI Data Platform, oferecendo recursos para atender workloads de Data Warehouse de maneira integrada, segura e automatizada. Ao longo do workshop, você aprenderá a criar e configurar schemas e usuários, realizar e orquestrar transformações de dados com procedures e Oracle Scheduler, importar e configurar aplicações com Select AI, implementar Data Redaction para proteção de informações sensíveis e configurar o ORDS para disponibilizar consultas sobre datasets. A proposta é proporcionar uma experiência prática com funcionalidades essenciais do Autonomous Database dentro da Oracle Cloud.** 

# **Parte 2 - Hands On Oracle Autonomous Database**

## **1️⃣ Criar de schema e preparação de ambiente**

Utilize a instância do Oracle Autonomous criado no Hands On do OCI AI dataplatform

![Link AIDP](images/link_adb.png)

Clique em database users

![adb_instance](images/adb_instance.png)

Clique em + Criar usuário, e crie o usuário AI, dê os grants conforme o print screen abaixo

> **⚠️ ATENÇÃO:** A sugestão é utilizar a senha **WORKSHOPsec2019##**, contudo você pode escolher um outra senha se assim desejar. O restante das configurações vocês podem utilizar o padrão e clicar no botão create

![adb_user](images/adb_user.png)

## **2️⃣ Configurar e orquestrar transformações utilizando procedures e oracle scheduler**

Usando o menu do canto superior esquerdo clique em SQL

![sql_dev_link](images/sql_dev_link.png)

Caso tenha concluido com sucesso o laboratório do AI Data platform, você deve conseguir visualizar os dois datasets replicados CUSTOMER_ORDERS e CUSTOMER_CLASS_AGG_REVIEW

![sql_dev_01](images/sql_dev_01.png)

Clique e arraste o dataset CUSTOMER_ORDERS para o meio da tela, selecione a caixa "Selecionar" e clique em aplicar

![sql_dev_02](images/sql_dev_02.png)

Altere o grupo de consumidor para medium

![sql_dev_03](images/sql_dev_03.png)

Clique no meio da tela e aperte ctrl + enter para executar a query

![sql_dev_04](images/sql_dev_04.png)

Clique com o botão direito no dataset e escolha abrir

![sql_dev_05](images/sql_dev_05.png)

Navegue, consulte os metadados do objeto e clique em fechar

![sql_dev_06](images/sql_dev_06.png)

De volta ao worksheet, conceda alguns grants adicionais ao usuário AI; copie, cole e execute o bloco abaixo apertando f5

``` sql
GRANT dwrole TO ai;
GRANT unlimited tablespace TO ai;
GRANT READ, WRITE ON DIRECTORY data_pump_dir TO ai;
GRANT EXECUTE ON DBMS_CLOUD TO ai;
GRANT CREATE PROPERTY GRAPH TO ai;
GRANT EXECUTE ON DBMS_LOCK TO ai;
GRANT EXECUTE ON DBMS_CLOUD_AI TO ai;
GRANT EXECUTE ON SYS.DBMS_REDACT TO AI;
GRANT ADMINISTER REDACTION POLICY TO AI;

BEGIN
  DBMS_NETWORK_ACL_ADMIN.APPEND_HOST_ACE(
    host => '*',
    ace => xs$ace_type(privilege_list => xs$name_list('connect'),
                       principal_name => 'ai',
                       principal_type => xs_acl.ptype_db));
END;
/ 
```

Agora crie e teste a procedure de replicação de datasets. Nosso objetivo é replicar as tabelas CUSTOMERS_ORDERS e CUSTOMER_CLASS_AGG_REVIEW do schema ADMIN para o schema AI

Crie e teste a procedure de replicação de datasets

``` sql
CREATE OR REPLACE PROCEDURE ADMIN.REFRESH_AI_TABLES
AS
BEGIN
    -- Drop AI.CUSTOMER_ORDERS if it exists
    BEGIN
        EXECUTE IMMEDIATE 'DROP TABLE AI.CUSTOMERS_ORDERS PURGE';
    EXCEPTION
        WHEN OTHERS THEN
            IF SQLCODE != -942 THEN
                RAISE;
            END IF;
    END;

    -- Recreate AI.CUSTOMER_ORDERS
    EXECUTE IMMEDIATE '
        CREATE TABLE AI.CUSTOMERS_ORDERS AS
        SELECT *
        FROM ADMIN.CUSTOMERS_ORDERS
    ';

    -- Drop AI.CUSTOMER_CLASS_AGG_REVIEW if it exists
    BEGIN
        EXECUTE IMMEDIATE 'DROP TABLE AI.CUSTOMER_CLASS_AGG_REVIEW PURGE';
    EXCEPTION
        WHEN OTHERS THEN
            IF SQLCODE != -942 THEN
                RAISE;
            END IF;
    END;

    -- Recreate AI.CUSTOMER_CLASS_AGG_REVIEW
    EXECUTE IMMEDIATE '
        CREATE TABLE AI.CUSTOMER_CLASS_AGG_REVIEW AS
        SELECT *
        FROM ADMIN.CUSTOMER_CLASS_AGG_REVIEW
    ';

END REFRESH_AI_TABLES;
/
```

``` sql
BEGIN
    ADMIN.REFRESH_AI_TABLES;
END;
/
```
![sql_dev_07](images/sql_dev_07.png)

Agora clique no ícone do canto superior esquerdo e em progamando

![scheduler_link](images/scheduler_link.png)

Clique em + Criar job, dê o nome de AI_Refresh, selecione a classe sys.medium e cole o bloco abaixo

``` sql
BEGIN
    ADMIN.REFRESH_AI_TABLES;
END;
```
![scheduler_01](images/scheduler_01.png)

Na aba modo de execução configure para executar de hora em hora a procedure e clique em criar

![scheduler_02](images/scheduler_02.png)

Execute o seu job

![scheduler_03](images/scheduler_03.png)

Após a conclusão clique na aba de histórico e relatório para ter a visão completa das execuções e também potenciais erros

![scheduler_04](images/scheduler_04.png)

## **3️⃣ Importar e configurar aplicação Select AI**

Clique no icone do canto superior esquerdo e selecione APEX, faça o login novamente com o usuário ADMIN e a senha configurada na criação do Autonomous Database

![apex_link](images/apex_link.png)

Clique no icone create workspace no canto direito da tela e selecione existing schema

![apex01](images/apex01.png)

Selecione o schema AI e coloque a senha do ambiente, recomendamos a senha conforme o print screen; Clique em create workspace

![apex02](images/apex02.png)

Na parte inferior esquerda da tela, faça o logoff do ambiente

![apex03](images/apex03.png)

Na tela do login, faça o login com o usuario AI e senha configurada na etapa anterior.

Na sequência clique em SQL Workshop e SQL Commands

![apex04](images/apex04.png)

Copie e cole o código abaixo (observação: vamos ter que fazer algumas alterações na sessão de create_credential conforme os ids e configurações do seu ambiente)

``` sql
BEGIN
   DBMS_CLOUD.CREATE_CREDENTIAL (
       credential_name => 'OBJ_STORE_CRED',
       user_ocid       => '<USER_OCID>',
       tenancy_ocid    => '<TENANCY_OCID>',
       private_key     => '<PRIVATE_KEY_SEM_CABEÇALHO_RODAPÉ>',
       fingerprint     => '<FINGERPRINT_CHAVE>');
END;
/

begin
   dbms_cloud_ai.create_profile(
    profile_name => 'OCI_GENAI',
    attributes   => '{"provider": "oci",
        "model":"meta.llama-3.3-70b-instruct" ,
        "credential_name": "OBJ_STORE_CRED",
        "object_list": [
            {"owner": "AI"}
            ],
        "region": "sa-saopaulo-1",
        "comments":"true"
    }'
   );
end;
/   
```

As informações referentes ao dbms_cloud.create_credential podem ser coletadas a aba de configurações do seu usuário OCI

![oci01](images/oci01.png)

Na aba de token and keys, crie uma API Key, faça o download da chave privada e clique no canto direito em view configuration file

![oci02](images/oci02.png)

Abaixo o exemplo de preenchimento do código que deve ser executado dentro do apex

![apex05](images/apex05.png)

Agora vamos adicionar comentários à tabela CUSTOMERS_ORDERS para facilitar o uso do Select AI. No APEX clique em sql scripts

![apex07](images/apex07.png)

Clique em create e cole o codigo abaixo. Dê o nome de comentarios_tabela e após isso, clique em run

``` sql
COMMENT ON TABLE AI.CUSTOMERS_ORDERS IS
'Tabela de pedidos e perfil de clientes usada para análises, segmentação e geração de consultas em linguagem natural no Select AI.';

COMMENT ON COLUMN AI.CUSTOMERS_ORDERS.CUSTOMER_ID IS
'Identificador único do cliente.';

COMMENT ON COLUMN AI.CUSTOMERS_ORDERS.ORDER_ID IS
'Identificador único do pedido.';

COMMENT ON COLUMN AI.CUSTOMERS_ORDERS.ORDER_DATE IS
'Data do pedido armazenada como texto.';

COMMENT ON COLUMN AI.CUSTOMERS_ORDERS.ORDER_MODE IS
'Canal ou modo de realização do pedido, como online, loja ou outro canal.';

COMMENT ON COLUMN AI.CUSTOMERS_ORDERS.ORDER_STATUS IS
'Status numérico do pedido.';

COMMENT ON COLUMN AI.CUSTOMERS_ORDERS.ORDER_TOTAL IS
'Valor total do pedido.';

COMMENT ON COLUMN AI.CUSTOMERS_ORDERS.SALES_REP_ID IS
'Identificador do representante de vendas responsável pelo pedido.';

COMMENT ON COLUMN AI.CUSTOMERS_ORDERS.PROMOTION_ID IS
'Identificador da promoção aplicada ao pedido.';

COMMENT ON COLUMN AI.CUSTOMERS_ORDERS.WAREHOUSE_ID IS
'Identificador do armazém ou centro de distribuição responsável pelo pedido.';

COMMENT ON COLUMN AI.CUSTOMERS_ORDERS.DELIVERY_TYPE IS
'Tipo de entrega selecionado para o pedido.';

COMMENT ON COLUMN AI.CUSTOMERS_ORDERS.COST_OF_DELIVERY IS
'Custo de entrega do pedido.';

COMMENT ON COLUMN AI.CUSTOMERS_ORDERS.WAIT_TILL_ALL_AVAILABLE IS
'Indica se o pedido aguarda todos os itens ficarem disponíveis antes do envio.';

COMMENT ON COLUMN AI.CUSTOMERS_ORDERS.DELIVERY_ADDRESS_ID IS
'Identificador do endereço de entrega.';

COMMENT ON COLUMN AI.CUSTOMERS_ORDERS.ORDER_CUSTOMER_CLASS IS
'Classe ou segmento do cliente no contexto do pedido.';

COMMENT ON COLUMN AI.CUSTOMERS_ORDERS.CARD_ID IS
'Identificador do cartão utilizado no pedido.';

COMMENT ON COLUMN AI.CUSTOMERS_ORDERS.INVOICE_ADDRESS_ID IS
'Identificador do endereço de cobrança ou faturamento.';

COMMENT ON COLUMN AI.CUSTOMERS_ORDERS.CUST_FIRST_NAME IS
'Primeiro nome do cliente.';

COMMENT ON COLUMN AI.CUSTOMERS_ORDERS.CUST_LAST_NAME IS
'Sobrenome do cliente.';

COMMENT ON COLUMN AI.CUSTOMERS_ORDERS.NLS_LANGUAGE IS
'Idioma preferencial do cliente.';

COMMENT ON COLUMN AI.CUSTOMERS_ORDERS.NLS_TERRITORY IS
'Território ou região preferencial do cliente.';

COMMENT ON COLUMN AI.CUSTOMERS_ORDERS.CREDIT_LIMIT IS
'Limite de crédito do cliente.';

COMMENT ON COLUMN AI.CUSTOMERS_ORDERS.CUST_EMAIL IS
'Endereço de e-mail do cliente.';

COMMENT ON COLUMN AI.CUSTOMERS_ORDERS.ACCOUNT_MGR_ID IS
'Identificador do gerente de conta responsável pelo cliente.';

COMMENT ON COLUMN AI.CUSTOMERS_ORDERS.CUSTOMER_SINCE IS
'Data desde quando o cliente está cadastrado, armazenada como texto.';

COMMENT ON COLUMN AI.CUSTOMERS_ORDERS.CUSTOMER_CLASS IS
'Classe ou segmento principal do cliente.';

COMMENT ON COLUMN AI.CUSTOMERS_ORDERS.SUGGESTIONS IS
'Sugestões ou recomendações associadas ao cliente.';

COMMENT ON COLUMN AI.CUSTOMERS_ORDERS.DOB IS
'Data de nascimento do cliente armazenada como texto.';

COMMENT ON COLUMN AI.CUSTOMERS_ORDERS.MAILSHOT IS
'Indica se o cliente aceita receber campanhas de marketing por e-mail.';

COMMENT ON COLUMN AI.CUSTOMERS_ORDERS.PARTNER_MAILSHOT IS
'Indica se o cliente aceita receber campanhas de parceiros.';

COMMENT ON COLUMN AI.CUSTOMERS_ORDERS.PREFERRED_ADDRESS IS
'Identificador do endereço preferido do cliente.';

COMMENT ON COLUMN AI.CUSTOMERS_ORDERS.PREFERRED_CARD IS
'Identificador do cartão preferido do cliente.';  
```

Concluido o setup vamos clicar em App builder e import

![apex06](images/apex06.png)

Faça o download do arquivo https://raw.githubusercontent.com/caiogusto2/oracleapex/main/SelectAI/selectai.zip e upload para o formulário; Clique next, depois import application, next, install supporting objects e por fim run application

![apex08](images/apex08.png)

O login será AI e a senha WORKSHOPsec2019##

![apex09](images/apex09.png)

Selecione o oci_genai e clique no x

![apex10](images/apex10.png)

Pergunte: Qual a quantidade de ordens por delivery_type

![apex11](images/apex11.png)

Outras perguntas que podem ser feitas
- qual a quantidade de ordens por customer_class
- quais as ordens efetuadas pelo email alfred.foley@yahoo.com
- qual a quantidade de ordens por warehouse_id

## **4️⃣ Configurar e testar ORDS**

De volta ao apex, vamos primeiramente configurar o data redaction para a coluna de email da nossa tabela CUSTOMERS_ORDERS. Clique em SQL Commands

![apex12](images/apex12.png)

Copie e cole o comando abaixo

``` sql
BEGIN
  DBMS_REDACT.ADD_POLICY(
    object_schema  => 'AI',
    object_name    => 'CUSTOMERS_ORDERS',
    policy_name    => 'REDACT_CUST_EMAIL_ORDS',
    column_name    => 'CUST_EMAIL',
    function_type  => DBMS_REDACT.FULL,
    expression     => 'SYS_CONTEXT(''USERENV'',''MODULE'') = ''/v1/consulta'''
  );
END;
/
```

Faça um teste e veja que como estamos logado com o usuário AI temos acesso completo ao dataset

``` sql
select cust_first_name, cust_last_name, cust_email from customers_orders;
```

![apex13](images/apex13.png)

Agora clique na aba de restfull services

![apex14](images/apex14.png)

Clique em Modules > create module, dê o nome de api e base path v1

![apex15](images/apex15.png)

Na sequencia clique em create template e na uri template escreva consulta

![apex16](images/apex16.png)

Na sequencia, crie um handler e no source coloque 

``` sql
select cust_first_name, cust_last_name, cust_email from customers_orders
```

![apex17](images/apex17.png)

Copie e cole a URL no web browser, seus dados aparecerão, contudo o email estará nulo por conta da regra de redaction

![apex18](images/apex18.png)

## **✅ Laboratório finalizado!**

Parabéns! Você concluiu o Hands On do **Oracle Autonomous**, aprendeu a utilizar as tabelas carregadas pelo AI Data platform para orquestrar e realizar outras diferentes transformações usando procedures e scheduler, importou e configurou uma aplicação APEX que demonstra o uso do Select AI e por fim criou um endpoint REST com uma regra de redaction.


## 👥 Agradecimentos

- **Autores** - Caio Oliveira
- **Autores Contribuintes** - Isabelle Anjos
- **Última Atualização Por/Data** - Agosto 2026

## 🛡️ Declaração de Porto Seguro (Safe Harbor)

O tutorial apresentado tem como objetivo traçar a orientação dos nossos produtos em geral. É destinado somente a fins informativos e não pode ser incorporado a um contrato. Ele não representa um compromisso de entrega de qualquer tipo de material, código ou funcionalidade e não deve ser considerado em decisões de compra. O desenvolvimento, a liberação, a data de disponibilidade e a precificação de quaisquer funcionalidades ou recursos descritos para produtos da Oracle estão sujeitos a mudanças e são de critério exclusivo da Oracle Corporation.

Esta é a tradução de uma apresentação em inglês preparada para a sede da Oracle nos Estados Unidos. A tradução é realizada como cortesia e não está isenta de erros. Os recursos e funcionalidades podem não estar disponíveis em todos os países e idiomas. Caso tenha dúvidas, entre em contato com o representante de vendas da Oracle. 
