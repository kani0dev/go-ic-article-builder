# Alfred
> ele ajuda a organizar o Quarto

Fábrica de templates `.qmd` (Quarto) para os artigos da IC.

A ferramenta lê um arquivo de configuração com os autores e categorias
permitidos e gera um artigo `.qmd` a partir de um template embutido no
binário. Nomes são comparados ignorando maiúsculas/minúsculas e espaços
extras, então `Jhon Doe` e `jhon doe` viram um autor só.

## Configuração

Copie o exemplo e edite:


```bash
./article-builder new \
  --title "Análise de Regressão Linear" \
  --author "jhon doe" \
  --author "Mac D. Duphing" \
  --category "estatística" \
  --category "regressão"
```

Gera `analise-de-regressao-linear.qmd`:

```qmd
---
title: "Análise de Regressão Linear"
author: ["Jhon Doe", "Mac D. Duphing"]
date: "2026-10-25"
categories: ["estatística", "regressão"]
---
## Template

essa pagina é um template de exemplo para criar outras paginas
```

### Flags

| Flag | Descrição |
| --- | --- |
| `-t, --title` | Título do artigo (obrigatório) |
| `-a, --author` | Autor da lista de configuração (pode repetir) |
| `-c, --category` | Categoria da lista de configuração (pode repetir) |
| `-d, --date` | Data no formato `YYYY-MM-DD` (padrão: hoje) |
| `-i, --image` | Nome da imagem do front matter (`-i ""` remove a linha) |
| `-o, --output` | Caminho do `.qmd` (padrão: derivado do título) |
| `--config` | Caminho da configuração (padrão: `ic-factory.yaml`) |
| `--template` | Template `.qmd` alternativo (padrão: embutido) |
| `-f, --force` | Sobrescreve o arquivo de saída |

## Template

O template padrão fica em `src/internal/templates/summary.qmd` e é embutido
no binário com `go:embed`. Ele usa delimitadores `[[ ]]` (e não `{{ }}`)
para não conflitar com a sintaxe do Quarto.

Para usar outro template sem recompilar:

```bash
./ic-article-builder new -t "Artigo" -a "Jhon Doe" --template meu-template.qmd
```

Variáveis disponíveis: `.Title`, `.Authors`, `.Categories`, `.Date`, `.Image`.
Funções: `quote` (string YAML com aspas) e `list` (lista YAML).

## Estrutura

```
src/
  cmd/ic-article-builder/   entrada do binário
  internal/cli/             comandos cobra
  internal/config/          leitura e validação do YAML
  internal/model/           Summary, Author, Category
  internal/summary/         factory (validação) e renderização
  internal/templates/       template .qmd embutido
  internal/slug/            título -> nome de arquivo
```

## Testes

```bash
cd src
go test ./...
```
