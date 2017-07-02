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
Caso de uso (Venda).
![Venda/Pagamento](https://cldup.com/WGlnnie5HW-3000x3000.png)


----------
Vídeo
---------------------------------------------------------------------------
Vídeo demonstrativo.
~#1
[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/udEqGnbm1dU/0.jpg)](https://www.youtube.com/watch?v=udEqGnbm1dU)

~#2
[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/Eyon3LG9D-4/0.jpg)](https://www.youtube.com/watch?v=Eyon3LG9D-4)


----------
Screenshots
---------------------------------------------------------------------------
Algumas das telas do projeto.

>Telas.
![Bem vindo.](https://cldup.com/T2vLvxslWH-3000x3000.png)

![Tela Principal.](https://cldup.com/UzB81ak-GI-3000x3000.png)

![Cadastro de Clientes.](https://cldup.com/m9hwxMmAuP-3000x3000.png)

![Cadastro de Produtos.](https://cldup.com/KOumrmu9Wp-3000x3000.png)

![Módulo de vendas.](https://cldup.com/LabHiYXsvT-1200x1200.png)

![Módulo de Contas.](https://cldup.com/lQ6wS9TjTu-1200x1200.png)

![Detalhamento de Venda.](https://cldup.com/DhOmYLmK7r-3000x3000.png)

![Movimento.](https://cldup.com/G-AIqhPqcO-3000x3000.png)

![Configuração de Backup.](https://cldup.com/vma0RsSw9C-3000x3000.png)

----------

Banco de Dados
---------------------------------------------------------------------------
Creates
```sql
SET statement_timeout = 0;
SET lock_timeout = 0;
SET idle_in_transaction_session_timeout = 0;
SET client_encoding = 'UTF8';
SET standard_conforming_strings = on;
SET check_function_bodies = false;
SET client_min_messages = warning;
SET row_security = off;

CREATE EXTENSION IF NOT EXISTS plpgsql WITH SCHEMA pg_catalog;

COMMENT ON EXTENSION plpgsql IS 'PL/pgSQL procedural language';

SET search_path = public, pg_catalog;

SET default_tablespace = '';

SET default_with_oids = false;
--
-- TOC entry 185 (class 1259 OID 33757)
-- Name: backup; Type: TABLE; Schema: public; Owner: postgres
--

CREATE TABLE backup (
    id integer NOT NULL,
    caminho_pg character varying(255),
    caminho_backup character varying(255),
    tempo bigint
);

INSERT INTO backup VALUES(1, '','', 60);

ALTER TABLE backup OWNER TO postgres;

CREATE TABLE cidades (
    id integer NOT NULL,
    nome character varying(255),
    estado_id integer,
    dt_insert timestamp without time zone,
    dt_update timestamp without time zone
);

ALTER TABLE cidades OWNER TO postgres;

CREATE SEQUENCE cidades_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;

ALTER TABLE cidades_id_seq OWNER TO postgres;

ALTER SEQUENCE cidades_id_seq OWNED BY cidades.id;

CREATE TABLE clientes (
    id integer NOT NULL,
    nome character varying(255) NOT NULL,
    cpf character varying(255),
    telefone character varying(255),
    cidade_id integer,
    endereco character varying(255)
);

ALTER TABLE clientes OWNER TO postgres;

CREATE SEQUENCE clientes_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;

ALTER TABLE clientes_id_seq OWNER TO postgres;

ALTER SEQUENCE clientes_id_seq OWNED BY clientes.id;

CREATE TABLE estados (
    id integer NOT NULL,
    nome character varying(255),
    uf character varying(5)
);

INSERT INTO estados (id, nome, uf) VALUES
(1, 'Acre', 'AC'),
(2, 'Alagoas', 'AL'),
(3, 'Amazonas', 'AM'),
(4, 'Amapá', 'AP'),
(5, 'Bahia', 'BA'),
(6, 'Ceará', 'CE'),
(7, 'Distrito Federal', 'DF'),
(8, 'Espírito Santo', 'ES'),
(9, 'Goiás', 'GO'),
(10, 'Maranhão', 'MA'),
(11, 'Minas Gerais', 'MG'),
(12, 'Mato Grosso do Sul', 'MS'),
(13, 'Mato Grosso', 'MT'),
(14, 'Pará', 'PA'),
(15, 'Paraíba', 'PB'),
(16, 'Pernambuco', 'PE'),
(17, 'Piauí', 'PI'),
(18, 'Paraná', 'PR'),
(19, 'Rio de Janeiro', 'RJ'),
(20, 'Rio Grande do Norte', 'RN'),
(21, 'Rondônia', 'RO'),
(22, 'Roraima', 'RR'),
(23, 'Rio Grande do Sul', 'RS'),
(24, 'Santa Catarina', 'SC'),
(25, 'Sergipe', 'SE'),
(26, 'São Paulo', 'SP'),
(27, 'Tocantins', 'TO');

ALTER TABLE estados OWNER TO postgres;

CREATE SEQUENCE estados_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;

ALTER TABLE estados_id_seq OWNER TO postgres;

ALTER SEQUENCE estados_id_seq OWNED BY estados.id;

CREATE TABLE item_venda_produto_usados (
    id integer NOT NULL,
    qtd_litros_usados double precision NOT NULL,
    produto_id integer NOT NULL,
    venda_id integer NOT NULL
);

ALTER TABLE item_venda_produto_usados OWNER TO postgres;

ALTER TABLE item_venda_produto_usados_id_seq OWNER TO postgres;

ALTER SEQUENCE item_venda_produto_usados_id_seq OWNED BY item_venda_produto_usados.id;

CREATE TABLE itens_venda_produto (
    id integer NOT NULL,
    quantidade double precision NOT NULL,
    dt_insert timestamp without time zone,
    dt_update timestamp without time zone,
    venda_id integer,
    produto_id integer,
    veiculo_id integer,
    valor_unitario double precision
);

ALTER TABLE itens_venda_produto OWNER TO postgres;

CREATE SEQUENCE itens_venda_produto_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;

ALTER TABLE itens_venda_produto_id_seq OWNER TO postgres;

ALTER SEQUENCE itens_venda_produto_id_seq OWNED BY itens_venda_produto.id;

CREATE TABLE produtos (
    id integer NOT NULL,
    nome character varying(255) NOT NULL,
    descricao character varying(255),
    preco_compra double precision NOT NULL,
    data_compra date NOT NULL,
    dt_insert timestamp without time zone,
    dt_update timestamp without time zone,
    qtd_litros double precision NOT NULL,
    total double precision NOT NULL
);

ALTER TABLE produtos OWNER TO postgres;

CREATE SEQUENCE produtos_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;

ALTER TABLE produtos_id_seq OWNER TO postgres;

ALTER SEQUENCE produtos_id_seq OWNED BY produtos.id;

CREATE TABLE veiculos (
    id integer NOT NULL,
    condutor character varying(255) NOT NULL,
    placa character varying(255) NOT NULL
);

ALTER TABLE veiculos OWNER TO postgres;

CREATE SEQUENCE veiculos_id_seq
    START WITH 1
    INCREMENT BY 1
    NO MINVALUE
    NO MAXVALUE
    CACHE 1;


ALTER TABLE veiculos_id_seq OWNER TO postgres;

ALTER SEQUENCE veiculos_id_seq OWNED BY veiculos.id;

CREATE TABLE vendas (
    id integer NOT NULL,
    data_venda date NOT NULL,
    valor_total double precision NOT NULL,
    dt_insert timestamp without time zone,
    dt_update timestamp without time zone,
    anulada integer,
    cliente_id integer
);
ALTER TABLE vendas OWNER TO postgres;

ALTER TABLE ONLY cidades ALTER COLUMN id SET DEFAULT nextval('cidades_id_seq'::regclass);

ALTER TABLE ONLY clientes ALTER COLUMN id SET DEFAULT nextval('clientes_id_seq'::regclass);

ALTER TABLE ONLY estados ALTER COLUMN id SET DEFAULT nextval('estados_id_seq'::regclass);

ALTER TABLE ONLY item_venda_produto_usados ALTER COLUMN id SET DEFAULT nextval('item_venda_produto_usados_id_seq'::regclass);

ALTER TABLE ONLY itens_venda_produto ALTER COLUMN id SET DEFAULT nextval('itens_venda_produto_id_seq'::regclass);

ALTER TABLE ONLY produtos ALTER COLUMN id SET DEFAULT nextval('produtos_id_seq'::regclass);

ALTER TABLE ONLY veiculos ALTER COLUMN id SET DEFAULT nextval('veiculos_id_seq'::regclass);

ALTER TABLE ONLY backup
    ADD CONSTRAINT backup_pkey PRIMARY KEY (id);

ALTER TABLE ONLY cidades
    ADD CONSTRAINT cidades_pkey PRIMARY KEY (id);

ALTER TABLE ONLY clientes
    ADD CONSTRAINT clientes_pkey PRIMARY KEY (id);

ALTER TABLE ONLY estados
    ADD CONSTRAINT estados_pkey PRIMARY KEY (id);

ALTER TABLE ONLY item_venda_produto_usados
    ADD CONSTRAINT item_venda_produto_usados_pkey PRIMARY KEY (id);

ALTER TABLE ONLY itens_venda_produto
    ADD CONSTRAINT itens_venda_produto_pkey PRIMARY KEY (id);

ALTER TABLE ONLY produtos
    ADD CONSTRAINT produtos_pkey PRIMARY KEY (id);

ALTER TABLE ONLY veiculos
    ADD CONSTRAINT veiculos_pkey PRIMARY KEY (id);

ALTER TABLE ONLY vendas
    ADD CONSTRAINT vendas_pkey PRIMARY KEY (id);

ALTER TABLE ONLY clientes
    ADD CONSTRAINT cidades_fk FOREIGN KEY (cidade_id) REFERENCES cidades(id);

ALTER TABLE ONLY vendas
    ADD CONSTRAINT cliente_fk FOREIGN KEY (cliente_id) REFERENCES clientes(id);

ALTER TABLE ONLY cidades
    ADD CONSTRAINT estados_fk FOREIGN KEY (estado_id) REFERENCES estados(id);

ALTER TABLE ONLY itens_venda_produto
    ADD CONSTRAINT produto_fk FOREIGN KEY (produto_id) REFERENCES produtos(id);

ALTER TABLE ONLY item_venda_produto_usados
    ADD CONSTRAINT produtos_fk FOREIGN KEY (produto_id) REFERENCES produtos(id);

ALTER TABLE ONLY itens_venda_produto
    ADD CONSTRAINT veiculo_fk FOREIGN KEY (veiculo_id) REFERENCES veiculos(id);

ALTER TABLE ONLY itens_venda_produto
    ADD CONSTRAINT venda_fk FOREIGN KEY (venda_id) REFERENCES vendas(id);

ALTER TABLE ONLY item_venda_produto_usados
    ADD CONSTRAINT vendas_fk FOREIGN KEY (venda_id) REFERENCES vendas(id);
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
![Diretórios](https://cldup.com/eAq9LU0BJs-3000x3000.png)
