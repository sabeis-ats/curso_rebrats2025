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

---

## 🧩 Público-alvo

Profissionais dos **Núcleos de Avaliação de Tecnologias em Saúde (NATS)** com experiência prévia em R, interessados em ampliar a autonomia no acesso e análise de dados da SABEIS.

---

## 🧠 Equipe

- **Docentes:** Felipe Ferré e Amanda Lyrio  
- **Monitores:** Mariá Pereira, Jéssica Barreto, Laís Lessa e Michael Ruberson  
- **Carga horária:** 12h | **Vagas:** 25  
- **Pré-requisito:** teste prático de domínio básico em R

---

## 🔗 Referências

- [SABEIS - BD Geral](https://github.com/sabeis-ats/bd_geral)  
- [SABEIS - ETL](https://rpubs.com/sabeis/etl)  
- Ferré, F. *Modelagem e gestão de banco de dados com SQL e integração com o R.*  
  In: **Avaliação de impacto das políticas de saúde: um guia para o SUS**, Editora MS, 2023.  
- [Anais SBCAS 2020 – Sala de Situação SABEIS](https://sol.sbc.org.br/index.php/sbcas/article/view/11530)

---

## 📂 Como usar este repositório

1. Clone ou baixe o repositório:
```bash
   git clone https://github.com/sabeis-ats/curso_rebrats2025.git
````

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
