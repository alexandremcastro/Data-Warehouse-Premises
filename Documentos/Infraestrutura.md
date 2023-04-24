## Sumário
+ [Infraestrutura](#Infraestrutura)
    + [Preparando o Oracle VirtualBox](#PreparandoVirtualBox)
        + [Download](#DownloadVirtualBox)
        + [Instalação](#InstalacaoVirtualBox)
        + [Virtualização](#VirtualizacaoVirtualBox)
    + [Preparando o Oracle Linux](#PreparandoLinux)
        + [Download](#DownloadLinux)
        + [Instalação](#InstalacaoLinux)
        + [Configurações](#ConfiguracaoLinux)
            + [Arquivo Sudoers](#Sudoers)
            + [Arquivo Hosts](#Hosts)
            + [Atualização do SO](#Atualizacao)
            + [Desativando o Firewall](#Firewall)
    + [Preparando o Oracle Database](#PreparandoDatabase)
        + [Download](#DownloadDatabase)
        + [Instalação](#InstalacaoDatabase)
            + [Pre-install](#Preinstall)
            + [Instalação](#InstalacaoOracleDatabase)
            + [Criando o Listener](#Listener)
            + [Criando o Banco](#CriandoBanco)
        + [Acesso](#Acesso)
    + [Download do Oracle Data Modeler](#DownloadModeler)
    + [Download do Oracle SQL Developer](#DownloadDeveloper)
    + [Download do Pentaho Data Integration](#DownloadPDI)

<a name = "Infraestrutura"></a>
## Infraestrutura
[Voltar para o ínicio ↑](#Inicio)

<a name = "PreparandoVirtualBox"></a>
### Preparando o Oracle VirtualBox
<br>

<a name = "DownloadVirtualBox"></a>
<b>Download</b>

Acesse a página de download do Oracle VirtualBox

[Downloads – Oracle VM VirtualBox](https://www.virtualbox.org/wiki/Downloads)

![Untitled](/Imagens/Untitled.png)

Clique na plataforma que você está utilizando, no meu caso `Windows hosts`

![Untitled](/Imagens/Untitled%201.png)

Automaticamente o download começará

![Untitled](/Imagens/Untitled%202.png)

<br>

<a name = "InstalacaoVirtualBox"></a>
<b>Instalação</b>

> **💡**: Instale o pacote Microsoft Visual C++ 2019 Redistributable Package (x64)

Abra o instalador do baixado na etapa anterior

Clique em `Next`

![Untitled](/Imagens/Untitled%203.png)

Mantenha as opções padrões

Clique em `Next`

![Untitled](/Imagens/Untitled%204.png)

Clique em `Yes`

![Untitled](/Imagens/Untitled%205.png)

Clique em `Yes`

![Untitled](/Imagens/Untitled%206.png)

Clique em `Install`

![Untitled](/Imagens/Untitled%207.png)

Clique em `Finish`

![Untitled](/Imagens/Untitled%208.png)

Automaticamente será aberto o VirtualBox

![Untitled](/Imagens/Untitled%209.png)

<br>

<a name = "VirtualizacaoVirtualBox"></a>
<b>Virtualização</b>

Abra o VirtualBox

Para criar uma instância, clique em `Novo`

![Untitled](/Imagens/Untitled%2010.png)

De o nome de `Oracle Linux`, assim o programa já entenderá que a versão utilizada será do Oracle Linux

Clique em `Próximo (N)`

![Untitled](/Imagens/Untitled%2011.png)

Selecione a quantidade de memória que você pode fornecer para a sua máquina, o recomendado é 4 GB

Clique em `Próximo (N)`

![Untitled](/Imagens/Untitled%2012.png)

Selecione a quantidade de espaço em disco para ser virtualizada, o recomendado é 40 GB

Clique em `Próximo`

![Untitled](/Imagens/Untitled%2013.png)

Verifique se as configurações estão corretas

Clique em `Finalizar`

![Untitled](/Imagens/Untitled%2014.png)

Selecione a máquina criada

Clique em `Configurações`

![Untitled](/Imagens/Untitled%2015.png)

Nas configurações, clique em `Rede`

![Untitled](/Imagens/Untitled%2016.png)

Em rede, alterne a rede para `Placa em modo Bridge`

Clique em `OK`

![Untitled](/Imagens/Untitled%2017.png)

A máquina está pronta para receber o Linux

<br>

<a name = "PreparandoLinux"></a>
### Preparando o Oracle Linux

<br>

<a name = "DownloadLinux"></a>
<b>Download</b>

Acesse a página de download do Oracle Linux

[Oracle Linux ISOs | Oracle, Software. Hardware. Complete.](https://yum.oracle.com/oracle-linux-isos.html)

![Untitled](/Imagens/Untitled%2018.png)

A versão recomendada para a compatibilidade com o banco de dados é a Linux 8

Selecione a versão mais atualizada do Oracle Linux 8

![Untitled](/Imagens/Untitled%2019.png)

O download começará atomicamente 

![Untitled](/Imagens/Untitled%2020.png)

<a name = "InstalacaoLinux"></a>
<b>Instalação</b>

Para a instalação, abra o VirtualBox e inicie a máquina criada

Selecione a máquina e clique em `Iniciar`

![Sem título.png](/Imagens/Sem_ttulo.png)

Nessa janela, selecione a iso baixada do Oracle Linux

![Untitled](/Imagens/Untitled%2021.png)

Selecione a ISO e clique em `Abrir`

![Untitled](/Imagens/Untitled%2022.png)

Clique em `Montar e Tentar Novo Boot`

![Untitled](/Imagens/Untitled%2023.png)

Inicializando a instalação do Oracle Linux

Com as setas do teclado, vá na opção `Install Oracle Linux 8.7.0` e aperte `Enter`

![Untitled](/Imagens/Untitled%2024.png)

Selecione a linguagem desejada, no meu caso vou deixar no padrão (inglês)

Clique em `Continue`

![Untitled](/Imagens/Untitled%2025.png)

Configurando o tipo de instalação que será feita

Clique em `Software Selection`

![Sem título.png](/Imagens/Sem_ttulo%201.png)

Em `Base Enviroment`, selecione `Server with GUI`

Em `Additional software for Selected Environment`, selecione `Development Tools`

Clique em `Done`

![Untitled](/Imagens/Untitled%2026.png)

Configurando a localização que será instalado o Linux

Clique em `Installation Destination`

![Sem título.png](/Imagens/Sem_ttulo%202.png)

Selecionando o disco virtual para a instalação

Marque o disco e clique em `Done`

![Untitled](/Imagens/Untitled%2027.png)

Configurando a rede

Clique em `Network & Host Name`

![Untitled](/Imagens/Untitled%2028.png)

Habilite a conexão de internet, mova a chave para `ON`

![Sem título.png](/Imagens/Sem_ttulo%203.png)

Digite um nome para o Host Name

Clique em `Apply`

![Sem título.png](/Imagens/Sem_ttulo%204.png)

Verifique se foi alterado o Host Name

Clique em `Done`

![Sem título.png](/Imagens/Sem_ttulo%205.png)

Configurando a senha de administrador

Clique em `Root Password`

![Untitled](/Imagens/Untitled%2029.png)

Insira uma senha segura

Clique em `Done`

![Untitled](/Imagens/Untitled%2030.png)

Configurando o usuário da máquina

Clique em `User Creation`

![Untitled](/Imagens/Untitled%2031.png)

Escolha um nome de sua preferência

Escolha uma senha segura de sua preferência 

Clique em `Done`

![Untitled](/Imagens/Untitled%2032.png)

Para iniciar a instalação, clique em `Begin Installation`

![Untitled](/Imagens/Untitled%2033.png)

Aguarde a instalação até aparecer a opção de `Reboot System`

![Untitled](/Imagens/Untitled%2034.png)

Clique em `Reboot System`

![Untitled](/Imagens/Untitled%2035.png)

Depois de reiniciar, selecione a primeira opção e aperte `Enter`

![Untitled](/Imagens/Untitled%2036.png)

Para aceitar os termos de licença, clique em `Lincense Information`

![Untitled](/Imagens/Untitled%2037.png)

Aceite os termos e clique em `Done`

![Untitled](/Imagens/Untitled%2038.png)

Clique em `FINISH CONFIGURATION`

![Untitled](/Imagens/Untitled%2039.png)

Faça login no Linux

Selecione o usuário criado na instalação

![Untitled](/Imagens/Untitled%2040.png)

Digite a senha e clique em `Sign In`

![Untitled](/Imagens/Untitled%2041.png)

Instalação concluída

![Untitled](/Imagens/Untitled%2042.png)

<br>

<a name = "ConfiguracaoLinux"></a>
<b>Configurações</b>

<a name = "Sudoers"></a>
<b>Arquivo Sudoers</b>

Para ter privilégios de administrador no usuário criado, é necessário incluir esse usuário no arquivo sudoers

Clique em `Acitivities`

Clique em `Terminal`

![Untitled](/Imagens/Untitled%2043.png)

Para entrar no usuário `ROOT` use o comando

```bash
su -
```

Digite a senha solicitada

Para editar o arquivo sudoers, use o comando

```bash
vi /etc/sudoers
```

Procure pela seguinte linha:

```bash
## Allow root to run any commands anywhere
root    ALL=(ALL)    ALL
```

![Untitled](/Imagens/Untitled%2044.png)

Embaixo dessa linha, aperte `I` e insira:

```bash
nomedousuario    ALL=(ALL)    ALL
```

Aperte `ESC` e digite `:wq!` para gravar e fechar o arquivo

![Untitled](/Imagens/Untitled%2045.png)

Para sair do usuário `ROOT` utilize o comando:

```bash
exit
```
<a name = "Hosts"></a>
<b>Arquivo Hosts</b>

Para configurar os hosts é necessário saber o IP da máquina virtual, use o comando:

```bash
ifconfig
```

![Untitled](/Imagens/Untitled%2046.png)

No meu caso o IP é: `192.168.0.105`

Acessando e editando o arquivo hosts

```bash
sudo vi /etc/hosts
```

Insira o IP e um nome para identificar o IP ao lado, respeite os espaçamentos como os exemplos do próprio arquivo.

![Untitled](/Imagens/Untitled%2047.png)

No meu caso ficou: `192.168.0.105    castro`

<a name = "Atualizacao"></a>
<b>Atualização do SO</b>

Com o seu usuário, rode o comando:

```bash
sudo yum update -y
```

![Untitled](/Imagens/Untitled%2048.png)

<a name = "Firewall"></a>
<b>Desativando o Firewall</b>

Para desligar o firewall, insira o comando

```bash
systemctl stop firewalld
```

![Untitled](/Imagens/Untitled%2049.png)

Agora desabilite o firewall

```bash
systemctl disable firewalld
```

![Untitled](/Imagens/Untitled%2050.png)

<br>

<a name = "PreparandoDatabase"></a>
### Preparando o Oracle Database

<br>

<a name = "DownloadDatabase"></a>
<b>Download</b>

Acesse a página de download do Oracle Database

[Oracle Database 21c Download for Linux x86-64](https://www.oracle.com/database/technologies/oracle21c-linux-downloads.html)

![Untitled](/Imagens/Untitled%2051.png)

Para fazer o download, é necessário iniciar uma sessão na Oracle

Caso não possua uma conta, clique em `Criar Conta`

![Untitled](/Imagens/Untitled%2052.png)

Após iniciar a seção no site da Oracle, será iniciado o download automaticamente

![Untitled](/Imagens/Untitled%2053.png)

Verifique qual é o IP da máquina virtual

```bash
ifconfig
```

Após realizar o download, faça a transferência do arquivo para a máquina virtual utilizando o SSH, para isso abra o terminal e insira o comando

```bash
scp <diretório>\LINUX.X64_213000_db_home.zip <usuário>[@](mailto:castro@192.168.0.100)<ip>:/home/<usuario>
```

![Untitled](/Imagens/Untitled%2054.png)

Após terminar o envio do arquivo, verifique se chegou corretamente na máquina virtual com o comando:

```bash
ls
```

![Untitled](/Imagens/Untitled%2055.png)

<br>

<a name = "InstalacaoDatabase"></a>
<b>Instalação</b>

<br>

<a name = "Preinstall"></a>
<b>Pre-install</b>

Instalando o pacote preinstall, use o comando

```bash
yum search preinstall
```

![Untitled](/Imagens/Untitled%2056.png)

Para começar a pre-instalação do banco de dados, use o comando

```bash
sudo yum install oracle-database-preinstall-21c.x86_64 -y
```

Verifique se na pre-instalação foi criado o usuário oracle, use o comando

```bash
useradd oracle
```

![Untitled](/Imagens/Untitled%2057.png)

Verifique se na pre-instalação foi criado os grupos, use os comandos

```bash
groupadd oinstall
groupadd dba
groupadd oper
groupadd backupdba
groupadd dgdba
groupadd kmdba
groupadd racdba
```

Altere a senha do usuário `oracle`, use o comando

```bash
sudo passwd oracle
```

Crie o diretório do `oracle home`, use o comando

```bash
sudo mkdir -p /u01/app/oracle/product/21.3.0.0/dbhome_1
```

Entre no diretório para verificar a criação do diretório, use o comando

```bash
cd /u01/app/oracle/product/21.3.0.0/dbhome_1
```

Para sair do diretório, use o comando

```bash
cd
```

Mude as permissões de acesso do diretório para o usuário `oracle` e o grupo `oinstall`

```bash
sudo chown -R oracle:oinstall /u01
```

Para verificar a alteração de permissões, acesse o diretório `/` e use os comandos

```bash
cd /
ls -la
```

![Untitled](/Imagens/Untitled%2058.png)

Após isso, saia do diretório

Mova o arquivo de instalação para o diretório criado, use o comando

```bash
sudo mv LINUX.X64_213000_db_home.zip /u01/app/oracle/product/21.3.0.0/dbhome_1
```

Mude a permissão do arquivo de instalação para o usuário `oracle` e grupo `oinstall`

Acesse o diretório do instalador, use o comando

```bash
cd /u01/app/oracle/product/21.3.0.0/dbhome_1
```

Altere a permissão, use o comando

```bash
sudo chown -R oracle:oinstall LINUX.X64_213000_db_home.zip
```

Verifique a alteração, use o comando

```bash
ls -la
```

![Untitled](/Imagens/Untitled%2059.png)

Após isso, saia do diretório

Configurando as variáveis de ambiente

Acesse o usuário `oracle`, use o comando

```bash
su - oracle
```

![Untitled](/Imagens/Untitled%2060.png)

Entre no arquivo das variáveis de ambiente, use o comando

```bash
vi .bash_profile
```

Dentro do arquivo, insira as seguintes variáveis

```bash
export ORACLE_SID=orcl21c
export ORACLE_BASE=/u01/app/oracle
export ORACLE_HOME=/u01/app/oracle/product/21.3.0.0/dbhome_1
export ORACLE_UNQNAME=orcl21c

export NLS_LANG=AMERICAN_AMERICA.WE8ISO8859P1
#export LANG=us_EN.UTF-8

export ORACLE_OWNER=oracle
export ORACLE_TERM=xterm

export PATH=$ORACLE_HOME/bin:$ORA_CRS_HOME/bin:$PATH:/usr/local/bin
export LD_LIBRARY_PATH=$ORACLE_HOME/lib:$ORA_CRS_HOME/lib:/usr/local/lib:$LD_LIBRARY_PATH
export CLASSPATH=$ORACLE_HOME/JRE:$ORACLE_HOME/jlib:$ORACLE_HOME/rdbms/jlib

alias dba='sqlplus "/ as sysdba"'
```

![Untitled](/Imagens/Untitled%2061.png)

Grave e feche o arquivo

Atualize as variáveis de ambiente, use o comando

```bash
source .bash_profile
```

Teste as variáveis de ambiente

```bash
echo $ORACLE_SID
echo $ORACLE_HOME
```

![Untitled](/Imagens/Untitled%2062.png)

Descompactando o arquivo do instalador

Entre no diretório da instalação

```bash
cd $ORACLE_HOME
```

Faça a descompactação, use o comando

```bash
unzip LINUX.X64_213000_db_home.zip
```

> **💡**: A extração pode demorar alguns minutos dependendo da máquina.

Após de descompactar, é necessário informar para o instalador qual versão do Oracle Linux está sendo utilizado, para essa configuração, vá no diretório que foi descompactado e use o comando

```bash
vi cv/admin/cvu_config
```

Procure pela linha onde está comentado `CV_ASSUME_DISTID`, substitua por:

```bash
CV_ASSUME_DISTID=OEL8
```

![Untitled](/Imagens/Untitled%2063.png)

Grave e feche o arquivo

<br>

<a name = "InstalacaoOracleDatabase"></a>
<b>Instalação</b>

> **💡**: É necessário estar no usuário Oracle.

Para começar a instalação use o comando 

```bash
./runInstaller
```

![Untitled](/Imagens/Untitled%2064.png)

Selecione a primeira opção, pois futuramente criarei o banco de dados manualmente

Selecione a opção `Set Up Software Only` 

Clique em `Next`

![Untitled](/Imagens/Untitled%2065.png)

No momento como não estou lidando com clusters, marque a primeira opção

Selecione `Single instance database installation`

Clique em`Next`

![Untitled](/Imagens/Untitled%2066.png)

Para simular um ambiente real, optarei pela instalação `Enterprise Edition`

Selecione `Enterprise Edition`

Clique em `Next`

![Untitled](/Imagens/Untitled%2067.png)

Verifique se o `Oracle base` está no diretório `/u01/app/oracle`

Clique em `Next` 

![Untitled](/Imagens/Untitled%2068.png)

Verifique se o `Inventory Directory` está no diretório `/u01/app/oraInventory`

Clique em `Next`

![Untitled](/Imagens/Untitled%2069.png)

Verifique se os grupos mostrados foram os mesmos criados anteriormente no preinstall

Caso esteja correto, clique em `Next`

![Untitled](/Imagens/Untitled%2070.png)

Marque a opção: `Automatically run configuration scripts`

Marque a opção: `use “root” user credential`

Crie uma senha e clique em `Next`

![Untitled](/Imagens/Untitled%2071.png)

Essa aba mostra um overview sobre a instalação, verifique se está tudo correto

Clique em `Install`

![Untitled](/Imagens/Untitled%2072.png)

Essa aba pergunta se vai querer que rode os scripts de configurações durante a instalação

Clique em `Yes`

![Untitled](/Imagens/Untitled%2073.png)

Instalação finalizada

Clique em `Close`

![Untitled](/Imagens/Untitled%2074.png)

<br>

<a name = "Listener"></a>
<b>Criando o Listener</b>

> **💡**: É necessário estar no usuário Oracle

Para criar o Listener, use o comando

```bash
netca
```

![Untitled](/Imagens/Untitled%2075.png)

Para adicionar um Listener, clique em `Add`

![Untitled](/Imagens/Untitled%2076.png)

Escolha um nome, recomendo deixar por padrão `LISTENER`

![Untitled](/Imagens/Untitled%2077.png)

Mantenha o padrão e clique em `Add`

![Untitled](/Imagens/Untitled%2078.png)

Mantenha a primeira opção (porta padrão)

Clique em `Next`

![Untitled](/Imagens/Untitled%2079.png)

Irei apenas configurar um listener Marque `No`

Clique em `Next`

![Untitled](/Imagens/Untitled%2080.png)

Clique em `Next`

![Untitled](/Imagens/Untitled%2081.png)

Finalizado a criação do Listener

Clique em `Finish`

![Untitled](/Imagens/Untitled%2082.png)

Para verificar o status do Listener, use o comando

```bash
lsnrctl status
```

![Untitled](/Imagens/Untitled%2083.png)

<br>

<a name = "CriandoBanco"></a>
<b>Criando o Banco</b>

> **💡**: É necessário estar no usuário Oracle

Para iniciar o setup para criar o banco de dados, execute

```bash
dbca
```

![Untitled](/Imagens/Untitled%2084.png)

![Untitled](/Imagens/Untitled%2085.png)

![Untitled](/Imagens/Untitled%2086.png)

![Untitled](/Imagens/Untitled%2087.png)

![Untitled](/Imagens/Untitled%2088.png)

![Untitled](/Imagens/Untitled%2089.png)

![Untitled](/Imagens/Untitled%2090.png)

![Untitled](/Imagens/Untitled%2091.png)

![Untitled](/Imagens/Untitled%2092.png)

![Untitled](/Imagens/Untitled%2093.png)

![Untitled](/Imagens/Untitled%2094.png)

![Untitled](/Imagens/Untitled%2095.png)

![Untitled](/Imagens/Untitled%2096.png)

![Untitled](/Imagens/Untitled%2097.png)

![Untitled](/Imagens/Untitled%2098.png)

<br>

<a name = "Acesso"></a>
<b>Acesso</b>

> **💡**: É necessário estar no usuário Oracle

Para acessar o banco de dados via terminal, abra o terminal, use o comando

```bash
sqlplus / as sysdba
```

![Untitled](/Imagens/Untitled%2099.png)

<br>

<a name = "DownloadModeler"></a>
### Download do Oracle Data Modeler

Acesse a página do Oracle Data Modeler

[Ferramentas de Modelagem de Dados | Oracle SQL Developer Data Modeler | Oracle Brasil](https://www.oracle.com/br/database/sqldeveloper/technologies/sql-data-modeler/)

![Captura da Web_17-3-2023_988_www.oracle.com.jpeg](/Imagens/Captura_da_Web_17-3-2023_988_www.oracle.com.jpeg)

Clique em `Fazer o download`

![Untitled](/Imagens/Untitled%20100.png)

Procure pela plataforma que você está utilizando

Clique em `Download`

![Untitled](/Imagens/Untitled%20101.png)

Aceite os termos de licença da Oracle

Clique em `Download <versão>.zip`

![Untitled](/Imagens/Untitled%20102.png)

Para fazer o download, é necessário iniciar uma sessão na Oracle

Caso não possua uma conta, clique em `Criar Conta`

![Untitled](/Imagens/Untitled%2052.png)

Após iniciar a seção no site da Oracle, será iniciado o download automaticamente

![Untitled](/Imagens/Untitled%20103.png)

Ao terminar de baixar, faça a extração dos arquivos para um local que deseja deixar salvo

![Untitled](/Imagens/Untitled%20104.png)

Abra o diretório que foi extraído

Abra o aplicativo `datamodeler`

![Untitled](/Imagens/Untitled%20105.png)

> **💡**: Se o datamodeler não abrir, instale o JDK 11, pois ele é feito na linguagem Java.

Essa é a interface usada para criar os modelos

![Untitled](/Imagens/Untitled%20106.png)

<br>

<a name = "DownloadDeveloper"></a>
### Download do Oracle SQL Developer

Acesse a página do Oracle SQL Developer

[SQL Developer](https://www.oracle.com/database/sqldeveloper/)

Clique em `Fazer o download`

![Untitled](/Imagens/Untitled%20107.png)

Procure pela plataforma que você está utilizando

Clique em `Download`

![Untitled](/Imagens/Untitled%20108.png)

Aceite os termos de licença da Oracle

Clique em `Download <versão>.zip`

![Untitled](/Imagens/Untitled%20102.png)

Para fazer o download, é necessário iniciar uma sessão na Oracle

Caso não possua uma conta, clique em `Criar Conta`

![Untitled](/Imagens/Untitled%2052.png)

Após iniciar a seção no site da Oracle, será iniciado o download automaticamente

![Untitled](/Imagens/Untitled%20109.png)

Ao terminar de baixar, faça a extração dos arquivos para um local que deseja deixar salvo

![Untitled](/Imagens/Untitled%20110.png)

Abra o diretório que foi extraído

Abra o aplicativo `sqldeveloper`

![Untitled](/Imagens/Untitled%20111.png)

> **💡**: Se o SQL Developer não abrir, instale o JDK 11, pois ele é feito na linguagem Java.

Essa é a interface usada para administrar o banco de dados

![Untitled](/Imagens/Untitled%20112.png)

Para acessar o banco de dados, com o botão direito do mouse, clique em `Oracle Conexões`

Clique em `Nova Conexão…`

![Untitled](/Imagens/Untitled%20113.png)

Em `Name`, insira qualquer nome, este campo serve apenas para identificar essa conexão

Em `Nome do Usuário`, preencha com o `sys`

Em `Senha`, preencha com a senha criada na criação do banco de dados

Em `Atribuição`, selecione `SYSDBA`

Em `Nome do Host`, insira o IP da máquina virtual

Em `Porta`, insira a porta criada no Listener, se for a padrão, insira `1521`

Em `SID`, insira o nome da instancia criada anteriormente, `orcl21c`

Clique em `Testar`

![Untitled](/Imagens/Untitled%20114.png)

Verifique se o status da conexão foi feita com sucesso.

Para se conectar no banco de dados, clique em `Conectar`

![Untitled](/Imagens/Untitled%20115.png)

Se tudo der certo, você será redirecionado para a tela de construção de query

![Untitled](/Imagens/Untitled%20116.png)

Com isso, toda infraestrutura está preparada para receber as modelagens.

<br>

<a name = "DownloadPDI"></a>
### Download do Pentaho Data Integration

> **💡**: Para rodar o PDI é necessário o Java JDK instalado.

Antes de instalar o PDI, é necessário configurar as variáveis de ambiente do sistema operacional

Vá em `Editar as variáveis de ambiente do sistema`

![Untitled](/Imagens/Untitled%20117.png)

Clique em `Variáveis de ambiente`

![Untitled](/Imagens/Untitled%20118.png)

Em variáveis de usuário para `<usuário>`, clique em `Novo`

![Untitled](/Imagens/Untitled%20119.png)

Crie a variável de ambiente do `JAVA_HOME`

No valor da variável, coloque o diretório da instalação do Java JDK

![Untitled](/Imagens/Untitled%20120.png)

Crie a variável de ambiente do `PENTAHO_JAVA_HOME`

No valor da variável, coloque o diretório da instalação do Java JDK

![Untitled](/Imagens/Untitled%20121.png)

Crie a variável de ambiente do `PENTAHO_JAVA`

No valor da variável, coloque o arquivo `Java.exe`, que está dentro do diretório da instalação do Java JDK

![Untitled](/Imagens/Untitled%20122.png)

Acesse a página de download do Pentaho Data Integration

[Pentaho Community Edition Download](https://www.hitachivantara.com/en-us/products/dataops-software/data-integration-analytics/pentaho-community-edition.html)

Para abrir as opções de download, clique em `Download Now`

![Untitled](/Imagens/Untitled%20123.png)

Na opção `pdi-ce-9.4.0.0-343.zip`, clique em Donwload

![Untitled](/Imagens/Untitled%20124.png)

Será iniciado o download

![Untitled](/Imagens/Untitled%20125.png)

Ao terminar de baixar, faça a extração dos arquivos para um local que deseja deixar salvo

![Untitled](/Imagens/Untitled%20126.png)

Abra o diretório que foi extraído

Abra o aplicativo Spoon.bat

![Untitled](/Imagens/Untitled%20127.png)

Se tudo foi configurado corretamente, esta janela de inicialização aparecerá

![Untitled](/Imagens/Untitled%20128.png)

Instalação finalizada!

![Untitled](/Imagens/Untitled%20129.png)