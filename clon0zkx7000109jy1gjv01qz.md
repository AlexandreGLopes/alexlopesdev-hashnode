---
title: "Memorex do Git"
datePublished: Mon Nov 06 2023 14:56:44 GMT+0000 (Coordinated Universal Time)
cuid: clon0zkx7000109jy1gjv01qz
slug: memorex-do-git
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1699282911947/5cce364f-d999-45e6-87dd-b3f266dd1fa2.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1699282754979/e1954ac9-3e60-4d55-8e55-434ca76a58ed.jpeg
tags: git

---

Este é nosso primeiro memorex. Em resumo, um guia rápido pra nossa memória pegar no tranco. Não vamos listar os comandos mais básicos como *add*, *status* e *commit*, a menos quando for para utilizar recursos que eles tenham a mais. Vamos listar aqui apenas comandos que costumo utilizar com mais frequência ou que sejam de interesse.

# COMANDOS BÁSICOS

### ADD

```bash
git add -- . ':!<path>'
```

* Adiciona todos os arquivos modificados ao staged, menos os que estiverem declarados no &lt;path&gt;
    

### COMPARAÇÃO

```bash
git diff
```

* Compara diferenças entre o que já está commitado e o que está modificado
    

```bash
git diff --cached
git diff --staged
```

* Comparam diferenças entre o que já está commitado e o que está staged
    

### HISTÓRICOS DE COMMITS

```bash
git log
```

* Listagem dos commits (do mais recente para o mais antigo)
    

```bash
git log --oneline
```

* Usado oneline para resumir as informações do commit
    

```bash
git log -1
git log -2
git log -3
```

* Ver apenas 1, 2 ou 3 comits e assim sucessivamente
    

```bash
git log -p
git log --patch
```

* Trazem o commit e junto os diffs de cada commit
    

```bash
git log --stat
```

* traz o commit e traz também o nome dos arquivos alterados de cada commit
    

```bash
git log <branch>
git log <branch> <comandos>
```

* Trazem a listagem de commits da branch especificada no comando
    

```bash
git log -p -S"<sequencia_de_caracteres_procurada>" -- <caminho_para_arquivo>

# exemplo concreto:
git log -p -S"dev" -- */Web.config
```

* traz a listagem de commits e os diffs procurando por alterações que introduziram ou removeram uma determinada sequência de caracteres no arquivo ou arquivos do caminho especificado.
    

### ALTERANDO COMMITS E MENSAGENS

```bash
git commit --amend -m "Mensagem nova para ser alterada"
```

* deleta o commit em HEAD e refaz o commit novamente com uma nova mensagem.
    
* O HASH VAI MUDAR POIS HAVERÁ DELEÇÃO
    
* CUIDADO. SE ALGUEM ESTIVER TRABALHANDO COM ESSE COMMIT TAMBÉM PODE DAR CONFLITO. POIS ESTÁ DELETANDO ELE.
    

```bash
git commit --amend --no-edit
```

* commita o que estiver staged no commit que estiver em HEAD sem modificar a mensagem.
    
* O HASH VAI MUDAR POIS HAVERÁ DELEÇÃO
    
* CUIDADO. SE ALGUEM ESTIVER TRABALHANDO COM ESSE COMMIT TAMBÉM PODE DAR CONFLITO. POIS ESTÁ DELETANDO ELE.
    

### MOVENDO ENTRE COMMITS E BRANCHES

```bash
git checkout <hash>

# exemplo concreto:
git checkout sb12bdh3
```

* Voltar a um commit de acordo com o número do hash escolhido
    

```bash
git checkout <branch>
git switch <branch>

# exemplo concreto:
git checkout main
```

* Voltar para o commit HEAD (mais recente) da branch
    

```bash
git checkout -f <branch>
```

* Muda de branch e remove todas as alterações que estiverem rastreadas (deixando as modificações não rastreadas)
    

```bash
git switch -
```

* Muda para a última branch que estava antes da atual
    

```bash
git checkout -b <branch> <remote_branch>
```

* Baixa uma branch do repositório e vai pra ela. Ou melhor, cria uma branch local com base na branch do repositório e faz o 'check' nela.
    

### REVERTENDO, DESCARTANDO E LIMPANDO ALTERAÇÕES

```bash
git checkout <arquivo>

# exemplo concreto:
git checkout index.txt
```

* Reverte as alterações do arquivo rastreado/modificado para a última versão
    

```bash
git checkout .
```

* Reverte a versão de TODOS os arquivos rastreados/modificados.
    

```bash
git clean -f
```

* Remover arquivos não rastreados usando force
    
* Não vai remover arquivos que já foram colocados em modified pelo comando "add" e depois removidos pelo "rm" e passaram a status modified
    

```bash
git restore --staged <arquivo>
git rm --cached <arquivo>
```

* Remover arquivos rastreados/stageds.
    

```bash
git restore <arquivo>
```

* Descarta mudanças do arquivo e retorna seu status para o do ultimo commit (mas só aparece quando temos um commit já feito)
    

```bash
git rm <arquivo>
```

* Descarta mudanças do arquivo e retorna seu status para unstaged
    

```bash
git reset --hard
```

* Descarta tudo o que estiver sendo feito desde o último commit, desde modified, staged e unstaged
    

### IGNORANDO OU RASTREANDO ARQUIVOS

```bash
git update-index --skip-worktree <arquivo>
```

* Atualizar o controle de rastreio de arquivos deixando de rastrear o arquivo nomeado
    

```bash
git update-index --no-skip-worktree <arquivo>
```

* Atualizar o controle de rastreio de arquivos voltando a rastrear o arquivo nomeado
    

```bash
find . -maxdepth 1 -type d \( ! -name . \) -exec bash -c "cd '{}' && pwd && git ls-files -z ${pwd} | xargs -0 git update-index --skip-worktree" \;
```

* Atualizar o controle de rastreio de arquivos *deixando de rastrear* todos os arquivos dentro da *pasta atual* e suas subpastas. Não esquecer de usar o comando cd para entrar na pasta antes de usar o comando
    
* *Só funciona no git bash*.
    

```bash
find . -maxdepth 1 -type d \( ! -name . \) -exec bash -c "cd '{}' && pwd && git ls-files -z ${pwd} | xargs -0 git update-index --no-skip-worktree" \;
```

* Atualizar o controle de rastreio de arquivos *voltando a rastrear* todos os arquivos dentro da *pasta atual* e suas subpastas. Não esquecer de usar o comando cd para entrar na pasta antes de usar o comando
    
* *Só funciona no git bash*.
    

```powershell
Get-ChildItem -Directory | ForEach-Object { Set-Location $_.FullName; Write-Host (Get-Location).Path; Get-ChildItem -File -Recurse -Attributes !Hidden | ForEach-Object { git update-index --skip-worktree $_.FullName } ; Set-Location .. }
```

* Comando correlato para *deixar de rastrear* que vimos acima. Só que este funciona no Powershell.
    

```powershell
Get-ChildItem -Directory | ForEach-Object { Set-Location $_.FullName; Write-Host (Get-Location).Path; Get-ChildItem -File -Recurse -Attributes !Hidden | ForEach-Object { git update-index --no-skip-worktree $_.FullName } ; Set-Location .. }
```

* Comando correlato para *voltar a rastrear* que vimos acima. Só que este funciona no Powershell.
    

### CONFIGURANDO REPOSITÓRIOS REMOTOS

```bash
git remote -v
```

* Lista os endereços de repositórios associados ao git
    

```bash
git remote add <nome_do_remote> <endereço_do_remote>

# exemplo concreto:
git remote add origin https://github.com/exemplo
# exemplo concreto:
git remote add abc https://github.com/exemplo
```

* Adiciona um endereço de repositório ao git. O nome comum de usar primariamente é origin, mas não necessariamente é o nome a ser escolhido. No exemplo damos o nome abc ao segundo.
    

```bash
git remote set-url <nome_do_remote> <endereço_do_remote>

# exemplo concreto:
git remote set-url origin https://gitlab.com/exemplo2
```

* Altera um endereço de repositório ao git
    

### BRANCHES

```bash
git branch
git branch --list
```

* Lista as branchs locais
    

```bash
git branch -a
```

* Lista todas as branchs locais e remotas. Mas não estará necessariamente atualizado com a lista remota atual do repositório.
    

```bash
git remote update origin --prune
```

* Atualiza a lista de branchs locais e remotas, baixando as que não estão listadas localmente e removendo as locais que foram deletadas do repositório remoto.
    

```bash
git branch <branch>
```

* Cria uma branch com o nome escolhido
    

```bash
git checkout -b <branch>
git switch -c <branch>
```

* Cria uma branch com o nome escolhido e já passa para esta branch
    

```bash
git checkout -b <branch> <remote_branch>
```

* Baixa uma branch do repositório e vai pra ela. Ou melhor, cria uma branch local com base na branch do repositório e faz o 'check' nela
    

```bash
git push -u <remote> <branch>
git push --set-upstream <remote> <branch>

# exemplo concreto:
git push --set-upstream origin branch_1
```

* Manda a branch local para o servidor (github, gitlab, bitbucket).
    

```bash
git branch -d <branch>
```

* Deleta localmente a branch específica
    

```bash
git branch -D <branch>
```

* Deleta localmente a branch específica forçando a deleção para o caso de mensagem de atenção impedindo.
    

```bash
git push --delete <remote> <branch>
```

* Deleta a branch do repositório remoto
    

```bash
git branch -m <novo_nome_da_branch>
```

* Altera o nome da branch na qual estamos checkados
    

```bash
git branch -m <branch> <novo_nome_da_branch>
```

* Altera o nome da branch específica que passamos no comando para um novo nome
    

### PULL

```bash
git pull <remote> <branch_da_qual_vamos_puxar>

# exemplo concreto:
git pull origin master
```

* Incorpora na branch atual as alterações da branch específicada
    

```bash
git fetch <remote> <branch_fonte>:<branch_destino>

# exemplo concreto:
git fetch origin master:master
```

* Faz fetch das alterações qe existem no remote de uma branch específica.
    
* Pode ser usado para buscar as alterações de uma branch quando estamos em outra. E após isso podemos fazer o merge com 'git merge '
    

### MERGE

```bash
git merge <branch_da_qual_as_mudanças_vem>
```

* Traz as mudanças da branch especificada para a branch atual
    

```bash
git branch --no-merged
```

* Mostra uma lista das branchs que não foram mergeadas com a atual
    

```bash
git branch --merged
```

* Mostra uma lista das branchs que foram mergeadas com a atual
    

```bash
git merge --abort
git reset --hard
```

* Cancela o merge que esteja com status de conflito
    

```bash
#1:
git checkout --ours <arquivo>
#2:
git add .
```

* Resolve o conflito de merge de um arquivo aceitando o formato da branch atual
    

```bash
#1:
git checkout --theirs <arquivo>
# 2:
git add .
```

* Resolve o conflito de merge de um arquivo aceitando o formato da branch que está sendo mergeada
    

### TAG

```bash
git tag
git tag --list
git tag -l
```

* Mostra a lista de todas as tags do projeto
    

```bash
git tag -n
```

* Mostra a lista de todas as tags do projeto com as mensagens
    

```bash
git tag <nome_tag>
```

* Cria uma tag lightweight com o nome específico no último commit da branch atual
    

```bash
git tag -a -m <mensagem_tag> <nome_tag>
```

* Cria uma tag annotated com mensagem e com o nome específico no último commit da branch atual
    

```bash
git tag <nome_tag> <hash_do_commit>
```

* Cria uma tag lightweight com o nome específico no commit especificado
    
* Pode ser usado pra tag annotated também se usar os comandos -a -m.
    

```bash
git show <tag>
```

* Mostra os detalhes do commit com a tag específica, inclusive nome, quem fez, mensagem da tag, mudanças do commit, etc.
    

```bash
git push <remote> <nome_tag>
```

* Manda as tags pro servidor
    

```bash
git push --tags
```

* Manda todas as tags pro servidor. Mas não é interessante porque se tiver alguma tag que já tenha sido apagada de lá por algum motivo, estará sendo enviada de volta.
    

```bash
git checkout <nome_tag>
```

* Voltar a um commit de acordo com a tag especificada
    

```bash
git diff <nome_tag> <nome_tag>
```

* Comparação de diferenças entre commits com as tags especificadas
    

```bash
git tag -d <nome_tag>
```

* Apagando a tag localmente
    

```bash
git push --delete <remote> <nome_tag>
```

* Apaga a tag do servidor
    

### STASH

```bash
git stash
```

* Salva numa memória as mudanças rastreadas e limpa as modificações da branch atual
    

```bash
git stash list
```

* Lista os stashs salvos na memória
    

```bash
git stash apply
```

* Retorna as mudanças mais atuais da memória para a branch atual. Sem limpar a memória
    

```bash
git stash apply <stash_posição>

# exemplo concreto:
git stash apply stash@{1}
```

* Retorna as mudanças do stash posição 1 da memória para a branch atual. Sem limpar a memória
    
* No Powershell se usarmos o comando como está no exemplo vai dar um erro. Precisamos colocar a posição entre aspas simples, por exemplo: `'stash@{1}'`
    

```bash
git stash pop
```

* Retorna as mudanças mais atuais da memória para a branch atual. limpando a memória
    
* Pode ser utilizado como o stash apply especificando quando posição da memória vai retornar
    

```bash
git stash drop
```

* Remove as mudanças mais atuais da memória.
    
* Pode ser utilizado como o stash apply especificando quando posição da memória vai remover
    

```bash
git stash branch <nome_branch> <stash_posição>
```

* Cria uma nova branch com o nome especificado a partir de um stash em memória também especificado por sua posição
    

# COMANDOS AVANÇADOS

### REVERTENDO COMMITS

```bash
git revert <commit>
git revert HEAD
```

* Cria um novo commit desfazendo as alterações que foram feitas no commit especificado no comando
    
* Pode ser usado o comando --no-edit para não abrir uma edição da mensagem do commit
    

### DESFAZENDO/DELETANDO COMMITS

```bash
git reset --hard
```

* Todas as mudanças rastreadas sejam desfeitas
    

```bash
git reset --hard <commit>
```

* Desfaz commits até o commit especificado e coloca o HEAD nele
    

```bash
git reset --hard <commit_ou_HEAD>~<numero>

# exemplo concreto:
git reset --hard HEAD~1
```

* Desfaz uma quantidade específica de commits a partir de onde estiver o HEAD. Esses commits são deletados, ignorados no histórico do log
    

```bash
git reset --mixed HEAD~<numero>
```

* Desfaz uma quantidade específica de commits a partir de onde estiver o HEAD. Mas as alterações que haviam no commit são jogados para a área unstaged.
    

```bash
git reset --soft HEAD~<numero>
```

* Desfaz uma quantidade específica de commits a partir de onde estiver o HEAD. Mas as alterações que haviam no commit são jogados para a área staged.
    

### FORÇANDO ENVIO DE MUDANÇAS

```bash
git push --force
```

* Força o envio de mudanças locais para o repositório. Mudanças que estejam apenas no repositório serão perdidas.
    
* Um uso importante pode ser para remover dados que não podem estar salvos no histórico de commits no servidor, como senhas etc.
    
* Outro uso importante também seria para retornar ao repositório mudanças que tenham sido deletadas completamente, mas que ainda tenhamos elas localmente e tenhamos certeza de que está correto.
    

```bash
git push --force-with-lease
```

* Vai sobrescrever a linha do tempo do repositório, mas somente se nenhuma alteração for perdida
    

### REBASE (UM MERGE QUE NÃO DEIXA COMMIT DE MERGE NO HISTÓRICO)

```bash
git rebase <branch>
```

* Troca a base da branch atual para outra base mais atual na branch especificada
    
* Recomendação: não utilizar rebase em branchs públicas, utilizadas por outras pessoas. Porque ele também vai sobrescrever o histórico de commits
    

```bash
git rebase --abort
```

* Cancela o rebase que esteja com status de conflito
    

```bash
git rebase --continue
```

* Após resolver os conflitos no rebase este comando efetiva o rebase
    

```bash
git pull --rebase
```

* Vai no remote buscando os commits remotos, vai aplicá-los em cima da branch local atual, e se tiver algum commit a local a mais (ahead) ele vai ser reescrito em cima da mudança que vem do servidor
    
* Ou seja, é uma forma de fazer um pull no caso de ter um rebase. E ajuda no caso de ter um commit a mais no local.
    

```bash
git rebase --interactive <commit_ou_HEAD>~<numero>
git rebase -i <commit_ou_HEAD>~<numero>

# exemplo concreto:
ex.: git rebase --interactive HEAD~3
# exemplo concreto:
ex.: git rebase --interactive sg2w3a7~1
```

* Faz a fusão de commits. Para juntar os commits no último commit mais recente terá que ser utilizada a opção squash no editor que vai abrir
    

### RECRIANDO BRANCH BASEADA NO SERVIDOR

```bash
git fetch <remote> <branch>

# exemplo concreto:
git fetch origin develop
```

* Traz parcialmente a branch remota para o repositório local. Será necessário fazer um cehckout para a branch em seguida.
    

### CHERRY PICK

```bash
git cherry-pick <commit>
```

* Busca o commit especificado de uma outra branch e traz pra branch atual
    

```bash
git cherry-pick -n <commit>
```

* Busca o commit especificado de uma outra branch e traz pra branch atual mas não efetua o commit. Aguarda pelo commit do usuário
    

### BISECT (UMA FORMA DE BUSCAR NO HISTÓRICO DE COMMITS EM QUAL COMMIT COMEÇOU UM ERRO)

```bash
git bisect start
```

* Inicia a busca binária.
    
* Vai esperar o próximo comando
    

```bash
git bisect good <commit>
```

* Informa-se o último commit conhecido no qual o código estava funcionando da maneira esperada.
    
* Vai esperar pelo próximo comando
    

```bash
git bisect bad <commit>
```

* Informa-se o commit conhecido no qual o código está dando erro. E teremos um checkout para um commit no meio do caminho entre os dois ultimos.
    
* Vai esperar pelo próximo comando. Neste próximo comando terá que informar se o commit no qual teve o checkout está funcionando conforme o esperado ou não.
    

```bash
git bisect good
# ou
git bisect bad
```

* Teremos um checkout para um commit no meio do caminho.
    
* Será repetido a espera pelos comandos até terminar as iterações e no final retornará o commit no qual o problema teve origem.
    

```bash
git bisect reset
```

* Ao terminar a iteração ainda nãoterminou o processo do bisect. É necessário fazer o bisect reset para finalizar.
    

### FETCH

```bash
git fetch
```

* Vai trazer commits, branchs e tags para o repositório local.
    
* Antes de executar um git pull fazemos um git fetch pra ver o que vai estar sendo trazido.
    
* Após o 'git fetch' podemos fazer um 'git log -a' e veremos as 'origin/branch' nas quais temos as alterações que estão no repositório remoto.
    
* E podemos até usar um 'git diff' pra comparar o local com o remoto.
    

### ALIAS

```bash
git config --global alias.<comando_novo> <comando_representado>

# exemplo concreto:
git config --global alias.s status
```

* Podemos gravar comandos novos para faciliar a chamada de comandos que utilizamos muito. No exemplo acima poderemos começar a usar 'git s' ao invés de 'git status'
    

```bash
git config --global --unset alias.<comando_novo>
```

* Descarta os comandos que foram adicionados pelo usuário com o alias
    

```bash
git config --get-regexp ^alias
# ou
git config --list | findstr alias
```

* Lista o aliases configurados (lembrando que 'findstr' é no powershell do windows e 'grep' é no bash do linux)
    

### GREP

```bash
git branch | grep <filtro>
```

* No caso, estamos usando o comando grep pra filtrar branchs com o nome que tenha o filtro que adicionamos no comando
    

```bash
git log --oneline | grep <filtro>
```

* No caso, estamos usando o comando grep pra filtrar commits com o nome que tenha o filtro que adicionamos no comando
    

```bash
git branch | findstr <filtro>
```

* `findstr` é o mesmo comando do grep só que para o POWERSHELL
    

### COMO ESCREVER MENSAGEM COM MAIS DE UMA LINHA NOS COMMITS

```bash
git commit -m 'Message
>
>goes
>here'
```

* Ao escrever um commit e abrir uma aspas ou aspas simples e não fechar alguns shells vão deixar escrever mais em linhas até que feche a aspas.
    

```bash
git commit -F- <<EOF
>Message
>
>goes
>here
>EOF
```

* Alternativamente também é possivel usar um "here document" (também conhecido como heredoc).