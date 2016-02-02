# git-coaching
A repo for git coaching.


## Requisitos

- Instalar [git](https://git-scm.com/). É necessário ter algum terminal/prompt de comando com o comando `git` disponível. Verificar com `git --version` ou algum comando semelhante.
- Criar uma conta no [GitHub](https://github.com/)


## Ferramentas recomendadas

- [SourceTree](https://www.sourcetreeapp.com/)
- [GitHub Desktop](https://desktop.github.com/)
- [git for windows](https://git-for-windows.github.io/)
- [GitUp](http://gitup.co/)


## Comandos - básicos

### init

```sh
git init
```

Cria um repositório git na pasta atual. Ver a pasta oculta `.git`


### status

```sh
git status
```


### add

```sh
git add <file> [<file>]
git add . # todos arquivos e pastas do cwd, recursivamente
git add -A # todos arquivos e pastas do repositório, independente do cwd ser a raíz
```

Se o arquivo nunca foi commitado, git passa a rastrear (track) o arquivo.

Se o arquivo já foi commitado, git adiciona ao índice de alterações que farão parte do próximo commit.

### commit

```sh
git commit -m 'Mensagem do commit'
```

Cria um commit

### log

```sh
git log
git log --oneline
git log --pretty=format:'%h %ad | [%an] %s%d' --graph --date=short
```

Log de commits

**Até aqui, todos os comandos foram locais. Agora veremos alguns comandos que se comunicam com repositórios remotos**







## Arquivos importantes

`.git` - pasta na raíz do projeto que contém toda base de dados do git do projeto atual.

`.gitignore` - arquivo na raíz do projeto contendo a lista de arquivos (suporta glob) a serem ignorados pelo git. Binários, dependências, cache de build, credenciais. Ex:

```text
node_modules
build
```

`.gitconfig` - arquivo na home do usuário, com configurações globais da instalação do git. Ex:

```text
[user]
  name = Rafael Eyng
  email = rafael@exemplo.com
[alias]
  l = log --oneline
[color]
  diff = auto
  status = auto
  branch = auto
  ui = true
[core]
  excludesfile = /Users/rafael/.gitignore_global
```

## Comandos - tags, remotes, branching




























## Materiais recomendados

[Pro Git](https://progit2.s3.amazonaws.com/en/2016-01-30-9e7cf/progit-en.1005.pdf) - free PDF e-book
