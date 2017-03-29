Gerenciador de vendas.
===================

Sistema desenvolvido com objetivo em gerenciar vendas. 
Adequado para despache de combustíveis. 

----------
**Índice**
 1. [Documentação.](#documentação)
 2. [Screenshots.](#screenshots)
 3. [Vídeo.](#vídeo)
 3. [Banco de Dados.](#banco-de-dados)
 4. [Instalação.](#instalação)
 5. [Scripts Backup.](#scripts-backup)

----------


Documentação
---------------------------------------------------------------------------

Em seu desenvolvimento foi utilizado o [NetBeans IDE](https://netbeans.org/), para programação da linguagem **Java**. Juntamente utilizando [iReport Designer](http://community.jaspersoft.com/project/ireport-designer) para desenhar os seus relatórios. Em seu armazenamento de dados foi aderido ao [PostgreSQL](https://www.postgresql.org/). Projeto estruturado ao padrão [MVC (Model-View-Controller)](https://pt.wikipedia.org/wiki/MVC), para melhor controle do projeto. E não menos importando, controle de versionamento pelo [GitHub](https://github.com/PauloJoseCagol/).



> **Notas**
> - **Kernel** *x86_64 Linux 4.10.4-1-ARCH*
> - *NetBeans IDE versão 8.1.*
> - [*iReport Plugin 5.5.0*](http://plugins.netbeans.org/plugin/4425/ireport)
> - *PostgreSQL 9.6.1 on x86_64-pc-linux-gnu, 64-bit.*


UML
Caso de uso (Venda/Pagamento).
![Venda/Pagamento](https://cldup.com/LYKsaPIocs.png)


----------
Vídeo
---------------------------------------------------------------------------
Vídeo demonstrativo.

[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/udEqGnbm1dU/0.jpg)](https://www.youtube.com/watch?v=udEqGnbm1dU)


----------
Screenshots
---------------------------------------------------------------------------
Algumas das telas do projeto.

>Telas.
![Bem vindo.](https://cldup.com/C-ACRO_-AA.png)
![Tela Principal.](https://cldup.com/B01FUQM4u8-2000x2000.png)

![Cadastro de Clientes.](https://cldup.com/MD1Vli0caP-3000x3000.png)
![Cadastro de Produtos.](https://cldup.com/GR4l7VNFD4-3000x3000.png)

![Módulo de vendas.](https://cldup.com/rkhdknmQDc-3000x3000.png)
![Módulo de Contas.](https://cldup.com/jWkjRb2aWG-3000x3000.png)

![Contas em aberto.](https://cldup.com/D9GRi0w-tl-3000x3000.png)
![Contas de um cliente.](https://cldup.com/IDNfDC0_HL-3000x3000.png)

----------

Banco de Dados
---------------------------------------------------------------------------
Creates
```sql
CREATE DATABASE posto
  WITH OWNER = postgres
       ENCODING = 'UTF8'
       TABLESPACE = pg_default
       LC_COLLATE = 'Portuguese_Brazil.1252'
       LC_CTYPE = 'Portuguese_Brazil.1252'
       CONNECTION LIMIT = -1;
       
CREATE TABLE enderecos(
  id serial primary key,
  descricao character varying(255),
  dt_insert timestamp without time zone,
  dt_update timestamp without time zone
);

CREATE TABLE clientes(
  id serial primary key,
  nome character varying(255) NOT NULL,
  cpf character varying(255),
  telefone character varying(255),
  endereco_id integer,
  CONSTRAINT enderecos_fk FOREIGN KEY (endereco_id) REFERENCES public.enderecos (id)   
);

CREATE TABLE vendas(
  id SERIAL PRIMARY KEY,
  data_venda date NOT NULL,
  valor_total double precision NOT NULL,
  dt_insert timestamp without time zone,
  dt_update timestamp without time zone,
  pago integer NOT NULL,
  cliente_id integer,
  CONSTRAINT cliente_fk FOREIGN KEY (cliente_id) REFERENCES public.clientes (id)

);
CREATE TABLE vendas_pagas(
id SERIAL,
valor DOUBLE PRECISION,
cancelada INTEGER,
venda_id INTEGER NOT NULL,
cliente_id INTEGER NOT NULL,
dt_insert timestamp without time zone,
dt_update timestamp without time zone,
CONSTRAINT vendas_fk FOREIGN KEY (venda_id) REFERENCES vendas(id),
CONSTRAINT clientes_fk FOREIGN KEY (cliente_id) REFERENCES clientes(id)
);

CREATE TABLE produtos(
  id serial primary key,
  nome character varying(255) NOT NULL,
  descricao character varying(255),
  preco_compra double precision NOT NULL,
  data_compra date NOT NULL,
  dt_insert timestamp without time zone,
  dt_update timestamp without time zone,
  qtd_litros double precision NOT NULL,
  total double precision NOT NULL
);

CREATE TABLE veiculos(
  id serial primary key,
  descricao character varying(255) NOT NULL,
  placa character varying(255) NOT NULL
);

CREATE TABLE itens_venda_produto(
  id SERIAL PRIMARY KEY,
  quantidade double precision NOT NULL,
  dt_insert timestamp without time zone,
  dt_update timestamp without time zone,
  venda_id integer,
  produto_id integer,
  veiculo_id integer,
  valor_unitario double precision,
  CONSTRAINT produto_fk FOREIGN KEY (produto_id) REFERENCES public.produtos (id),
  CONSTRAINT venda_fk FOREIGN KEY (venda_id) REFERENCES public.vendas (id),
  CONSTRAINT veiculo_fk FOREIGN KEY (veiculo_id) REFERENCES veiculos(id)
);
```


----------

Instalação
---------------------------------------------------------------------------

>**Requisitos**
> 1. Sistema Operacional Windows ou Linux.
> 2. [JRE 8 (Java SE Runtime Environment 8)](http://www.oracle.com/technetwork/java/javase/downloads/jre8-downloads-2133155.html) Plataforma de execução.
> 3. [PostgreSQL (Locale pt_BR)](https://www.postgresql.org/download/) Armazenamento de dados.
> 4. [BitBuket](https://bitbucket.org/paulojosecagol/projeto-posto/) Repositório privado do projeto.

Arquitetura do diretório
![Diretórios](https://cldup.com/ehPGsKBxfx.png)


----------
Scripts Backup
---------------------------------------------------------------------------

```bat
@ECHO OFF
@CLS
@COLOR 02
@FOR /F "tokens=1-4 delims=/ " %%I IN ('DATE /t') DO SET data=%%L-%%K-%%J
@FOR /F "TOKENS=1-2* delims=:" %%A IN ('TIME /t') DO SET hora=%%Ahr%%Bmin
@SET caminho=C:\Posto\dist\Backup\
@SET database=posto

@ECHO =======================================================
@ECHO BACKUP POSTO WINDOWS VERSION
@ECHO =======================================================
@ECHO.
@ECHO.
@ECHO.
@ECHO Arquivo de Backup: %caminho%%database%_%data%_%hora%.sql
@ECHO.
@ECHO.

@SET PGPASSWORD=postgres

@c:
@cd C:\Program Files\PostgreSQL\9.6\bin

@pg_dump.exe -d %database% --inserts -E UTF8 -U postgres > %caminho%%database%_%data%_%hora%.sql

@if not exist %caminho%%database%_%data%_%hora%.sql goto MESSAGE

@ECHO *******************************************
@ECHO BACKUP REALIZADO COM SUCESSO !!!!
@ECHO *******************************************
@ECHO.

goto END
:MESSAGE

@ECHO *******************************************
@ECHO ATENCAO: ERRO NA CRIACAO DO BACKUP !
@ECHO *******************************************
@ECHO.
PAUSE
:END
```
