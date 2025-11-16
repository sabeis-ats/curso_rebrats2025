# Curso RebrATS 2025 — Acesso aos Dados da SABEIS via FTP utilizando R

Este repositório contém o material do curso **RebrATS 2025**, promovido pela **Sala Aberta de Inteligência em Saúde (SABEIS / DGITS / SECTICS / Ministério da Saúde)**.  
O objetivo é capacitar profissionais da **Rede Brasileira de Avaliação de Tecnologias em Saúde (RebrATS)** no **acesso, manipulação e análise de dados públicos do SUS** utilizando a linguagem **R** e o protocolo **FTP**.

---

## 📘 Estrutura do curso

O conteúdo do curso está disponível nos slides da pasta principal e abrange:

### 1. Introdução e fundamentos
- Conceitos de **dados abertos do SUS** e da **SABEIS**.  
- Estrutura do **repositório FTP do DATASUS** e principais bases (SIA, SIH, SIM, SINAN, CNES, etc).  
- Estratégia de disseminação via **TabNet/TabWin**.

### 2. Acesso aos dados via FTP
- Conexão a repositórios públicos usando **R e o pacote `curl`**.  
- Listagem e download de arquivos `.csv.gz` diretamente do servidor.  
- Substituição do uso de `RCurl` por `curl` para maior compatibilidade com SFTP moderno.

### 3. ETL e manipulação em R
- Leitura eficiente com `data.table::fread()`.  
- Organização e armazenamento em formatos **RData** e **SQL**.  
- Criação de funções automatizadas para carga e atualização de bases.  
- Integração com **repositórios GitHub da SABEIS** (ex.: [sabeis-ats/bd_geral](https://github.com/sabeis-ats/bd_geral)).

### 4. Enriquecimento e integração de dados
- Junção de tabelas de referência (PCDT, CID-10, SIGTAP, território).  
- Cruzamento de códigos de diretrizes com arquivos FTP.  
- Construção de coortes e indicadores em R.

### 5. Exemplo mínimo: acesso SFTP, seleção automática e leitura em R

Exemplo mínimo que:
1. Lista os arquivos no FTP via SFTP.  
2. Identifica o **primeiro arquivo `.csv.gz` cujo número de registros (informado no nome do arquivo) é menor que 20 mil**.  
3. Faz o download desse arquivo e lê o conteúdo com `data.table::fread()`.

```r
library(curl)
library(data.table)

base_url <- "sftp://45.231.133.29:7891/srv/ftp/dadosabertos/"
userpwd  <- "livre:12345"

## 1) Listar o diretório no SFTP
h_list <- new_handle(userpwd = userpwd, dirlistonly = TRUE)
con    <- curl(base_url, handle = h_list)
arquivos <- readLines(con, warn = FALSE)
close(con)

# Limpeza básica das linhas
arquivos <- trimws(arquivos)
arquivos <- arquivos[nzchar(arquivos)]

# Se o servidor devolver formato "ls -l", pega só o último campo (nome do arquivo)
arquivos <- sub("^.* ", "", arquivos)

# Filtrar apenas arquivos .csv.gz
arquivos_csv <- arquivos[grepl("\\.csv\\.gz$", arquivos)]

## 2) Extrair o número de registros do nome
## Assumindo padrão: qualquer_coisa.NUMERO.csv.gz
## Ex.: tf_sia_200801_pa_045_pom_subgrupo604.20251105.1398.csv.gz -> 1398
n_reg <- as.integer(sub(".*\\.([0-9]+)\\.csv\\.gz$", "\\1", arquivos_csv))

# Índice do primeiro arquivo com menos de 20 mil registros
idx <- which(n_reg < 20000)[1]
if (is.na(idx)) stop("Nenhum arquivo com menos de 20000 registros encontrado.")

arquivo_escolhido <- arquivos_csv[idx]
cat("Arquivo escolhido:", arquivo_escolhido, "com", n_reg[idx], "registros (do nome)\n")

## 3) Baixar o arquivo escolhido
h_dl   <- new_handle(userpwd = userpwd)
destino <- arquivo_escolhido

curl_download(
  url      = paste0(base_url, arquivo_escolhido),
  destfile = destino,
  mode     = "wb",
  handle   = h_dl
)

cat("Arquivo salvo em:", destino, "\n")

## 4) Ler com data.table
df <- fread(destino)

dim(df)          # número de linhas e colunas
df[1:3, 9:11]    # primeiras linhas, colunas 9 a 11
````

Saída ilustrativa (exemplo real):

```text
Arquivo salvo em: tf_sia_200801_pa_045_pom_subgrupo604.20251105.1398.csv.gz 

[1] 1398   29

         nu_cnspcn co_procedimento co_cidpri
             <i64>           <int>    <char>
1: 178772228470654       604240104      E740
2: 172373491300552       604240104      E740
3: 171277821143942       604240104      E740
```

---

## 🧩 Público-alvo

Profissionais dos **Núcleos de Avaliação de Tecnologias em Saúde (NATS)** com experiência prévia em R, interessados em ampliar a autonomia no acesso e análise de dados da SABEIS.

---

## 🧠 Equipe

* **Docentes:** Felipe Ferré e Amanda Lyrio
* **Monitores:** Mariá Pereira, Jéssica Barreto, Laís Lessa e Michael Ruberson
* **Carga horária:** 12h | **Vagas:** 25
* **Pré-requisito:** teste prático de domínio básico em R

---

## 🔗 Referências

* [SABEIS - BD Geral](https://github.com/sabeis-ats/bd_geral)
* [SABEIS - ETL](https://rpubs.com/sabeis/etl)
* Ferré, F. *Modelagem e gestão de banco de dados com SQL e integração com o R.*
  In: **Avaliação de impacto das políticas de saúde: um guia para o SUS**, Editora MS, 2023.
* [Anais SBCAS 2020 – Sala de Situação SABEIS](https://sol.sbc.org.br/index.php/sbcas/article/view/11530)

---

## 📂 Como usar este repositório

1. Clone ou baixe o repositório:

```bash
   git clone https://github.com/sabeis-ats/curso_rebrats2025.git
```

2. Abra o material no RStudio ou visualizador Markdown.
3. Execute os exemplos de código em sequência, garantindo acesso à internet e às dependências R:

```r
   install.packages(c("curl", "data.table", "readr", "dplyr", "stringr", "tidyr"))
```

4. Explore os scripts e slides disponíveis para reproduzir as atividades práticas.

---

## 🏁 Licença

Material didático de uso público, distribuído sob **Licença Creative Commons Atribuição (CC BY 4.0)**.
Créditos obrigatórios: *Sala Aberta de Inteligência em Saúde (SABEIS / DGITS / SECTICS / Ministério da Saúde)*.

```
```
