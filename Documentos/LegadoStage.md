## Sumário
+ [Sistema Legado/Staging Area](#LegadoStaging)
    + [Simulando o sistema legado](#Legado)
    + [Criando a Staging Area](#Staging)
    + [Criando as conexões com o banco](#Conexoes)
    + [Transformações](#Transformacoes)
        + [Criando a dimensão cliente](#T-D-Cliente)
        + [Criando a dimensão localidade](#T-D-Localidade)
        + [Criando a dimensão produto](#T-D-Produto)
        + [Criando o fato venda](#T-Fato)

<a name = "LegadoStaging"></a>
## Sistema Legado/Staging Area
[Voltar para o ínicio ↑](#Inicio)

<br>

<a name = "Legado"></a>
### Simulando o sistema legado

Para não precisar criar vários bancos de dados, para atender cada etapa do projeto, utilizarei separação de cada etapa por usuário do banco de dados.

Com o usuário `oracle`, abra o terminal

Para iniciar o Listener, use o comando

```bash
lsnrctl start
```

Entre no banco de dados, use o comando

```bash
sqlplus / as sysdba
```

Dentro do shell SQL, inicie o banco de dados, use o comando

```bash
startup
```

Abra o Oracle SQL Developer e faça a conexão com o usuário `sys`

Com o usuário `sys`, libere as permissões de scripts na seção, rode o seguinte script:

```sql
alter session set "_ORACLE_SCRIPT"=true;
```

Crie uma tablespace `source`, rode o seguinte script:

```sql
CREATE TABLESPACE TBS_SOURCE
LOGGING DATAFILE '/u01/app/oracle/oradata/ORCL21C/SOURCE.dbf' 
SIZE 1M AUTOEXTEND ON NEXT 10M MAXSIZE UNLIMITED EXTENT MANAGEMENT LOCAL SEGMENT SPACE MANAGEMENT AUTO;
```

Verifique se a tablespace foi criada corretamente, rode o seguinte script:

```sql
select * from dba_data_files
```

![Untitled](/Imagens/Untitled%20143.png)

Com a tablespace criada, crie o usuário `source`, rode o seguinte script:

```sql
CREATE USER source IDENTIFIED BY ***<SENHA>***
DEFAULT TABLESPACE TBS_SOURCE
TEMPORARY TABLESPACE TEMP;

GRANT "CONNECT" TO source;
GRANT "RESOURCE" TO source;

GRANT CREATE TABLE TO source;
GRANT UNLIMITED TABLESPACE TO source;
```

Para verificar se o usuário foi criado, rode o seguinte script:

```sql
SELECT * 
FROM all_users 
WHERE USERNAME = 'SOURCE'
```

![Untitled](/Imagens/Untitled%20144.png)

Inicie uma sessão com o usuário `source`

![Untitled](/Imagens/Untitled%20145.png)

Criando o schema do sistema legado

Com o usuário `source`, rode o seguinte script:

```sql
CREATE TABLE TB_CADASTRO_CLIENTE 
(
    ID_CLIENTE INTEGER  NOT NULL 
    , NOME_CLIENTE VARCHAR2(100) 
    , EMAIL_CLIENTE VARCHAR2(50) 
    , CONSTRAINT TB_CADASTRO_CLIENTE_PK PRIMARY KEY 
(
    ID_CLIENTE 
)
ENABLE 
);

CREATE TABLE TB_ENDERECO 
(
    ID_ENDERECO INTEGER NOT NULL 
    , LOGRADOURO VARCHAR2(50) 
    , NUMERO NUMBER 
    , CIDADE VARCHAR2(20) 
    , ESTADO VARCHAR2(20) 
    , PAIS VARCHAR2(20) 
    , CEP VARCHAR2(20) 
    , ID_CLIENTE INTEGER
    , CONSTRAINT TB_ENDERECO_PK PRIMARY KEY 
(
    ID_ENDERECO 
)
ENABLE 
);

ALTER TABLE TB_ENDERECO
ADD CONSTRAINT TB_ENDERECO_FK1 FOREIGN KEY
(
    ID_CLIENTE 
)
REFERENCES TB_CADASTRO_CLIENTE
(
    ID_CLIENTE 
)
ENABLE;

CREATE TABLE TB_PRODUTO 
(
    ID_PRODUTO INTEGER NOT NULL 
    , SKU VARCHAR2(30) 
    , NOME_PRODUTO VARCHAR2(100) 
    , ID_CATEGORIA INTEGER
    , CONSTRAINT TB_PRODUTO_PK PRIMARY KEY 
(
    ID_PRODUTO 
)
ENABLE 
);

CREATE TABLE TB_CATEGORIA 
(
    ID_CATEGORIA INTEGER NOT NULL 
    , NOME_CATEGORIA VARCHAR2(20) 
    , NOME_SUB_CATEGORIA VARCHAR2(20) 
    , CONSTRAINT TB_CATEGORIA_PK PRIMARY KEY 
(
    ID_CATEGORIA 
)
ENABLE 
);

ALTER TABLE TB_PRODUTO
ADD CONSTRAINT TB_PRODUTO_FK1 FOREIGN KEY
(
    ID_CATEGORIA 
)
REFERENCES TB_CATEGORIA
(
    ID_CATEGORIA 
)
ENABLE;

CREATE TABLE TB_LOCALIDADE 
(
    ID_LOCALIDADE INTEGER NOT NULL 
    , NOME_LOCALIDADE VARCHAR2(100) 
    , CIDADE_LOCALIDADE VARCHAR2(20) 
    , CONSTRAINT TB_LOCALIDADE_PK PRIMARY KEY 
(
    ID_LOCALIDADE 
)
ENABLE 
);

CREATE TABLE TB_PEDIDOS 
(
    ID_TRANSACAO INTEGER NOT NULL 
    , DATA_TRANSACAO TIMESTAMP 
    , DATA_ENTREGA TIMESTAMP 
    , STATUS_PAGAMENTO VARCHAR2(20) 
    , ID_CLIENTE INTEGER 
    , ID_LOCALIDADE INTEGER 
    , CONSTRAINT TB_PEDIDOS_PK PRIMARY KEY 
(
    ID_TRANSACAO 
)
ENABLE 
);

ALTER TABLE TB_PEDIDOS
ADD CONSTRAINT TB_PEDIDOS_FK1 FOREIGN KEY
(
    ID_CLIENTE 
)
REFERENCES TB_CADASTRO_CLIENTE
(
    ID_CLIENTE 
)
ENABLE;

ALTER TABLE TB_PEDIDOS
ADD CONSTRAINT TB_PEDIDOS_FK2 FOREIGN KEY
(
    ID_LOCALIDADE 
)
REFERENCES TB_LOCALIDADE
(
    ID_LOCALIDADE 
)
ENABLE;

CREATE TABLE TB_ITENS_PEDIDO 
(
    ID_TRANSACAO INTEGER NOT NULL 
    , ID_PRODUTO INTEGER NOT NULL 
    , QUANTIDADE INTEGER 
    , PRECO_UNITARIO DOUBLE PRECISION 
    , CONSTRAINT TB_ITENS_PEDIDO_PK PRIMARY KEY 
    (
    ID_TRANSACAO 
    , ID_PRODUTO 
)
ENABLE 
);

ALTER TABLE TB_ITENS_PEDIDO
ADD CONSTRAINT TB_ITENS_PEDIDO_FK1 FOREIGN KEY
(
    ID_PRODUTO 
)
REFERENCES TB_PRODUTO
(
    ID_PRODUTO 
)
ENABLE;
```

Para inserir os registros, rode o seguinte script:

```sql
TRUNCATE TABLE TB_CADASTRO_CLIENTE;
INSERT INTO "SOURCE"."TB_CADASTRO_CLIENTE" (ID_CLIENTE, NOME_CLIENTE, EMAIL_CLIENTE) VALUES ('1098', 'Pele', 'pele@gmail.com');
INSERT INTO "SOURCE"."TB_CADASTRO_CLIENTE" (ID_CLIENTE, NOME_CLIENTE, EMAIL_CLIENTE) VALUES ('1099', 'Zico', 'zico@gmail.com');
INSERT INTO "SOURCE"."TB_CADASTRO_CLIENTE" (ID_CLIENTE, NOME_CLIENTE, EMAIL_CLIENTE) VALUES ('1000', 'Ronaldo', 'ronaldo@gmail.com');
INSERT INTO "SOURCE"."TB_CADASTRO_CLIENTE" (ID_CLIENTE, NOME_CLIENTE, EMAIL_CLIENTE) VALUES ('1198', 'Rivaldo', 'rivaldo@gmail.com');
INSERT INTO "SOURCE"."TB_CADASTRO_CLIENTE" (ID_CLIENTE, NOME_CLIENTE, EMAIL_CLIENTE) VALUES ('1298', 'Zidane', 'zidane@gmail.com');
INSERT INTO "SOURCE"."TB_CADASTRO_CLIENTE" (ID_CLIENTE, NOME_CLIENTE, EMAIL_CLIENTE) VALUES ('1398', 'Cristiano', 'cristiano@gmail.com');
INSERT INTO "SOURCE"."TB_CADASTRO_CLIENTE" (ID_CLIENTE, NOME_CLIENTE, EMAIL_CLIENTE) VALUES ('1048', 'Messi', '');
INSERT INTO "SOURCE"."TB_CADASTRO_CLIENTE" (ID_CLIENTE, NOME_CLIENTE, EMAIL_CLIENTE) VALUES ('1928', 'Julio', 'xxxgmail.com');
INSERT INTO "SOURCE"."TB_CADASTRO_CLIENTE" (ID_CLIENTE, NOME_CLIENTE, EMAIL_CLIENTE) VALUES ('1028', 'Messias', 'messias@gmail.com');
INSERT INTO "SOURCE"."TB_CADASTRO_CLIENTE" (ID_CLIENTE, NOME_CLIENTE, EMAIL_CLIENTE) VALUES ('1348', 'Matusalem', '12345');
COMMIT;

TRUNCATE TABLE TB_ENDERECO;
INSERT INTO "SOURCE"."TB_ENDERECO" (ID_ENDERECO, LOGRADOURO, NUMERO, CIDADE, ESTADO, PAIS, CEP, ID_CLIENTE) VALUES ('999887766', 'Rua Nano', '245', 'Rio de Janeiro', 'RJ', 'Brasil', '22998-761', '1098');
INSERT INTO "SOURCE"."TB_ENDERECO" (ID_ENDERECO, LOGRADOURO, NUMERO, CIDADE, ESTADO, PAIS, CEP, ID_CLIENTE) VALUES ('999887768', 'Rua Macieiras', '12', 'Belo Horizonte', 'MG', 'Brasil', '22998-763', '1099');
INSERT INTO "SOURCE"."TB_ENDERECO" (ID_ENDERECO, LOGRADOURO, NUMERO, CIDADE, ESTADO, PAIS, CEP, ID_CLIENTE) VALUES ('999887769', 'Av. Goiabeiras', '76', 'Vela Velha', 'ES', 'Brasil', '21998-763', '1000');
INSERT INTO "SOURCE"."TB_ENDERECO" (ID_ENDERECO, LOGRADOURO, NUMERO, CIDADE, ESTADO, PAIS, CEP, ID_CLIENTE) VALUES ('999887770', 'Av. Kremilim', '769', 'Cariacica', 'ES', 'Brasil', '21398-763', '1198');
commit;

TRUNCATE TABLE TB_CATEGORIA;
INSERT INTO "SOURCE"."TB_CATEGORIA" (ID_CATEGORIA, NOME_CATEGORIA, NOME_SUB_CATEGORIA) VALUES ('87654', 'Notebook', 'Pessoal');
INSERT INTO "SOURCE"."TB_CATEGORIA" (ID_CATEGORIA, NOME_CATEGORIA, NOME_SUB_CATEGORIA) VALUES ('87655', 'Notebook', 'Business');
INSERT INTO "SOURCE"."TB_CATEGORIA" (ID_CATEGORIA, NOME_CATEGORIA, NOME_SUB_CATEGORIA) VALUES ('87656', 'Camera', 'Longa Distância');
INSERT INTO "SOURCE"."TB_CATEGORIA" (ID_CATEGORIA, NOME_CATEGORIA, NOME_SUB_CATEGORIA) VALUES ('87657', 'Camera', 'Semi Profissional');
INSERT INTO "SOURCE"."TB_CATEGORIA" (ID_CATEGORIA, NOME_CATEGORIA, NOME_SUB_CATEGORIA) VALUES ('87658', 'Smartphone', '8 GB Memória');
INSERT INTO "SOURCE"."TB_CATEGORIA" (ID_CATEGORIA, NOME_CATEGORIA, NOME_SUB_CATEGORIA) VALUES ('87659', 'Smartphone', '4 GB Memória');
commit;

TRUNCATE TABLE TB_PRODUTO;
INSERT INTO "SOURCE"."TB_PRODUTO" (ID_PRODUTO, SKU, NOME_PRODUTO, ID_CATEGORIA) VALUES ('12098712', 'DFGTHN6ER4RF', 'Notebook Vaio', '87654');
INSERT INTO "SOURCE"."TB_PRODUTO" (ID_PRODUTO, SKU, NOME_PRODUTO, ID_CATEGORIA) VALUES ('12098713', 'DFWEHN6ER4RF', 'Iphone 8', '87658');
INSERT INTO "SOURCE"."TB_PRODUTO" (ID_PRODUTO, SKU, NOME_PRODUTO, ID_CATEGORIA) VALUES ('12098714', 'DF11HN6ER4RF', 'Camera Sony', '87657');
INSERT INTO "SOURCE"."TB_PRODUTO" (ID_PRODUTO, SKU, NOME_PRODUTO, ID_CATEGORIA) VALUES ('12098715', 'DFGUHN6ER4RF', 'Notebook MSI 16 GB', '87654');
INSERT INTO "SOURCE"."TB_PRODUTO" (ID_PRODUTO, SKU, NOME_PRODUTO, ID_CATEGORIA) VALUES ('12098716', 'DFGUHN6E07RF', 'Samsung Galaxy 9', '87659');
INSERT INTO "SOURCE"."TB_PRODUTO" (ID_PRODUTO, SKU, NOME_PRODUTO, ID_CATEGORIA) VALUES ('12098717', 'DFGUHN6ER08F', 'Camera Canon XTR', '87656');
INSERT INTO "SOURCE"."TB_PRODUTO" (ID_PRODUTO, SKU, NOME_PRODUTO, ID_CATEGORIA) VALUES ('12098718', 'DFGUHN6094RF', 'Notebook ASUS 16 GB', '87654');
commit;

TRUNCATE TABLE TB_LOCALIDADE;
INSERT INTO "SOURCE"."TB_LOCALIDADE" (ID_LOCALIDADE, NOME_LOCALIDADE, CIDADE_LOCALIDADE) VALUES ('1', 'Loja Barueri', 'Barueri');
INSERT INTO "SOURCE"."TB_LOCALIDADE" (ID_LOCALIDADE, NOME_LOCALIDADE, CIDADE_LOCALIDADE) VALUES ('2', 'Loja Centro', 'São Paulo');
INSERT INTO "SOURCE"."TB_LOCALIDADE" (ID_LOCALIDADE, NOME_LOCALIDADE, CIDADE_LOCALIDADE) VALUES ('3', 'Loja Tatuape', 'Tatuapé');
INSERT INTO "SOURCE"."TB_LOCALIDADE" (ID_LOCALIDADE, NOME_LOCALIDADE, CIDADE_LOCALIDADE) VALUES ('4', 'Loja Cinelandia', 'Rio de Janeiro');
INSERT INTO "SOURCE"."TB_LOCALIDADE" (ID_LOCALIDADE, NOME_LOCALIDADE, CIDADE_LOCALIDADE) VALUES ('5', 'Loja Pelourinho', 'Salvador');
commit;

TRUNCATE TABLE TB_PEDIDOS;
INSERT INTO "SOURCE"."TB_PEDIDOS" (ID_TRANSACAO, DATA_TRANSACAO, DATA_ENTREGA, STATUS_PAGAMENTO, ID_CLIENTE, ID_LOCALIDADE) VALUES ('009987654432', null, TO_TIMESTAMP('2018-04-17 14:23:22.395000000', 'YYYY-MM-DD HH24:MI:SS.FF'), 'Pago', '1099', 1);
INSERT INTO "SOURCE"."TB_PEDIDOS" (ID_TRANSACAO, DATA_TRANSACAO, DATA_ENTREGA, STATUS_PAGAMENTO, ID_CLIENTE, ID_LOCALIDADE) VALUES ('009985654432', TO_TIMESTAMP('2018-04-16 14:22:38.437000000', 'YYYY-MM-DD HH24:MI:SS.FF'), TO_TIMESTAMP('2018-04-17 15:23:22.395000000', 'YYYY-MM-DD HH24:MI:SS.FF'), 'Pago', '1098', 2);
INSERT INTO "SOURCE"."TB_PEDIDOS" (ID_TRANSACAO, DATA_TRANSACAO, DATA_ENTREGA, STATUS_PAGAMENTO, ID_CLIENTE, ID_LOCALIDADE) VALUES ('009985554432', null, TO_TIMESTAMP('2018-04-17 16:23:22.395000000', 'YYYY-MM-DD HH24:MI:SS.FF'), 'Pago', '1000', 5);
INSERT INTO "SOURCE"."TB_PEDIDOS" (ID_TRANSACAO, DATA_TRANSACAO, DATA_ENTREGA, STATUS_PAGAMENTO, ID_CLIENTE, ID_LOCALIDADE) VALUES ('009981254432', TO_TIMESTAMP('2018-04-16 16:22:38.437000000', 'YYYY-MM-DD HH24:MI:SS.FF'), TO_TIMESTAMP('2018-04-17 16:23:22.395000000', 'YYYY-MM-DD HH24:MI:SS.FF'), 'NA', '1398', 2);
INSERT INTO "SOURCE"."TB_PEDIDOS" (ID_TRANSACAO, DATA_TRANSACAO, DATA_ENTREGA, STATUS_PAGAMENTO, ID_CLIENTE, ID_LOCALIDADE) VALUES ('009982354432', TO_TIMESTAMP('2018-04-16 17:28:38.437000000', 'YYYY-MM-DD HH24:MI:SS.FF'), TO_TIMESTAMP('2018-04-17 17:13:22.395000000', 'YYYY-MM-DD HH24:MI:SS.FF'), 'Pago', '1048', 3);
INSERT INTO "SOURCE"."TB_PEDIDOS" (ID_TRANSACAO, DATA_TRANSACAO, DATA_ENTREGA, STATUS_PAGAMENTO, ID_CLIENTE, ID_LOCALIDADE) VALUES ('009987954432', TO_TIMESTAMP('2018-04-16 18:24:38.437000000', 'YYYY-MM-DD HH24:MI:SS.FF'), TO_TIMESTAMP('2018-04-17 18:43:22.395000000', 'YYYY-MM-DD HH24:MI:SS.FF'), 'Pago', '1028', 4);
INSERT INTO "SOURCE"."TB_PEDIDOS" (ID_TRANSACAO, DATA_TRANSACAO, DATA_ENTREGA, STATUS_PAGAMENTO, ID_CLIENTE, ID_LOCALIDADE) VALUES ('009980954432', TO_TIMESTAMP('2018-04-16 13:29:38.437000000', 'YYYY-MM-DD HH24:MI:SS.FF'), TO_TIMESTAMP('2018-04-17 19:53:22.395000000', 'YYYY-MM-DD HH24:MI:SS.FF'), 'Pago', '1348', 5);
commit;

TRUNCATE TABLE TB_ITENS_PEDIDO;
INSERT INTO "SOURCE"."TB_ITENS_PEDIDO" (ID_TRANSACAO, ID_PRODUTO, QUANTIDADE, PRECO_UNITARIO) VALUES ('009987654432', '12098712', '1', '2900,00');
INSERT INTO "SOURCE"."TB_ITENS_PEDIDO" (ID_TRANSACAO, ID_PRODUTO, QUANTIDADE, PRECO_UNITARIO) VALUES ('009985654432', '12098713', '1', '3900,00');
INSERT INTO "SOURCE"."TB_ITENS_PEDIDO" (ID_TRANSACAO, ID_PRODUTO, QUANTIDADE, PRECO_UNITARIO) VALUES ('009985554432', '12098712', '3', '2870,00');
INSERT INTO "SOURCE"."TB_ITENS_PEDIDO" (ID_TRANSACAO, ID_PRODUTO, QUANTIDADE, PRECO_UNITARIO) VALUES ('009981254432', '12098715', '1', '1765,00');
INSERT INTO "SOURCE"."TB_ITENS_PEDIDO" (ID_TRANSACAO, ID_PRODUTO, QUANTIDADE, PRECO_UNITARIO) VALUES ('009982354432', '12098714', '2', '1740,00');
INSERT INTO "SOURCE"."TB_ITENS_PEDIDO" (ID_TRANSACAO, ID_PRODUTO, QUANTIDADE, PRECO_UNITARIO) VALUES ('009987954432', '12098712', '1', '1900,00');
INSERT INTO "SOURCE"."TB_ITENS_PEDIDO" (ID_TRANSACAO, ID_PRODUTO, QUANTIDADE, PRECO_UNITARIO) VALUES ('009980954432', '12098718', '2', '856,00');
commit;
```

Para verificar se tudo está funcionando, rode o script:

```sql
select nome_localidade, nome_produto, sum(d.quantidade * d.preco_unitario) as total
from TB_LOCALIDADE a, TB_PRODUTO b, TB_PEDIDOS c, TB_ITENS_PEDIDO d
where a.id_localidade = c.ID_LOCALIDADE
and b.ID_PRODUTO = d.ID_PRODUTO
and c.ID_TRANSACAO = d.ID_TRANSACAO
group by nome_localidade, nome_produto
order by nome_localidade, nome_produto
```

![Untitled](/Imagens/Untitled%20146.png)

Com isso o sistema legado está pronto, com isso, já é possível criar a staging area

<br>

<a name = "Staging"></a>
### Criando a Staging Area

Com o banco de dados no ar, abra o Oracle SQL Developer e faça a conexão com o usuário `sys`

Com o usuário `sys`, libere as permissões de scripts na seção, rode o seguinte script:

```sql
alter session set "_ORACLE_SCRIPT"=true;
```

Crie uma tablespace `stage`, rode o seguinte script:

```sql
CREATE TABLESPACE TBS_STAGE
LOGGING DATAFILE '/u01/app/oracle/oradata/ORCL21C/STAGE.dbf' 
SIZE 1M AUTOEXTEND ON NEXT 10M MAXSIZE UNLIMITED EXTENT MANAGEMENT LOCAL SEGMENT SPACE MANAGEMENT AUTO;
```

Verifique se a tablespace foi criada corretamente, rode o seguinte script:

```sql
select * from dba_data_files
```

![Untitled](/Imagens/Untitled%20147.png)

Com a tablespace criada, crie o usuário `STAGEAREA`, rode o seguinte script:

```sql
CREATE USER STAGEAREA IDENTIFIED BY ***<SENHA>***
DEFAULT TABLESPACE "TBS_STAGE"
TEMPORARY TABLESPACE "TEMP";

GRANT "CONNECT" TO "STAGEAREA";
GRANT "CDB_DBA" TO "STAGEAREA" ;
GRANT "RESOURCE" TO "STAGEAREA" ;

GRANT SELECT ANY TABLE TO "STAGEAREA" ;
GRANT CREATE TABLESPACE TO "STAGEAREA" ;
GRANT UNLIMITED TABLESPACE TO "STAGEAREA" ;
```

Para verificar se o usuário foi criado, rode o seguinte script:

```sql
SELECT * 
FROM all_users 
WHERE USERNAME = 'STAGEAREA'
```

![Untitled](/Imagens/Untitled%20148.png)

Inicie uma sessão com o usuário `STAGEAREA`

![Untitled](/Imagens/Untitled%20149.png)

Criando o schema da staging area

Com o usuário `STAGEAREA`, rode o seguinte script:

```sql
CREATE TABLE ST_CADASTRO_CLIENTE 
(
    ID_CLIENTE INTEGER  NOT NULL 
    , NOME_CLIENTE VARCHAR2(255) 
    , EMAIL_CLIENTE VARCHAR2(255) 
);

CREATE TABLE ST_ENDERECO 
(
    ID_ENDERECO INTEGER NOT NULL 
    , LOGRADOURO VARCHAR2(255) 
    , NUMERO NUMBER 
    , CIDADE VARCHAR2(2255) 
    , ESTADO VARCHAR2(255) 
    , PAIS VARCHAR2(255) 
    , CEP VARCHAR2(255) 
    , ID_CLIENTE INTEGER
);

CREATE TABLE ST_PRODUTO 
(
    ID_PRODUTO INTEGER NOT NULL 
    , SKU VARCHAR2(255) 
    , NOME_PRODUTO VARCHAR2(255) 
    , ID_CATEGORIA INTEGER
);

CREATE TABLE ST_CATEGORIA 
(
    ID_CATEGORIA INTEGER NOT NULL 
    , NOME_CATEGORIA VARCHAR2(255) 
    , NOME_SUB_CATEGORIA VARCHAR2(255) 
);

CREATE TABLE ST_LOCALIDADE 
(
    ID_LOCALIDADE INTEGER NOT NULL 
    , NOME_LOCALIDADE VARCHAR2(255) 
    , CIDADE_LOCALIDADE VARCHAR2(255) 
);

CREATE TABLE ST_PEDIDOS 
(
    ID_TRANSACAO INTEGER NOT NULL 
    , DATA_TRANSACAO TIMESTAMP 
    , DATA_ENTREGA TIMESTAMP 
    , STATUS_PAGAMENTO VARCHAR2(255) 
    , ID_CLIENTE INTEGER 
    , ID_LOCALIDADE INTEGER 
);

CREATE TABLE ST_ITENS_PEDIDO 
(
    ID_TRANSACAO INTEGER NOT NULL 
    , ID_PRODUTO INTEGER NOT NULL 
    , QUANTIDADE INTEGER 
    , PRECO_UNITARIO DOUBLE PRECISION 
);
```

Com o schema criado, já é possível trazer os dados do sistema legado

<br>

<a name = "Conexoes"></a>
### Criando as conexões com o banco

O primeiro passo é criar a conexão com o banco de dados

Para isso é necessário a instalação do driver JDBC da Oracle, acesse o link

[JDBC and UCP Downloads page | Oracle Brasil](https://www.oracle.com/br/database/technologies/appdev/jdbc-downloads.html)

Clique em `Oracle Database 21c (21.9.0.0) JDBC Driver & UCP Downloads - Innovation Release`

Clique em `ojdbc11.jar`

![Untitled](/Imagens/Untitled%20150.png)

Após baixar o arquivo, mova ele para a pasta `lib`, dentro da instalação do Pentaho

![Untitled](/Imagens/Untitled%20151.png)

Abra o `Spoon.bat`

Clique duas vezes em `Transformações`

![Untitled](/Imagens/Untitled%20152.png)

Na aba `View`, clique duas vezes em `Conexões`

![Untitled](/Imagens/Untitled%20153.png)

Em `Connection name`, insira um nome para identificar a conexão

Em `Connection type`, selecione `Oracle`

Em `Access`, selecione `Native (JDBC)`

Em `Host Name`, insira o `IP` da máquina com o Oracle Database

Em `Database Name`, insira `orcl21c`

Em `Port Number`, mantenha a porta padrão `1521`

Em `Username`, insira `source`

Em `Password`, insira a senha criada para o usuário `source` anteriormente

Clique em `Test`

![Untitled](/Imagens/Untitled%20154.png)

Verifique se a conexão foi bem-sucedida

Clique em `OK`

![Untitled](/Imagens/Untitled%20155.png)

Repita esse processo e crie a mesma conexão mudando apenas o usuário para o Stage

![Untitled](/Imagens/Untitled%20156.png)

![Untitled](/Imagens/Untitled%20157.png)

Clique em Tools, em assistente, clique em Copy Tables

![Untitled](/Imagens/Untitled%20158.png)

Na esquerda, selecione `source`

Na direita, selecione `stagearea`

Clique em `Next >`

![Untitled](/Imagens/Untitled%20159.png)

Selecione as tabelas que serão copiadas do Source para a Stage Area

Clique em `Add the selected items on the left.`

![Untitled](/Imagens/Untitled%20160.png)

Repita esse processo para todas as tabelas

Clique em `Next ->`

![Untitled](/Imagens/Untitled%20161.png)

Selecione um nome para identificar o Job

Selecione um diretório onde será salvo o Job

Clique em `Finish`

![Untitled](/Imagens/Untitled%20162.png)

Será criado esse Job

![Untitled](/Imagens/Untitled%20163.png)

Clique em `Run` para rodar o Job

![Untitled](/Imagens/Untitled%20164.png)

Mantenha as configurações padrões

Clique em `Run`

![Untitled](/Imagens/Untitled%20165.png)

Para rodar o job é necessário salvar, clique em `Sim`

![Untitled](/Imagens/Untitled%20166.png)

Verifique se após rodar o Job se as transformações foram bem-sucedidas do início ao fim

![Untitled](/Imagens/Untitled%20167.png)

Com a conexão da Stage Area no banco de dados, verifique se as tabelas foram copiadas corretamente

```sql
SELECT table_name 
FROM user_tables;
```

![Untitled](/Imagens/Untitled%20168.png)

![Untitled](/Imagens/Untitled%20169.png)

Renomeie o nome das tabelas com as iniciais ST, seguindo as boas práticas

```sql
ALTER TABLE TB_CADASTRO_CLIENTE RENAME TO ST_CADASTRO_CLIENTE;
ALTER TABLE TB_CATEGORIA RENAME TO ST_CATEGORIA;
ALTER TABLE TB_ENDERECO RENAME TO ST_ENDERECO;
ALTER TABLE TB_ITENS_PEDIDO RENAME TO ST_ITENS_PEDIDO;
ALTER TABLE TB_LOCALIDADE RENAME TO ST_LOCALIDADE;
ALTER TABLE TB_PEDIDOS RENAME TO ST_PEDIDOS;
ALTER TABLE TB_PRODUTO RENAME TO ST_PRODUTO;

SELECT table_name 
FROM user_tables;
```

![Untitled](/Imagens/Untitled%20170.png)

<br>

<a name = "Transformacoes"></a>
### Transformações

A parte mais importante e demorada da implementação é nessa etapa, estarei realizando transformações para se adequar ao modelo dimensional criado na etapa de modelagem dimensional

<br>

<a name = "T-D-Cliente"></a>
<b>Criando a dimensão cliente</b>

Para criar a dimensão cliente, é necessário realizar alguns ajustes na tabela ST_CADASTRO_CLIENTE, nela que será baseada a criação da dimensão

Verificando a tabela ST_CADASTRO_CLIENTE

```sql
SELECT *
FROM ST_CADASTRO_CLIENTE;
```

![Untitled](/Imagens/Untitled%20171.png)

Adicionando campo de data

```sql
ALTER TABLE ST_CADASTRO_CLIENTE 
ADD (DATA_REGISTRO DATE);

COMMIT;

SELECT *
FROM ST_CADASTRO_CLIENTE;
```

![Untitled](/Imagens/Untitled%20172.png)

Preenchendo a data

```sql
UPDATE ST_CADASTRO_CLIENTE 
SET DATA_REGISTRO = SYSDATE;

COMMIT;

SELECT * 
FROM ST_CADASTRO_CLIENTE;
```

![Untitled](/Imagens/Untitled%20173.png)

Corrigindo e-mails mal cadastrados

```sql
SELECT * 
FROM ST_CADASTRO_CLIENTE;
```

![Untitled](/Imagens/Untitled%20174.png)

Removendo e-mails nulos

```sql
UPDATE ST_CADASTRO_CLIENTE 
SET EMAIL_CLIENTE = 'NÃO INFORMADO' 
WHERE EMAIL_CLIENTE IS NULL;

COMMIT;

SELECT * 
FROM ST_CADASTRO_CLIENTE;
```

![Untitled](/Imagens/Untitled%20175.png)

Removendo e-mail que não existe

```sql
UPDATE ST_CADASTRO_CLIENTE 
SET EMAIL_CLIENTE = 'NÃO INFORMADO' 
WHERE SUBSTR(EMAIL_CLIENTE, 1, 3) = 'xxx';

COMMIT;

SELECT * 
FROM ST_CADASTRO_CLIENTE;
```

![Untitled](/Imagens/Untitled%20176.png)

Removendo e-mail campo sem e-mail, apenas número

```sql
UPDATE ST_CADASTRO_CLIENTE 
SET EMAIL_CLIENTE = 'NÃO INFORMADO' 
WHERE EMAIL_CLIENTE = '12345'

COMMIT;

SELECT * 
FROM ST_CADASTRO_CLIENTE;
```

![Untitled](/Imagens/Untitled%20177.png)

Criando a tabela da dimensão cliente

```sql
CREATE TABLE ST_DIM_CLIENTE
(
    NK_ID_CLIENTE VARCHAR2(20),
    NM_CLIENTE VARCHAR2(50),
    NM_CIDADE_CLIENTE VARCHAR2(50),
    FLAG_ACEITA_CAMPANHA CHAR(1),
    DESC_CEP VARCHAR2(10)
);

INSERT INTO ST_DIM_CLIENTE
SELECT 
    A.ID_CLIENTE,
    A.NOME_CLIENTE,
    B.CIDADE, 0,
    B.CEP
FROM ST_CADASTRO_CLIENTE A, ST_ENDERECO B
WHERE A.ID_CLIENTE = B.ID_CLIENTE;

COMMIT;

SELECT * 
FROM ST_DIM_CLIENTE;
```

![Untitled](/Imagens/Untitled%20178.png)

<br>

<a name = "T-D-Localidade"></a>
<b>Criando a dimensão localidade</b>

```sql
CREATE TABLE ST_DIM_LOCALIDADE
(
    NK_ID_LOCALIDADE VARCHAR2(20) NOT NULL,
    NM_LOCALIDADE VARCHAR2(50) NOT NULL,
    NM_CIDADE_LOCALIDADE VARCHAR2(50) NOT NULL,
    NM_REGIAO_LOCALIDADE VARCHAR2(50) NOT NULL
);

INSERT INTO ST_DIM_LOCALIDADE
SELECT
     ID_LOCALIDADE,
     NOME_LOCALIDADE,
     CIDADE_LOCALIDADE,
CASE WHEN CIDADE_LOCALIDADE = 'Barueri' THEN 'Sudeste'
     WHEN CIDADE_LOCALIDADE = 'São Paulo' THEN 'Sudeste'
     WHEN CIDADE_LOCALIDADE = 'Rio de Janeiro' THEN 'Sudeste'
     WHEN CIDADE_LOCALIDADE = 'Salvador' THEN 'Nordeste '
     WHEN CIDADE_LOCALIDADE = 'Tatuapé' THEN 'Sudeste'
ELSE 'NA'
END AS REGIAO
FROM ST_LOCALIDADE;

COMMIT;

SELECT *
FROM ST_DIM_LOCALIDADE;
```

![Untitled](/Imagens/Untitled%20179.png)

<br>

<a name = "T-D-Produto"></a>
<b>Criando a dimensão produto</b>

```sql
CREATE TABLE ST_DIM_PRODUTO
(
    NK_ID_PRODUTO VARCHAR2(20) NOT NULL,
    DESC_SKU VARCHAR2(50) NOT NULL,
    NM_PRODUTO VARCHAR2(50) NOT NULL,
    NM_CATEGORIA_PRODUTO VARCHAR2(30) NOT NULL,
    NM_MARCA_PRODUTO VARCHAR2(30) NOT NULL
);

INSERT INTO ST_DIM_PRODUTO
SELECT 
     A.ID_PRODUTO,
     A.SKU,
     A.NOME_PRODUTO,
     B.NOME_CATEGORIA,
CASE WHEN A.NOME_PRODUTO LIKE '%Sony%' THEN 'Sony'
     WHEN A.NOME_PRODUTO LIKE '%Iphone%' THEN 'Apple'
     WHEN A.NOME_PRODUTO LIKE '%MSI%' THEN 'MSI'
     WHEN A.NOME_PRODUTO LIKE '%Galaxy%' THEN 'Samsung'
     WHEN A.NOME_PRODUTO LIKE '%ASUS%' THEN 'Asus'
     WHEN A.NOME_PRODUTO LIKE '%Vaio' THEN 'Vaio'
     WHEN A.NOME_PRODUTO LIKE '%Canon%' THEN 'Canon'
     ELSE 'NA'
     END AS MARCA_PRODUTO
FROM ST_PRODUTO A, ST_CATEGORIA B
WHERE A.ID_CATEGORIA = B.ID_CATEGORIA;

COMMIT;

SELECT *
FROM ST_DIM_PRODUTO;
```

![Untitled](/Imagens/Untitled%20180.png)

<br>

<a name = "T-Fato"></a>
<b>Criando o fato venda</b>

```sql
CREATE TABLE ST_VENDA
(
    ID_TRANSACAO INTEGER,
    DATA_VENDA DATE,
    STATUS_PAGAMENTO VARCHAR2(20),
    ID_CLIENTE INTEGER,
    ID_LOCALIDADE INTEGER,
    ID_PRODUTO INTEGER,
    QUANTIDADE INTEGER,
    PRECO_UNITARIO DECIMAL
);

INSERT INTO ST_VENDA
    SELECT 
    A.ID_TRANSACAO, A.DATA_ENTREGA, 
    CASE
    WHEN A.STATUS_PAGAMENTO = 'NA' THEN 'Erro'
    ELSE A.STATUS_PAGAMENTO
    END as STATUS_PAGAMENTO, A.ID_CLIENTE, A.ID_LOCALIDADE, B.ID_PRODUTO, B.QUANTIDADE, B.PRECO_UNITARIO
FROM ST_PEDIDOS A, ST_ITENS_PEDIDO B
WHERE A.ID_TRANSACAO = B.ID_TRANSACAO;
COMMIT;

SELECT * FROM ST_VENDA;
```

![Untitled](/Imagens/Untitled%20181.png)
