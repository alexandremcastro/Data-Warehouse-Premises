
# Data Warehouse On-Premises
[![GitHub license](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/alexandremcastro/Data-Warehouse-Premises/blob/main/LICENSE)

Um projeto de Data Warehouse com Oracle Database é um esforço para criar um ambiente de armazenamento de dados centralizado e integrado que possa suportar as necessidades de análise de negócios de uma organização. O objetivo final é fornecer aos usuários de negócios um acesso fácil e rápido a informações precisas e relevantes para que possam tomar decisões mais informadas e eficazes.

O projeto começa com a identificação dos requisitos de negócios e dos dados que precisam ser armazenados e analisados. Em seguida, são criados modelos de dados que definem as tabelas, colunas e relacionamentos necessários para armazenar e integrar os dados. Os modelos de dados são projetados para ser escalável e permitir a adição de novos dados e análises conforme as necessidades do negócio evoluem.

Uma vez que o modelo de dados é definido, o próximo passo é a construção do data warehouse em si. O Oracle Database é uma das opções de banco de dados mais populares para Data Warehousing, devido à sua capacidade de processamento de grandes volumes de dados e à sua capacidade de escalabilidade. A arquitetura do Data Warehouse pode ser baseada em um modelo de camadas, onde os dados são carregados em uma camada de aterramento (staging layer) antes de serem transformados e carregados em camadas subsequentes. A camada final é a camada de apresentação, onde os usuários de negócios acessam os dados por meio de ferramentas de visualização ou de relatórios.

Um aspecto crítico de qualquer projeto de data warehouse é o processo de ETL (Extração, Transformação e Carregamento), que envolve a extração dos dados de várias fontes, a transformação para garantir a integridade e qualidade dos dados, e o carregamento nos repositórios de dados do data warehouse.

Por fim, o Data Warehouse é mantido por meio de atividades regulares de administração e manutenção, como monitoramento de desempenho, backup e recuperação de dados e gerenciamento de usuários e segurança.

Em resumo, um projeto de Data Warehouse com Oracle Database é uma iniciativa complexa que requer a definição cuidadosa de requisitos de negócios, a criação de um modelo de dados escalável, a construção de um Data Warehouse robusto e o gerenciamento contínuo do ambiente de armazenamento de dados.

## Ferramentas utilizadas
> **📝 Nota**: Este projeto foi concluído em Janeiro de 2023, e é importante ressaltar que alguns dos requisitos abordados podem sofrer alterações ao longo do tempo. Como em qualquer área de tecnologia, novas soluções e tecnologias surgem constantemente, o que pode tornar algumas das abordagens e soluções apresentadas neste projeto desatualizadas em algum momento.

1. [Oracle VM VirtualBox](https://www.virtualbox.org/): Fornece a virtualização de máquinas para simulação de um ambiente on-premises.
2. [Oracle Linux](https://www.oracle.com/br/linux/): Sistema operacional fornecido pela Oracle e otimizado para as suas aplicações.
3. [Oracle Data Modeler](https://www.oracle.com/br/database/sqldeveloper/technologies/sql-data-modeler/): Fornece um ambiente para criar os modelos (*lógico, dimensional e físico*).
4. [Oracle Database](https://www.oracle.com/database/): Fornece um banco de dados relacional.
5. [Oracle SQL Developer](https://www.oracle.com/database/sqldeveloper/): Fornece um ambiente para acessar o banco de dados e realizar consultas em SQL.
6. [Pentaho Data Integration](https://help.hitachivantara.com/Documentation/Pentaho/8.3/Products/Pentaho_Data_Integration): Ferramenta para realizar a integração de dados.

## Motivação
Este projeto experimental é o resultado das minhas experiências com Data Warehouse. Durante o processo de criação, tive a oportunidade de explorar as técnicas mais recentes de armazenamento e análise de dados em larga escala.

Ao longo do desenvolvimento, trabalhei com diversas ferramentas e tecnologias, incluindo plataformas de integração de dados, banco de dados, modelagem de dados e desenvolvimento de queries.

## Sumário
+ [Infraestrutura](https://github.com/alexandremcastro/Data-Warehouse-Premises/blob/main/Documentos/Infraestrutura.md#Infraestrutura)
    + [Preparando o Oracle VirtualBox](https://github.com/alexandremcastro/Data-Warehouse-Premises/blob/main/Documentos/Infraestrutura.md#PreparandoVirtualBox)
    + [Preparando o Oracle Linux](https://github.com/alexandremcastro/Data-Warehouse-Premises/blob/main/Documentos/Infraestrutura.md#PreparandoLinux)
    + [Preparando o Oracle Database](https://github.com/alexandremcastro/Data-Warehouse-Premises/blob/main/Documentos/Infraestrutura.md#PreparandoDatabase)
    + [Download do Oracle Data Modeler](https://github.com/alexandremcastro/Data-Warehouse-Premises/blob/main/Documentos/Infraestrutura.md#DownloadModeler)
    + [Download do Oracle SQL Developer](https://github.com/alexandremcastro/Data-Warehouse-Premises/blob/main/Documentos/Infraestrutura.md#DownloadDeveloper)
    + [Download do Pentaho Data Integration](https://github.com/alexandremcastro/Data-Warehouse-Premises/blob/main/Documentos/Infraestrutura.md#DownloadPDI)
+ [Modelagem](https://github.com/alexandremcastro/Data-Warehouse-Premises/blob/main/Documentos/Modelagem.md#Modelagem)
    + [Modelo negocial](https://github.com/alexandremcastro/Data-Warehouse-Premises/blob/main/Documentos/Modelagem.md#ModeloNegocial)
    + [Modelo lógico](https://github.com/alexandremcastro/Data-Warehouse-Premises/blob/main/Documentos/Modelagem.md#ModeloLogico)
    + [Modelo relacional](https://github.com/alexandremcastro/Data-Warehouse-Premises/blob/main/Documentos/Modelagem.md#ModeloRelacional)
    + [Modelo dimensional](https://github.com/alexandremcastro/Data-Warehouse-Premises/blob/main/Documentos/Modelagem.md#ModeloDimensional)
    + [Modelo físico](https://github.com/alexandremcastro/Data-Warehouse-Premises/blob/main/Documentos/Modelagem.md#ModeloFisico)
+ [Sistema Legado/Staging Area](https://github.com/alexandremcastro/Data-Warehouse-Premises/blob/main/Documentos/LegadoStage.md#LegadoStaging)
    + [Simulando o sistema legado](https://github.com/alexandremcastro/Data-Warehouse-Premises/blob/main/Documentos/LegadoStage.md#Legado)
    + [Criando a Staging Area](https://github.com/alexandremcastro/Data-Warehouse-Premises/blob/main/Documentos/LegadoStage.md#Staging)
    + [Criando as conexões com o banco](https://github.com/alexandremcastro/Data-Warehouse-Premises/blob/main/Documentos/LegadoStage.md#Conexoes)
    + [Transformações](https://github.com/alexandremcastro/Data-Warehouse-Premises/blob/main/Documentos/LegadoStage.md#Transformacoes)
+ [Implementação](https://github.com/alexandremcastro/Data-Warehouse-Premises/blob/main/Documentos/Implementacao.md#Implementacao)
    + [Criando o Data Warehouse](https://github.com/alexandremcastro/Data-Warehouse-Premises/blob/main/Documentos/Implementacao.md#DataWarehouse)
    + [Carga de dados](https://github.com/alexandremcastro/Data-Warehouse-Premises/blob/main/Documentos/Implementacao.md#CargaDados)
    + [Verificando a integridade de dados](https://github.com/alexandremcastro/Data-Warehouse-Premises/blob/main/Documentos/Implementacao.md#Integridade)

## Requisitos
- Hardware: O Oracle Database requer um hardware de servidor robusto com um mínimo de 2 GB de RAM para versões mais antigas e pelo menos 4 GB para versões mais recentes. É recomendável ter um processador multicore com, pelo menos, 4 núcleos e uma unidade de disco rígido com, no mínimo, 40 GB de espaço livre.

- Sistema operacional: O Oracle Database é executado em várias plataformas de sistema operacional, incluindo Linux, Windows, Solaris e AIX. Verifique se o sistema operacional é compatível com a versão do Oracle Database que deseja instalar.

- Software adicional: O Oracle Database requer a instalação de software adicional, como o Oracle Client, o Oracle Net Services, o Oracle Enterprise Manager e o Java Runtime Environment (JRE).

- Espaço em disco: O espaço em disco requerido para instalar o Oracle Database varia de acordo com a versão e as opções selecionadas durante a instalação. No entanto, é recomendável ter pelo menos 10 GB de espaço livre no disco rígido.

- Rede: O Oracle Database requer uma rede adequada para comunicação com outras máquinas e para acesso remoto. Verifique se a rede suporta as configurações de rede necessárias para a instalação do Oracle Database.

## Tarefas
**Tarefas concluídas**

- [x] Descrever sobre a finalidade do Data Warehouse
- [x] Instalar as ferramentas necessárias para o projeto
- [x] Simular um ambiente On-Premises
- [x] Criar os modelos (negocial, lógico, relacional, dimensional e físico)
- [x] Implementar e integrar os dados no Data Warehouse
- [x] Verificar integridade dos dados

**Futuras implementações**

- [ ] Simular mais necessidades do cliente
- [ ] Modificar as tabelas de acordo com as novas necessidades
- [ ] Simular mais cargas de dados diárias para popular mais ainda o Data Warehouse
- [ ] Criar um dashboard com os insights que serão possíveis com adição de novos dados

## Links
**Projeto:**

* [Projeto no site](https://alexandre-castro.vercel.app/blog/datawarehouse-premises)

* [Projeto no Notion](https://alexandremcastro.notion.site/01-2023-Data-Warehouse-On-Premises-25b04fc48a6043b186b66f6cedf9a19d)

* [Projeto no GitLab](https://gitlab.com/alexandremcastro/Data-Warehouse-Premises)

* [Projeto no GitHub](https://github.com/alexandremcastro/Data-Warehouse-Premises)

**Artigos orientadores:**

* [O que é um Data Warehouse - SAP](https://www.sap.com/brazil/insights/what-is-a-data-warehouse.html)

* [Conceitos de Data Warehouse - AWS](https://aws.amazon.com/pt/data-warehouse/)

* [Data Warehousing in Microsoft Azure - Microsoft](https://learn.microsoft.com/en-us/azure/architecture/data-guide/relational-data/data-warehousing)

* [Conceitos Básicos Sobre OLAP - DevMedia](https://www.devmedia.com.br/conceitos-basicos-sobre-olap/12523)

## Conclusão
O objetivo principal deste projeto experimental foi demonstrar a eficácia e o potencial do Data Warehouse para empresas e organizações que lidam com grandes volumes de dados. Para alcançar esse objetivo, simulei dados reais de um cenário real, possibilitanto a análise e interpretação desses dados.

Este projeto foi uma experiência enriquecedora e me permitiu adquirir conhecimentos valiosos sobre a arquitetura e implementação de soluções de Data Warehouse. Espero que esta iniciativa possa ser útil para outras pessoas e organizações que buscam explorar o potencial do armazenamento e análise de dados.