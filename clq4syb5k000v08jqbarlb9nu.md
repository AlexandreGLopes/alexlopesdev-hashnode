---
title: "O Básico e Essencial do Docker"
datePublished: Thu Dec 14 2023 06:11:21 GMT+0000 (Coordinated Universal Time)
cuid: clq4syb5k000v08jqbarlb9nu
slug: o-basico-e-essencial-do-docker
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1702516977833/62252dd8-2525-4734-bd85-5f6874488d2d.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1702534269598/549d4bcc-c0af-4224-ac34-5209ec6f67c5.jpeg
tags: docker

---

### O que é Docker?

É uma forma de empacotar aplicações para as executar em pequenos ambientes isolados do sistema operacional hospedeiro. Esses ambientes são chamados de contêineres e são possibilitados por um processo parecido com uma virtualização. A diferença é que os conteineres acessam recursos do sistema hospedeiro sem ter que montar uma máquina virtual completa.

O empacotamento de aplicações no Docker se dá pela construção do que chamamos de *imagens*. Elas vão conter: o código do aplicativo, bibliotecas, ferramentas, dependências e outros arquivos necessários para executá-lo. São elas que podemos portar e rodar em vários hosts diferentes.

Assim, além de facilitar a execução de aplicações conteinerizadas, o Docker facilita também a portabilidade desses apps. Isso, porque esses pacotes são compartilháveis e construídos com o mínimo essencial para rodar a aplicação, além de otimizar a utilização de recursos do hospedeiro.

Em geral, o Docker usa como base um kernel de Linux do host para rodar seus contêineres. Mas, mesmo assim, um contêiner rodando num hospedeiro Linux terá seu próprio kernel subjacente. Já no caso de um hospedeiro com Windows o kernel próprio também estará lá, mas este é geralmente subjacente ao do WSL2. Por conta disso, é mais comum que sejam utilizadas imagens definidas para rodar mini ambientes Linux dentro do contêiner.

Um dos repositórios de imagens mais famosos, e onde muitas empresas mantém empacotamentos oficiais de suas aplicações, é o Docker Hub. Dele poderemos baixar imagens, rodá-las e até mesmo customizá-las. Link: [https://hub.docker.com/](https://hub.docker.com/)

Dado esse panorama geral, vamos à prática.

```powershell
docker run <nomeDaImagemLocal_ou_noDockerHub>
```

Com o Docker instalado se utilizamos o comando "*docker run"* para rodar uma imagem. Mas, se não temos ela salva localmente o que ocorre? É feito o download automático e em seguida um conteiner dela é iniciado. Lembrando que, para isso, o nome da imagem tem que ser referente a uma que tenhamos localmente ou uma que exista no Docker Hub.

```powershell
docker ps
```

Para verificar os contêineres que estão em execução no momento usamos *"docker ps"*. Ele nos retorna uma lista com o id do contêiner, o nome da imagem, o comando usado para rodá-lo, o tempo passado desde a criação, o status, as portas mapeadas do dele para o host e o seu nome (atribuído ou automático).

Vamos rodar, como exemplo, um contêiner de um sistema operacional Linux com a distribuição ubuntu.

```powershell
docker run -it ubuntu
```

Este comando roda o contêiner utilizando duas opções: -it. Essas duas opções em conjunto fazem com que um *terminal interativo* seja aberto logo em seguida. E neste terminal temos como manipular esta aplicação pela linha de comando.

Caso não utilizássemos as opções -it a aplicação também rodaria mas, no caso do ubuntu, terminaria sua execução logo em seguida porque essa imagem está programada para rodar apenas o bash e não aguardar por novos processos.

Uma forma de rodar essa aplicação em segundo plano de deixá-la rodando seria usar o seguinte comando:

```powershell
docker run -d ubuntu tail -f /dev/null
```

Neste caso, nenhuma interação com o terminal do contêiner é aberta de imediato. O comando *"tail -f /dev/null"* faz com que a execução da instrução rode indefinidamente. E a opção -d é responsável por rodar em segundo plano. Mas e agora, como faço para acessar o terminal da aplicação? Teremos que usar o comando *"exec".*

```powershell
docker exec -it <containerId_ou_nomeDoContainer> bash
```

Agora somos levados para dentro do terminal da aplicação. Mas como chegamos até aqui? *Executamos interativamente* o *terminal* e, além disso, executamos o */bin/bash*. Para sair usamos o comando *"exit"*, que seria o mesmo comando para fechar uma comunicação local ou remota com um terminal.

É importante notar que: este comando que roda e acessa o bash do contêiner não vai ser útil apenas quando estivermos executando um ubuntu dockerizado, ou qualquer outra imagem de distro Linux. Ele pode ser útil para qualquer outra imagem construída a partir de um Linux! Mesmo que seja uma imagem de um banco de dados, de uma aplicação web, de uma api, etc. Isso porque, como já mecionei acima, as imagens são comumente "definidas para rodar mini ambientes Linux dentro do contêiner" junto com a aplicação.

Voltando aos comandos. Se quisermos parar a execução de um contêiner é só utilizar o comando *"stop"*:

```powershell
docker stop <containerId_ou_nomeDoContainer>
```

Mas estes contêineres continuarão salvos numa lista com status de *Exited* (parado). Para visualizar isso usamos o seguinte comando:

```powershell
docker ps -a
```

Podemos reiniciar estes contêineres com o comando *"restart"*:

```powershell
docker restart <containerId_ou_nomeDoContainer>
```

Podemos também excluir os contêineres que não vamos mais utilizar:

```powershell
docker rm <containerId_ou_nomeDoContainer>
```

E podemos parar e já remover o contêiner utilizando a opção *force* junto com o *rm*:

```powershell
docker rm -f <containerId_ou_nomeDoContainer>
```

Além disso, para manipular as imagens que baixamos de forma básica podemos usar os comandos de listagem:

```powershell
docker images
```

Este comando vai mostrar informações como: nome da imagem no repositório e/ou local, tag (de versão ou atribuída), id da imagem, há quantos dias foi baixada e seu tamanho.

Para baixar uma nova imagem sem ter que rodar ela de imediato podemos utilizar o comando *pull*:

```powershell
docker pull <nome_da_imagem_no_repositório>
```

Opcionalmente, podemos designar uma imagem e uma versão específica para baixar (e o mesmo pode ser feito junto com o comando run). Caso contrário, a versão baixada será sempre a mais atual. As tags das versões se encontram no Docker Hub:

```powershell
docker pull <containerId_ou_nomeDoContainer>:<tag_da_versao>
```

Para remover uma imagem usamos:

```powershell
docker rmi <nomeDaImagem_ou_idDaImagem>
```

Caso haja várias imagens de mesmo nome, mas com versões diferentes, será necessário especificar a tag para remover, assim como fizemos para baixar.

Por fim, os comandos básicos que podem vir a ser necessários para checar se houve algum problema com nossos contêineres são os comandos de *log*.

Para checar os logs de um contêiner até o momento utilizamos:

```powershell
docker logs <containerId_ou_nomeDoContainer>
```

E para acompanhar os logs em tempo real utilizamos a opção -f com o comando de logs. Essa opção, neste caso, não quer dizer *force* mas sim *follow,* ou seja, estamos "seguindo" o que acontece no contêiner.

```powershell
docker logs -f <containerId_ou_nomeDoContainer>
```

De bônus, e talvez sendo um comando um pouco avançado mas bem importante, é sempre bom saber verificar o quanto o docker está utilizando de espaço no seu sistema. Há um comando para listar isso:

```powershell
docker system df
```

Há um comando para liberar espaço tomado por recursos não utilizados. O comando *docker system prune* remove todos os contêineres parados, todas as redes não utilizadas e todas as imagens não utilizadas (não associadas a nenhum contêiner em execução).

```powershell
docker system prune
```

E se algo não estiver funcionando como deveria, limpar o cache de build as vezes é uma saída interessante:

```powershell
docker builder prune
```

Por enquanto é só. Nos próximos artigos sobre o Docker vamos nos aprofundando um pouco mais. Abordaremos customização de imagens, utilização de Dockerfile e Docker Compose, comandos mais avançados, etc. E, por fim, a ideia final é ir montando também um memorex, igual ao que foi feito com o Git, para servir de referência rápida.

### Alocando recursos do host no Windows

No Windows, por conta do wsl2, podemos utilizá-lo para configurar qual o montante de memória RAM e/ou núcleos vamos deixar disponíveis para o uso do Docker.  
Para isso, basta criar um arquivo chamado *.wslconfig*, caso ele já não exista, na pasta do usuário do sistema que usa o WSL e o Docker. Neste arquivo vamos colocar as seguintes informações:

```plaintext
[wsl2]
memory=4GB
processors=2
```

No caso exemplo acima, estamos alocando 4 gigas para memória RAM e dois núcleos para uso dos contêineres.  
No Linux isso não é necessário. Pois, neste caso, o sistema do pinguim pode utilizar todos os recursos do sistema que estejam disponíveis a qualquer momento. Não há uma limitação configurável.