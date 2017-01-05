# Git & GitHub

A repo for Git & GitHub coaching.


## Requisitos

- Instalar [git](https://git-scm.com/). É necessário ter algum terminal/prompt de comando com o comando `git` disponível. Verificar com `git --version` ou algum comando semelhante.
- Criar uma conta no [GitHub](https://github.com/)


## Comandos - operações locais básicas

### init

```sh
git init
```

Cria um repositório git na pasta atual. Ver a pasta oculta `.git`.


### status

```sh
git status
```

Mostra o status do repositório local e de seus arquivos.


### diff

```sh
git diff
git diff <file>
git diff --staged
```

Mostra as alterações feitas em cada arquivo.


### add

```sh
git add <file> [<file>]
git add . # todos arquivos e pastas do cwd, recursivamente
git add -A # todos arquivos e pastas do repositório, independente do cwd ser a raíz
```

Se o arquivo nunca foi commitado, git passa a rastrear (track) o arquivo.

Se o arquivo já foi commitado, git adiciona ao índice de alterações que farão parte do próximo commit.

Se não houvesse o  `git add`, teríamos que fazer sempre commit de todas as alterações, sem poder de escolha.

### commit

```sh
git commit -m 'Mensagem do commit'
```

Cria um commit de todas as alterações adicionadas ao índice.

### log

```sh
git log
git log --oneline
git log --pretty=format:'%h %ad | [%an] %s%d' --graph --date=short
```

Log de commits.


## Arquivos importantes

`.git` - pasta na raíz do projeto que contém toda base de dados do git do projeto atual.

`.gitignore` - arquivo na raíz do projeto contendo a lista de arquivos (suporta glob) a serem ignorados pelo git. Binários, dependências, cache de build, credenciais, configurações de IDE. Ex:

```text
.project
build/
node_modules/
```

`.gitattributes` - arquivo na raíz do projeto, contendo algumas configurações para aquele projeto.

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


## Comandos - tags, branching

### tag

```sh
git tag
git tag v1.0.0 <commit_hash>
git tag v1.0.0
```

Lista ou cria tags. Uma tag é uma referência **fixa** a um commit. Uma tag é vinculada a um commit específico (por default, o atual, indicado por HEAD).

Ver mais sobre tags em `push`.


### branch

```sh
git branch
git branch <branch_name>
git branch -d <branch_name>
git branch -D <branch_name> # force delete
```

Lista, cria, apaga branches. No git, branches não são cópias do projeto, são apenas referências **móveis** para commits.  A branch default se chama **master**.

### checkout

```sh
git checkout <commit_hash>
git checkout <branch_name>
git checkout -b <branch_name> # cria e troca para <branch_name>
git checkout -b <branch_name> <tag_name> # cria <branch_name> com o estado da tag especificada
```

Checkout é o processo de pegar uma versão específica do projeto inteiro (ou também de arquivos específicos) do *git directory* e disponibilizar no *working directory*. *Working directory* é a parte do repositório git onde você trabalha (suas pastas e arquivos), diferente do *git directory* (a pasta .git), onde git armazena seus dados.


### merge

```sh
git merge <desired_branch>
```

Faz merge da branch especificada com a branch atual.

O merge é a maneira mais comum de integrar branches. Na maior parte dos casos, git consegue resolver o merge automaticamente. Se houverem alterações conflitantes nas duas branches, é necessário resolver o merge e fazer um commit da resolução. Em geral só existem conflitos se a mesma linha de um arquivo foi alterada de forma diferente em cada branch.

Pensando no grafo de commits, o merge cria um novo nodo (um commit) com referência para 2 ou mais commits pais.


### rebase

```sh
git rebase <desired_branch>
```

Faz rebase da branch especificada **no topo** da branch atual.

É uma alternativa ao merge para integrar branches. Diferentemente do merge, não cria um novo commit que aponta para os commits pais, mas sim "corta fora" a branch alvo e reaplica ela, commit a commit, na ponta da branch atual.


## GitHub repositories

### organization, repository, team

Demonstração no navegador.


## Comandos - trabalhando com remotes

**remote** - um repositório remoto do projeto, com o qual nosso repositório local se comunica. Normalmente em um serviço como GitHub, mas não necessariamente. O remote default se chama **origin**.


### remote

```sh
git remote
git remote -v
git remote add <name> <url>
```

Lista os remotes, adiciona novos remotes, remove remotes etc.


### clone

```sh
git clone <url>
git clone <url> <dest>
```

Cria um repositório local clonado a partir de um repositório remoto.

Em um projeto em equipe, a pessoa que cria o projeto dá `git init` e manda para um remote. Todos os demais dão `git clone` a partir desse remote.

Quando clonamos um repositório de um remote, este remote automaticamente é adicionado com o nome de origin.


### pull

```sh
git pull <remote> <branch>
git pull
```

Atualiza a branch local com o conteúdo da branch remota. Se a ponta das duas branches for diferente, requer merge.


### push

```sh
git push <remote> <branch>
git push
git push --tags # tags precisam de um push específico
```

Atualiza a branch remota com o conteúdo da branch local. Se a ponta das duas branches for diferente, requer pull + merge antes do push. Ou seja, o conflito é resolvido localmente, não remotamente.


## GitHub issues & pull requests

### issues e pull requests

Demonstração no navegador.


## Tópicos especiais

### branch para "backup"

Quando você vai fazer alguma coisa que não tem muita certeza, pode criar uma branch de "backup" para voltar com segurança a um ponto anterior. Simplesmente faça:

```sh
git branch <backup-branch-name> # qualquer nome não usado ainda
```

Tudo o que você fez é criar uma referência (o nome da branch) a um ponto específico do projeto ao qual você pode querer voltar, usando um `git checkout <branch>`.


### desfazendo alterações locais

Para desfazer as alterações de algum arquivo:

```sh
git checkout -- <file>
```

Para desfazer o `git add` de algum arquivo:

```sh
git reset <file>
```

Cuidado com esses comandos. Eles não são intuitivos, e são comandos que podem fazer você apagar código que não queria. Recomendo usar as ferramentas (SourceTree etc) para fazer essas ações.

`checkout` e `reset` fazem diversas coisas diferentes de acordo com as flags que recebem. É interessante ler a documentação desses comandos.


### reescrever história

Certos comandos de git reescrevem a história do projeto, ou seja, permitem apagar ou alterar objetos existentes do grafo de commits. Em certos casos isso não gera problemas, mas em geral deve ser evitado.

A regra é: **não reescreva algo que você já enviou para um remote**.

Ou seja, se você criou um commit no seu repositório local, você pode escolher apagá-lo sem problemas. Mas se você já enviou esse commit para o remote e apagá-lo no repositório local, pode gerar sérios conflitos com a versão do remote ou com outro desenvolvedor que já havia baixado aquele commit.


### revertendo um commit

Para commits que já sofreram push, é recomendável usar a técnica de fazer um novo commit desfazendo as alterações que foram introduzidas por esse commit que você deseja apagar. Ferramentas como GitHub Desktop e SourceTree fazem isso automaticamente, com a opção **Revert commit**.

Para commits locais que não sofreram push, pode-se usar:

```sh
git reset --hard HEAD~1 # CUIDADO! Você vai perder o último commit
```

onde `1` é o número de commits que se deseja voltar, contando a partir do HEAD. Estamos literalmente voltando a ponta do grafo em 1 commit e perdendo referência para o commit "apagado".


### amend

```sh
git commit --amend
```

Este comando refaz o último commit, adicionando ao commit tudo que estiver na *staging area* (tudo que sofreu `git add`). É uma forma de reescrever a história do último commit. **Não usar se o commit já sofreu push**.

Serve para alguns cenários comuns:

1. reescrever a mensagem do último commit
1. adicionar/remover ao último commit alguma coisa que esquecemos, e que não queremos fazer em um novo commit


### reset hard / soft

Duas situações comuns podem ser resolvidas com `reset --hard` e `reset --soft`.

1. Quando você está com muitas alterações e quer desfazer todas de uma vez:

  ```sh
  git reset --hard # se houver arquivos "untracked", é necessário fazer um `git add -A` antes para o git rastreá-los
  ```

1. Quando você quer desfazer o último commit (**não usar se já fez push do commit**) mas não perder as alterações que ele introduziu:

  ```sh
  git reset --soft HEAD~1
  ```

Isso irá desfazer a ação do commit, mas não as alterações que o commit introduziu. As alterações estarão na **staging area** da mesma forma que elas estavam antes de executar o commit.


### merge vs rebase

Embora o merge seja geralmente mais cômodo de fazer, rebase é geralmente considerado uma técnica superior. O use de rebase faz a história do projeto ficar mais linear do que expandida, e evita commits de resolução de merge.

Rebase é um comando complexo que suporta diversas nuances. Existem projetos que adotam a técnica de "um commit por pull request", para manter um histórico mais conciso e legível. O desenvolvedor pode trabalhar em sua branch local fazendo diversos commits, e antes de fazer push para o remote, usar rebase para fazer *squash* dos commits em um só. Veja a documentação para mais detalhes.


### stash

Às vezes estamos trabalhando em alguma alteração e precisamos parar para resolver alguma outra coisa no projeto. Geralmente isso envolveria uma troca de branches, ou resetar tudo o que fizemos até agora e recomeçar depois. Nesses casos, geralmente essas condições são verdadeiras:

- não queremos que nossas alterações (ainda não fizemos commit) em uma branch sejam levadas para a outra
- não queremos fazer commit pois a tarefa está incompleta
- não queremos desfazer e perder as alterações

`git stash` é um comando que "coloca de lado" todas nossas alterações atuais, em uma pilha, e nos deixa com o *working directory* limpo. Quando quisermos aplicar novamente nossas alterações, usamos `git stash pop`.

Note que o `stash` tem o comportamento default de uma pilha, mas podemos aplicar alterações que não estejam no topo caso já tenhamos empilhado mais coisas. Veja a documentação para mais detalhes.


### pull vs fetch

Até agora, só nos preocupamos com 2 tipos de branches: *local branches* (branches no seu repositório, nas quais você trabalha) e *remote branches* (branches no remote, nas quais você faz pull ou push).

Existe um terceiro tipo de branch, as *remote-tracking branches*. São branches **locais**, que fazem referência ao estado de branches **remotas**. Enquanto que uma *local branch* se chama, por exemplo, `master`, uma *remote-tracking branch* se chama `origin/master`, onde `origin` é o nome do remote. Essas branches são atualizadas automaticamente em comandos que acessam a *remote branch* correspondente.

A diferença dos comandos `pull` e `fetch`:

- `pull` atualiza a referência de 1 *remote-tracking branch* e de sua *local branch* correspondente. Por exemplo, se você está na `master` e faz `git pull`, suas branches `origin/master` (remote-tracking) e `master` (local) serão atualizadas com o conteúdo da branch no servidor.

- `fetch` atualiza a referência de **todas** *remote-tracking branches* e de **nenhuma** *local branch*. É uma forma de baixar os dados de todas as branches para sua máquina sem afetar suas *local branches*, apenas suas *remote-tracking branches*. Após um fetch, pode-se fazer um merge ou rebase da *remote-tracking branch* para a *local branch*, como `git merge origin/master`.


### detached HEAD

Dependendo do que você fizer, pode acabar em *detached HEAD state*. Embora pareça algo muito errado, simplesmente significa que seu HEAD está apontando diretamente para um commit (e não para uma branch, que por sua vez aponta para o commit, que é normalmente a situação do seu HEAD num projeto).


### Git Hooks

githooks são scripts arbitrários (você define o que quiser) que são executados em função de certos comandos git. Por exemplo, pode-se rodar testes ou build antes de fazer um commit ou um push e interromper o comando caso os testes falhem.

Para mais detalhes, veja [este post](http://codeheaven.io/simple-git-hooks-with-ghooks/).


### gh-pages

GitHub fornece um mecanismo simplificado de hospedagem de sites estáticos. Suporta *custom domains* e *gzip*.

Basta criar uma branch chamada `gh-pages` e dar push para o GitHub. A home do site estático será um arquivo `index.html` na raíz do projeto, e estará disponível em `<nome-do-usuario>.github.io/<nome-do-projeto>`.


### Submodules & subtrees

Se você **precisar embutir código reutilizável** (um módulo ou projeto auto-contido) dentro de um projeto que o contenha, e **não puder utilizar algum gerenciador de dependências** para isso por alguma questão específica da tecnologia/framework que estiver utilizando, submodules e subtrees podem ser a solução. Mas somente se não houver alternativa melhor.

Este tema é complexo e não será tratado aqui. Estas são provavelmente as melhores referências sobre isso:

[Submodules](https://medium.com/@porteneuve/mastering-git-submodules-34c65e940407#.lqo946vqm)

[Subtrees](https://medium.com/@porteneuve/mastering-git-subtrees-943d29a798ec#.kl471llr0)


## Workflows

### topic branch

Existe uma branch estável, a partir da qual são criadas branches para desenvolvimento de features. Quando for desejado integrar a feature, integra-se nessa branch estável através de *pull request* (preferencialmente com *code review*).

![topic branch workflow](https://cloud.githubusercontent.com/assets/4842605/12835036/b0842866-cb93-11e5-9197-a2e96a2ee472.png)

Opcionalmente pode haver uma branch estável de desenvolvimento e uma branch de produção que reflete o estado do sistema no ar, e só recebe da branch de desenvolvimento no deploy de novas versões.


### git flow

O git flow prevê um rígido esquema de branches, com 5 tipos de branches diferentes: features, development, releases, master e hotfixes.

![git flow workflow](https://cloud.githubusercontent.com/assets/4842605/12835086/29f607be-cb94-11e5-9ead-c60ca7c6468e.png)


Referências:

[A successful Git branching model](http://nvie.com/posts/a-successful-git-branching-model/)

[git-flow cheatsheet](http://danielkummer.github.io/git-flow-cheatsheet/)

### referência de workflows

Veja o guia da [Atlassian](https://www.atlassian.com/git/tutorials/comparing-workflows)


## Ferramentas recomendadas

- [SourceTree](https://www.sourcetreeapp.com/)
- [GitHub Desktop](https://desktop.github.com/)
- [git for windows](https://git-for-windows.github.io/)
- [SmartGit](http://www.syntevo.com/smartgit/)
- [Waffle](https://waffle.io/)
- [git flow](https://github.com/nvie/gitflow)
- [GitUp](http://gitup.co/)
- [ghooks](https://github.com/gtramontina/ghooks)


## Materiais recomendados

[Pro Git](https://progit2.s3.amazonaws.com/en/2016-01-30-9e7cf/progit-en.1005.pdf) - free PDF e-book
