---
title: "Configurando uma VM Linux no host Windows usando VirtualBox"
datePublished: Tue May 07 2024 06:00:41 GMT+0000 (Coordinated Universal Time)
cuid: clvvzf3pi00070ajwhlfhetkv
slug: configurando-uma-vm-linux-no-host-windows-usando-virtualbox
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1715054665217/650fab4d-7bf7-49fd-9cc7-0f033ae01126.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1715061612068/058e2a44-3d4f-4a9a-8256-e6a3d745eb51.jpeg
tags: virtual-machine, virtualbox

---

É comum que, ao construir softwares multiplataformas, seja necessário testar suas funcionalidades em diversos sistemas operacionais diferentes, para garantir que tudo esteja 100% funcional. Uma forma de fazer isso é, estando numa máquina Windows, por exemplo, configurar uma máquina virtual rodando Linux para esses fins de testagem. Este será o caso prático deste artigo. Para isso, vamos habilitar diversos recursos que facilitam a vida na hora de desenvolver uma solução: trocas fáceis de arquivos entre host e VM, conexão entre host e VM por meio de uma LAN virtual, configuração de usuário com privilégios de administrador do sistema virtual, etc.

Nos sistemas Windows mais recentes é necessário habilitar as ferramentas de virtualização. No menu Iniciar basta buscar por "Ativar ou Desativar Recursos do Windows" ou no caso em inglês "Turn Windows features on or off". Buscar a opção Hyper-V e habilitá-la (assim como suas opções internas). É interessante reiniciar o sistema em seguida.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1715055620493/364b36fb-477a-44e1-89b8-6f4113ac545e.png align="center")

O Windows já vem com uma ferramenta para virtualização chamada "Gerenciador de Hyper-V", que é uma ferramenta robusta. Mas, em nosso caso, vamos optar por baixar e usar o Virtual Box por ser mais simples e conter tudo o que precisaremos.

Além do Virtual Box precisaremos baixar também a ISO de uma distribuição Linux. Os sites oficiais do Virtual Box e de qualquer distribuição permitem os downloads gratuitos e seguros.

Após a instalação vamos criar uma nova máquina virtual:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1715055980628/bb74d0bb-ebeb-4ef1-a09a-993c278c4f05.png align="center")

Vamos escolher o nome de nossa VM e vamos apontar para a ISO que baixamos. Na versão atual do Virtual Box temos a instalação da ISO de maneira Desassistida. No caso vamos marcar a opção para "Pular \[a\] instalação desassistida" e vamos escolher quantidade de núcleos, memória ram e espaço em disco compartilhados do host para a VM. Recomendamos utilizar o que o Virtual Box sinalizar como quantidade em verde, que é o que poderá ser usado sem comprometer o host.

Em seguida, clicando nas configurações da VM que acabamos de criar, no menu Geral, na aba Avançado, já vamos habilitar a "Área de Transferência Compartilhada" e o "Arrastar e Soltar" como Bi-direcionais. Assim poderemos copiar e colar textos do host direto na VM, assim como arrastar e soltar arquivos para uma cópia simples.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1715056285999/560c2b43-dfa0-4c31-9e67-9f6d5e362c75.png align="center")

Desde a alteração do Xorg para o Wayland em algumas distribuições linux o arrastar e soltar de arquivos não raramente acaba em algum erro. Neste caso recomendamos outras formas de transferência de arquivos entre host e VM.  
Outra opção para compartilhamento de arquivos é, em Pastas Compartilhadas, adicionar uma pasta que contenha os arquivos que você gostaria que estivessem acessíveis na máquina virtual. É importante, habilitar a opção de "Montar Automaticamente". E, se não escolher um ponto de montagem, o Linux vai mostrar essa pasta no gerenciador de arquivos específico dele, na parte de "outros locais".

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1715059705299/1faad908-46d6-4fde-b43b-2ac0a5deb2d1.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1715059626791/c6e1dac4-b3b2-403a-8450-44446bc9593c.png align="center")

Na opção de Rede, na aba de Adaptador 2, vamos habilitar a placa de rede e vamos optar por conectá-la em "modo Bridge" e vamos escolher a placa de rede principal que host usa pra se conectar com o modem. Assim, teremos como fazer conexões entre o host e a VM, assim como com os outros aparelhos que estiverem conectados à mesma rede do seu modem. Isso é importante para testes de aplicações web, api's ou que estejam consumindo recursos de outros lugares da rede.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1715056331977/b9dd29bc-8178-46c5-80e0-7637235f3aaa.png align="center")

Primeiros passos prontos, agora basta iniciar a VM e fazer o processo de instalação do sistema operacional escolhido por meio da ISO.

O Linux deve iniciar sem opções de resolução de tela maiores que 800x600. Para resolver isso, devemos instalar os "Guest Additions" ("CD de Adicionais para Convidado"). Na tela da VM, clicando em Dispositivos, encontraremos a opção de instalação. O que esta opção faz é "inserir" virtualmente um CD, montando uma imagem ISO com os recursos necessários para fazer funcionar uma série de recursos que estarão faltando.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1715057164566/ad031474-6889-4747-a126-3d4045f55dcd.png align="center")

Geralmente, só com essa montagem o Linux vai reconhecer que há um instalador na pasta montada e vai pedir permissão para instalação. Caso isso não ocorra, basta entrar na pasta, pelo gerenciador ou pelo terminal e encontrar um arquivo que deve se chamar como "VBoxLinuxAdditions.run", ou algo parecido. Para rodá-lo basta usar o seguinte comando:

```bash
sudo sh VBoxLinuxAdditions.run
```

Em seguida as opções para aumentar a resolução da tela estarão habilitados. Talvez seja necessário fazer um log out ou reiniciar a VM.

Para verificar o ip da VM vamos utilizar algum dos seguintes comandos do linux:  
***ip a, ip addr,*** ou ***ip address.***

Assim teremos os IPs da máquina. Para testar se a máquina está acessível basta buscar o IP que está na mesma linha do gateway do modem e usar o comando *ping* de outro dispositivo. Caso os pings tenham sucesso temos acesso à comunicação com o Linux em VM a partir do host ou de dispositivos terceiros.

Talvez algumas tarefas acabem por exigir que nosso usuário principal tenha privilégios de administrador. Já que se trata de uma VM de teste não há grandes problemas em mexer com a segurança do sistema. Em outros casos, mexer nisso requer uma avaliação muito mais séria. Para isso, vamos fazer os seguintes passos:

1) Acessar o usuário root usando comando e senha:

```bash
su
<digitar senha do usuario root em seguida>
```

2) Editar o arquivo *sudoers*. No exemplo abaixo estamos usando o nano. Mas você pode usar o Vim ou outro de sua preferência. Caso não tenha um editor de texto instalado, é só instalar um usando o *apt install*.

```bash
sudo nano /etc/sudoers
```

3) Encontrar a linha na qual está escrito algo como:  
*%sudo ALL=(ALL:ALL) ALL*

4) Abaixo dela insira uma linha e escreva o seguinte:  
*&lt;***nomeDoSeuUsuario&gt; ALL=(ALL) ALL**

5) Se estiver usando o *nano* os comandos para salvar e sair são:  
**Ctrl + O** *(Salvar)* 6 - **Ctrl + X** *(Fechar )* 7 - **Ctrl + D** *(Sair do modo root)*

A partir disso seu usuário já fará parte da lista de usuários com privilégios de administrador do arquivo sudoers.

A partir disso temos tudo o que precisamos para fazer o deploy e testar todos os recursos de nossas aplicações num ambiente Linux de maneira fácil e rápida, habilitadas todas as ferramentas que vão ajudar a manejar esse ambiente de testes.