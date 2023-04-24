## Sumário
+ [Modelagem](#Modelagem)
    + [Modelo negocial](#ModeloNegocial)
    + [Modelo lógico](#ModeloLogico)
        + [Como usar a ferramenta (básico)](#UsarFerramenta)
        + [Modelo](#ModeloL)
    + [Modelo relacional](#ModeloRelacional)
        + [Convertendo o modelo lógico para relacional](#ConversaoL-R)
        + [Modelo](#ModeloR)
    + [Modelo dimensional](#ModeloDimensional)
        + [Modelo](#ModeloD)
    + [Modelo físico](#ModeloFisico)
        + [Convertendo o modelo dimensional para físico](#ConversaoD-F)
        + [Modelo](#ModeloF)

<a name = "Modelagem"></a>
## Modelagem
[Voltar para o ínicio ↑](#Inicio)

<br>

<a name = "ModeloNegocial"></a>
### Modelo negocial

Para realizar as próximas etapas de modelagem, criei um documento exemplo de um cenário negocial, a través dele será possível compreender as necessidades do cliente e a partir disso criar os modelos.

Antes de seguir, leia com atenção todos os tópicos tratados no documento a seguir:

[https://docs.google.com/document/d/1FnD1x7mD9dfAUp4KDGpRvbq5n7Bjb6BJ/edit?usp=sharing&ouid=111849261700532294099&rtpof=true&sd=true](https://docs.google.com/document/d/1FnD1x7mD9dfAUp4KDGpRvbq5n7Bjb6BJ/edit?usp=sharing&ouid=111849261700532294099&rtpof=true&sd=true)

<br>

<a name = "ModeloLogico"></a>
### Modelo lógico

> **💡**: Para esta etapa, é necessário a ferramenta Oracle Data Modeler.

<a name = "UsarFerramenta"></a>
<b> Como usar a ferramenta (básico)</b>

Para começar a criação do modelo lógico, abra o Oracle Data Modeler

Com o programa aberto, clique na aba `Logical`

![Untitled](/Imagens/Untitled%20130.png)

Através do modelo negocial criarei as entidades de acordo com o documentado

Para criar uma nova entidade, clique em `Nova Entidade`

![Untitled](/Imagens/Untitled%20131.png)

Clique em qualquer lugar na parte branca para adicionar a nova entidade

De um nome qualquer

![Untitled](/Imagens/Untitled%20132.png)

Para adicionar os atributos da entidade, clique na aba `Atributos`

Clique no símbolo:

![Untitled](/Imagens/Untitled%20133.png)

Defina o nome, tipo de dados e tipo de origem dos dados

![Sem título.png](/Imagens/Sem_ttulo%206.png)

Para relacionar uma entidade com a outra, selecione o tipo de relacionamento

![Untitled](/Imagens/Untitled%20134.png)

Clique na primeira entidade e depois na segunda entidade, criando assim o relacionamento entre elas

![Untitled](/Imagens/Untitled%20135.png)

<br>

<a name = "ModeloL"></a>
<b>Modelo lógico</b>

![Logical.png](/Imagens/Logical.png)

<br>

<a name = "ModeloRelacional"></a>
### Modelo relacional

> **💡**: Para esta etapa, é necessário a ferramenta Oracle Data Modeler.

<br>

<a name = "ConversaoL-R"></a>
<b>Convertendo o modelo lógico para relacional</b>

Para converter, clique em `Engenharia para Modelo Relacional`

![Sem título.png](/Imagens/Sem_ttulo%207.png)

Clique em `Engenharia`

![Untitled](/Imagens/Untitled%20136.png)

Será gerado o modelo relacional, faça ajustes se necessário

![Untitled](/Imagens/Untitled%20137.png)

<br>

<a name = "ModeloR"></a>
<b>Modelo relacional</b>

![Relational_1.png](/Imagens/Relational_1.png)

<br>

<a name = "ModeloDimensional"></a>
### Modelo dimensional

> **💡**: Para esta etapa, é necessário a ferramenta Oracle Data Modeler.

<a name = "ModeloD"></a>
<b>Modelo dimensional</b>

Baseado no modelo relacional, montei o modelo dimensional

![Dimensional.png](/Imagens/Dimensional.png)

<br>

<a name = "ModeloFisico"></a>
### Modelo físico

> **💡**: Para esta etapa, é necessário a ferramenta Oracle Data Modeler.

<br>

<a name = "ConversaoD-F"></a>
<b>Convertendo o modelo dimensional para físico</b>

Para gerar o modelo físico, clique em `Gerar DDL`

![Untitled](/Imagens/Untitled%20138.png)

Escolha a versão `Oracle Database 21c`

Clique em `Gerar`

![Untitled](/Imagens/Untitled%20139.png)

Clique em `OK`

![Untitled](/Imagens/Untitled%20140.png)

Com o modelo físico criado, clique em `Salvar`

![Untitled](/Imagens/Untitled%20141.png)

Aparecerá essa janela depois de salvar

![Untitled](/Imagens/Untitled%20142.png)

<br>

<a name = "ModeloF"></a>
### Modelo físico

```sql
CREATE TABLE dim_cliente (
    sk_cliente         INTEGER NOT NULL,
    nk_id_cliente      VARCHAR2(20) NOT NULL,
    nm_cliente         VARCHAR2(50) NOT NULL,
    nm_cidade_cliente  VARCHAR2(50) NOT NULL,
    by_aceita_campanha CHAR(1) NOT NULL,
    desc_cep           VARCHAR2(10) NOT NULL
);

ALTER TABLE dim_cliente ADD CONSTRAINT dim_cliente_pk PRIMARY KEY ( sk_cliente );

CREATE TABLE dim_localidade (
    sk_localidade        INTEGER NOT NULL,
    nm_id_localidade     VARCHAR2(20) NOT NULL,
    nm_localidade        VARCHAR2(50) NOT NULL,
    nm_cidade_localidade VARCHAR2(50) NOT NULL,
    nm_regiao_localidade VARCHAR2(50) NOT NULL
);

ALTER TABLE dim_localidade ADD CONSTRAINT dim_localidade_pk PRIMARY KEY ( sk_localidade );

CREATE TABLE dim_produto (
    sk_produto           INTEGER NOT NULL,
    nk_id_produto        VARCHAR2(20) NOT NULL,
    desc_sku             VARCHAR2(50) NOT NULL,
    nm_produto           VARCHAR2(50) NOT NULL,
    nm_categoria_produto VARCHAR2(30) NOT NULL,
    nm_marca_produto     VARCHAR2(30) NOT NULL
);

ALTER TABLE dim_produto ADD CONSTRAINT dim_produto_pk PRIMARY KEY ( sk_produto );

CREATE TABLE dim_tempo (
    sk_data            INTEGER NOT NULL,
    data               DATE NOT NULL,
    desc_data_completa VARCHAR2(50) NOT NULL,
    nr_ano             INTEGER NOT NULL,
    nm_trimestre       VARCHAR2(50) NOT NULL,
    nr_mes             INTEGER NOT NULL,
    nr_semana          VARCHAR2(50) NOT NULL,
    nr_semana_1        INTEGER NOT NULL,
    nr_ano_semana      VARCHAR2(20) NOT NULL,
    nr_dia             INTEGER NOT NULL,
    nm_dia_semana      NUMBER NOT NULL,
    flag_feriado       CHAR(3) NOT NULL,
    nm_feriado         VARCHAR2(20) NOT NULL
);

ALTER TABLE dim_tempo ADD CONSTRAINT dim_tempo_pk PRIMARY KEY ( sk_data );

CREATE TABLE fato_venda (
    vl_venda                     NUMBER NOT NULL,
    qnt_venda                    INTEGER NOT NULL,
    dim_cliente_sk_cliente       INTEGER NOT NULL,
    dim_produto_sk_produto       INTEGER NOT NULL,
    dim_localidade_sk_localidade INTEGER NOT NULL,
    dim_tempo_sk_data            INTEGER NOT NULL
);

ALTER TABLE fato_venda
ADD CONSTRAINT fato_venda_dim_cliente_fk FOREIGN KEY ( dim_cliente_sk_cliente )
REFERENCES dim_cliente ( sk_cliente );

ALTER TABLE fato_venda
ADD CONSTRAINT fato_venda_dim_localidade_fk FOREIGN KEY ( dim_localidade_sk_localidade )
REFERENCES dim_localidade ( sk_localidade );

ALTER TABLE fato_venda
ADD CONSTRAINT fato_venda_dim_produto_fk FOREIGN KEY ( dim_produto_sk_produto )
REFERENCES dim_produto ( sk_produto );

ALTER TABLE fato_venda
ADD CONSTRAINT fato_venda_dim_tempo_fk FOREIGN KEY ( dim_tempo_sk_data )
REFERENCES dim_tempo ( sk_data );
```

Agora, com o modelo físico em “mãos”, é possível começar a implementação do Data Warehouse.