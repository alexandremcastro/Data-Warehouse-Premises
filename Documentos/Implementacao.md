## Sumário
+ [Implementação](#Implementacao)
    + [Criando o Data Warehouse](#DataWarehouse)
        + [Dimensão cliente](#DW-Cliente)
        + [Dimensão produto](#DW-Produto)
        + [Dimensão localidade](#DW-Localidade)
        + [Dimensão tempo](#DW-Tempo)
        + [Fato venda](#DW-Fato)
    + [Carga de dados](#CargaDados)
        + [Dimensão tempo](#CargaTempo)
        + [Dimensão cliente](#CargaCliente)
        + [Dimensão produto](#CargaProduto)
        + [Dimensão localidade](#CargaLocalidade)
        + [Fato venda](#FatoVendaa)
    + [Verificando a integridade de dados](#Integridade)

<a name = "Implementacao"></a>
## Implementação
[Voltar para o ínicio ↑](#Inicio)

<a name = "DataWarehouse"></a>
### Criando o Data Warehouse

Primeiro passo é logar no banco de dados com o usuário sys

![Untitled](/Imagens/Untitled%20182.png)

Crie a tablespace para armazenar os dados do Data Warehouse

```sql
CREATE TABLESPACE TBS_DW
LOGGING DATAFILE '/u01/app/oracle/oradata/ORCL21C/TBS_DW.dbf'
SIZE 1M AUTOEXTEND ON NEXT 10M MAXSIZE UNLIMITED EXTENT MANAGEMENT LOCAL SEGMENT SPACE MANAGEMENT AUTO;
```

Removendo as limitações da tablespace criada

```sql
ALTER USER dw QUOTA UNLIMITED ON TBS_DW;
```

Criando schema DW

```sql
CREATE USER dw IDENTIFIED BY dw0123
DEFAULT TABLESPACE TBS_DW
TEMPORARY TABLESPACE TEMP;

GRANT CONNECT TO dw ;
GRANT RESOURCE TO dw ;

GRANT SELECT ANY TABLE TO dw;
GRANT INSERT ANY TABLE TO dw;
GRANT CREATE ANY TABLE TO dw;
```

Entre no usuário criado `dw`

![Untitled](/Imagens/Untitled%20183.png)

<a name = "DW-Cliente"></a>
<b>Dimensão cliente</b>

Criando a sequência para receber o campo da surrogate key na dimensão cliente

```sql
CREATE SEQUENCE DIM_CLIENTE_ID_SEQ START WITH 100 INCREMENT BY 1 NOCACHE NOCYCLE;
```

Criando a dimensão cliente com a surrogate key, mencionando a sequência

```sql
CREATE TABLE TB_DIM_CLIENTE
(
    SK_CLIENTE INTEGER DEFAULT dim_cliente_id_seq.NEXTVAL,
    NK_ID_CLIENTE VARCHAR2(20),
    NM_CLIENTE VARCHAR2(50),
    NM_CIDADE_CLIENTE VARCHAR2(50),
    FLAG_ACEITA_CAMPANHA CHAR(1),
    DESC_CEP VARCHAR2(10),
    CONSTRAINT TB_DIM_CLIENTE_PK PRIMARY KEY (SK_CLIENTE) ENABLE
);
```

<br>

<a name = "DW-Produto"></a>
<b>Dimensão produto</b>

Criando a sequência para receber o campo da surrogate key na dimensão produto

```sql
CREATE SEQUENCE DIM_PRODUTO_ID_SEQ START WITH 100 INCREMENT BY 1 NOCACHE NOCYCLE;
```

Criando a dimensão cliente com a surrogate key, mencionando a sequência

```sql
CREATE TABLE TB_DIM_PRODUTO
(
    SK_PRODUTO INTEGER DEFAULT DIM_PRODUTO_ID_SEQ.NEXTVAL,
    NK_ID_PRODUTO VARCHAR(20) NOT NULL,
    DESC_SKU VARCHAR2(50) NOT NULL,
    NM_PRODUTO VARCHAR2(50) NOT NULL,
    NM_CATEGORIA_PRODUTO VARCHAR2(30) NOT NULL,
    NM_MARCA_PRODUTO VARCHAR2(30) NOT NULL,
    CONSTRAINT TB_DIM_PRODUTO_PK PRIMARY KEY (SK_PRODUTO) ENABLE
);
```

<br>

<a name = "DW-Localidade"></a>
<b>Dimensão localidade</b>

Criando a sequência para receber o campo da surrogate key na dimensão localidade

```sql
CREATE SEQUENCE DIM_LOCALIDADE_ID_SEQ START WITH 100 INCREMENT BY 1 NOCACHE NOCYCLE;
```

Criando a dimensão cliente com a surrogate key, mencionando a sequência

```sql
CREATE TABLE TB_DIM_LOCALIDADE
(
    SK_LOCALIDADE INTEGER DEFAULT DIM_LOCALIDADE_ID_SEQ.NEXTVAL,
    NK_ID_LOCALIDADE VARCHAR2(20) NOT NULL,
    NM_LOCALIDADE VARCHAR2(50) NOT NULL,
    NM_CIDADE_LOCALIDADE VARCHAR2(50) NOT NULL,
    NM_REGIAO_LOCALIDADE VARCHAR2(50) NOT NULL,
    CONSTRAINT TB_DIM_LOCALIDADE_PK PRIMARY KEY (SK_LOCALIDADE) ENABLE
);
```

<br>

<a name = "DW-Tempo"></a>
<b>Dimensão tempo</b>

Criando a dimensão cliente com a surrogate key, mencionando a sequência

```sql
CREATE TABLE TB_DIM_TEMPO
(
    SK_DATA INTEGER NOT NULL,
    DATA DATE NOT NULL,
    NR_ANO NUMBER(4) NOT NULL,
    NR_MES NUMBER NOT NULL,
    NM_MES VARCHAR2(20) NOT NULL,
    NR_DIA NUMBER NOT NULL,
    CONSTRAINT TB_DIM_DATA_PK PRIMARY KEY (SK_DATA) ENABLE
);
```

<br>

<a name = "DW-Fato"></a>
<b>Fato venda</b>

Criando o fato venda

```sql
CREATE TABLE TB_FATO_VENDA
(
SK_CLIENTE INTEGER NOT NULL,
SK_PRODUTO INTEGER NOT NULL,
SK_LOCALIDADE INTEGER NOT NULL,
SK_DATA INTEGER NOT NULL,
VL_VENDA DECIMAL(12, 4),
QNT_VENDA INTEGER,
CONSTRAINT TB_FATO_VENDA_PK PRIMARY KEY (SK_CLIENTE, SK_PRODUTO, SK_LOCALIDADE, SK_DATA)                    ENABLE
);
```

<br>

<a name = "CargaDados"></a>
### Carga de dados

<br>

<a name = "CargaTempo"></a>
<b>Dimensão tempo</b>

Criando os dados de tempo para a dimensão tempo

```sql
INSERT INTO TB_DIM_TEMPO 
SELECT 
    TO_CHAR(
    TO_DATE('31/12/2012', 'DD/MM/YYYY') + NUMTODSINTERVAL(n, 'day'), 
    'YYYYMMDD'
    ) AS SK_DATA, 
    TO_DATE('31/12/2012', 'DD/MM/YYYY') + NUMTODSINTERVAL(n, 'day') as FULL_DATE, 
    TO_CHAR(
    TO_DATE('31/12/2012', 'DD/MM/YYYY') + NUMTODSINTERVAL(n, 'day'), 
    'YYYY'
    ) AS NR_ANO, 
    TO_CHAR(
    TO_DATE('31/12/2012', 'DD/MM/YYYY') + NUMTODSINTERVAL(n, 'day'), 
    'MM'
    ) AS NR_MES, 
    TO_CHAR(
    TO_DATE('31/12/2012', 'DD/MM/YYYY') + NUMTODSINTERVAL(n, 'day'), 
    'Month'
    ) AS NM_MES, 
    TO_CHAR(
    TO_DATE('31/12/2012', 'DD/MM/YYYY') + NUMTODSINTERVAL(n, 'day'), 
    'DD'
    ) AS NR_DIA 
FROM 
(
    SELECT 
    LEVEL N 
    FROM 
    DUAL CONNECT BY LEVEL <= 2000
);
COMMIT;
```

<br>

<a name = "CargaCliente"></a>
<b>Dimensão cliente</b>

```sql
INSERT INTO TB_DIM_CLIENTE 
SELECT 
    DIM_CLIENTE_ID_SEQ.NEXTVAL, 
    NK_ID_CLIENTE, 
    NM_CLIENTE, 
    NM_CIDADE_CLIENTE, 
    FLAG_ACEITA_CAMPANHA, 
    DESC_CEP 
FROM 
    stagearea.ST_DIM_CLIENTE;

COMMIT;
```

<br>

<a name = "CargaProduto"></a>
<b>Dimensão produto</b>

```sql
INSERT INTO TB_DIM_PRODUTO 
SELECT 
    DIM_PRODUTO_ID_SEQ.NEXTVAL, 
    NK_ID_PRODUTO, 
    DESC_SKU, 
    NM_PRODUTO, 
    NM_CATEGORIA_PRODUTO, 
    NM_MARCA_PRODUTO 
FROM 
    stagearea.ST_DIM_PRODUTO;

COMMIT;
```

<br>

<a name = "CargaLocalidade"></a>
<b>Dimensão localidade</b>

```sql
INSERT INTO TB_DIM_LOCALIDADE 
SELECT 
    DIM_LOCALIDADE_ID_SEQ.NEXTVAL, 
    NK_ID_LOCALIDADE, 
    NM_LOCALIDADE, 
    NM_CIDADE_LOCALIDADE, 
    NM_REGIAO_LOCALIDADE 
FROM 
    stagearea.ST_DIM_LOCALIDADE;

COMMIT;
```

<br>

<a name = "FatoVendaa"></a>
<b>Fato venda</b>

Adicionando na dimensão cliente, um registro para clientes não identificados nas vendas

```sql
INSERT INTO TB_DIM_CLIENTE 
VALUES 
(
    -1, -1, '<não identificado>', '<não identificado>', 
    0, 'NA'
);
COMMIT;
```

Criando a fato venda

```sql
INSERT INTO TB_FATO_VENDA 
SELECT 
    COALESCE(B.SK_CLIENTE, -1) AS SK_CLIENTE, 
    COALESCE(C.SK_PRODUTO, -1) AS SK_PRODUTO, 
    COALESCE(D.SK_LOCALIDADE, -1) AS SK_LOCALIDADE, 
    TO_NUMBER(
    TO_CHAR(DATA_VENDA, 'yyyymmdd'), 
    '99999999'
    ) AS SK_DATA, 
    (A.PRECO_UNITARIO * A.QUANTIDADE) AS VL_VENDA, 
    A.QUANTIDADE AS QNT_VENDA 
FROM 
    STAGEAREA.ST_VENDA A 
    LEFT JOIN TB_DIM_CLIENTE B ON A.ID_CLIENTE = B.NK_ID_CLIENTE 
    LEFT JOIN TB_DIM_PRODUTO C ON A.ID_PRODUTO = C.NK_ID_PRODUTO 
    LEFT JOIN TB_DIM_LOCALIDADE D ON A.ID_LOCALIDADE = D.NK_ID_LOCALIDADE;
COMMIT;
```

Interligando as chaves entre as dimensões

```sql
<-- DIMENSÃO CLIENTE -->
ALTER TABLE TB_FATO_VENDA
ADD CONSTRAINT TB_FATO_VENDA_FK_CLIENTE FOREIGN KEY (SK_CLIENTE)
REFERENCES TB_DIM_CLIENTE (SK_CLIENTE)

ENABLE;

<-- DIMENSÃO TEMPO -->
ALTER TABLE TB_FATO_VENDA
ADD CONSTRAINT TB_FATO_VENDA_FK_DATA FOREIGN KEY (SK_DATA)
REFERENCES TB_DIM_TEMPO (SK_DATA)

ENABLE;

<-- DIMENSÃO  LOCALIDADE -->
ALTER TABLE TB_FATO_VENDA
ADD CONSTRAINT TB_FATO_VENDA_FK_LOCALIDADE FOREIGN KEY (SK_LOCALIDADE)
REFERENCES TB_DIM_LOCALIDADE (SK_LOCALIDADE)

ENABLE;

<-- DIMENSÃO PRODUTO -->
ALTER TABLE TB_FATO_VENDA
ADD CONSTRAINT TB_FATO_VENDA_FK_PRODUTO FOREIGN KEY (SK_PRODUTO)
REFERENCES TB_DIM_PRODUTO (SK_PRODUTO)

ENABLE;

COMMIT;
```

<br>

<a name = "Integridade"></a>
### Verificando a integridade de dados

Clique em `Arquivo —> Importar —> Dicionário de Dados`

![Untitled](/Imagens/Untitled%20184.png)

Selecione a conexão do Data Warehouse

Clique em `Próximo >`

![Untitled](/Imagens/Untitled%20185.png)

Selecione o esquema `DW`

Clique em `Próximo >`

![Untitled](/Imagens/Untitled%20186.png)

Selecione todos os objetos

Clique em `Próximo >`

![Untitled](/Imagens/Untitled%20187.png)

Verifique as informações

Clique em `Finalizar`

![Untitled](/Imagens/Untitled%20188.png)

Será gerado o modelo dimensional, verifique se ele está igual ao que foi projetado na etapa de modelagem

![Untitled](/Imagens/Untitled%20189.png)

Com isso, o Data Warehouse está pronto pra uso!
