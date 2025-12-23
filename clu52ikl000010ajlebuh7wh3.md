---
title: "Paradigmas de Programação"
datePublished: Sun Mar 24 2024 05:17:52 GMT+0000 (Coordinated Universal Time)
cuid: clu52ikl000010ajlebuh7wh3
slug: paradigmas-de-programacao
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1711246725958/2b480c27-1486-4525-aa93-46d51f5b1f06.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1715099292039/7ec2a9d2-3876-40af-b139-fd7f1e602031.jpeg
tags: paradigmas

---

Paradigmas são identidades que definem as diversas estruturas possíveis para o código de uma aplicação. Eles expressam como um código deve funcionar, solucionar problemas e como vai ser a sua organização. Ou seja, é uma qualificação da código conforme sua funcionalidade.

Paradigmas também podem ser vistos como identidades para as algumas linguagens de programação. Algumas linguagens têm apenas um paradigma possível, podendo ser definidas como "puras". Outras tem origens em um paradigma específico mas acabam evoluindo para suportar diversos deles, as linguagens multi paradigma.

Existem dois grupos principais de paradigmas: Imperativos e Declarativos. Mas esta separação é mais didática do que uma representação perfeita da realidade.

### Paradigmas Imperativos

Neste paradigma estão os aplicativos construídos com foco em instruções sequenciais exatas, onde é visível o passo a passo do início ao fim de uma tarefa, e que modificam o estado do programa na sua conclusão.

A escolha desse paradigma é mais recomendada para aplicações que não precisem de manutenções a curto prazo.

Dentro desse grupo estão: (a) programação procedural; (b) programação orientada a objetos; e (c) computação paralela.

1. **Programação Procedural**:
    
    Muitas vezes usada como sinônimo do paradigma imperativo. Consiste numa lista de instruções passada em sequência e que podem ser agrupadas em procedimentos para reutilização.
    
    Exemplos de linguagens que dão suporte: C, C++, Java, Pascal.
    
    Indicação para uso: (a) quando o programa é estático e se espera que mude pouco e que poucos recursos sejam adicionados ao longo do tempo; (b) quando não há muita reutilização de código dentro do programa; (c) quando o procedimento a ser executado precisa de uma visualização clara das diferentes dependências que vão sendo chamadas a cada passo, conforme forem causando as mudanças de estado.
    
2. **Programação orientada a objetos:**
    
    Se baseia na capacidade de representar os problemas do mundo real, sobre o qual ela é aplicada, na estrutura do código escrito. Dentro de uma aplicação assim teremos parcelas do código separadas chamadas de Objetos que representam coisas do mundo real. Esses objetos terão características, os parâmetros; e comportamentos, os métodos. Esse tipo de estruturação apresenta alta capacidade de modularidade.
    
    Exemplos de linguagens que dão suporte: PHP, Java, Ruby, C#, Python, C++.
    
    Indicação para uso: (a) quando vários programadores atuam juntos e não precisam entender todo o código; (b) existe muito código a ser reutilizado; (c) tem mudanças previstas a longo prazo desde o início do projeto.
    
3. **Computação paralela:**
    
    Muitos computadores podem ser utilizados para chegar a um mesmo objetivo. Então, está enquadrado neste tipo uma aplicação que realize uma tarefa permitindo que muitos processadores executem seu código juntos, e em menos tempo.
    
    Exemplos de linguagens que dão suporte: C e C++.
    
    Indicação para uso: (a) quando um sistema possui mais de uma CPU ou processadores multi núcleo; (b) quando é preciso resolver problemas que podem levar dias para alcançar o objetivo; (c) simulação computacional, inteligência artificial ou modelagem que exija muitos cálculos dinâmicos.
    

### Paradigmas Declarativos

Aplicações nas quais o código foca em descrever o deve ser feito e não em indicar o passo a passo de como fazer. Em outras palavras, quando é informado ao computador apenas as propriedades do resultado desejado e não se informa o como fazer.

Temos também aplicações nas quais é feita a declaração de "verdades lógicas imutáveis" para que o computador possa chegar a um resultado.

Aqui temos um maior nível de abstração. E com isso estamos falando do processo evolutivo das linguagens de programação. Os programadores começaram a desenvolver aplicações em código binário, a forma concreta de comunicação do computador, sem abstração alguma da realidade da máquina. Passou-se a uma linguagem mais abstraída com o uso do Assembly. E a evolução se seguiu com Fortran, Cobol, etc., e depois com C, Pascal, etc., e em seguida C++, Java, etc., aos poucos elevando o nível de abstração. E os paradigmas também têm impacto nessa elevação, tanto que o paradigma declarativo pode ser considerado de nível de abstração mais alto que o imperativo.

Os programas costumam ser menores por terem menos código. E isso ocorre por conta da expressividade maior e da menor necessidade de delimitar os detalhes da implementação.

Dentro desse grupo estão: (a) paradigma de lógica de programação; (b) e paradigma funcional.

1. **Paradigma de lógica de programação:**
    
    Difere bastante dos demais pois não é composto por instruções. É baseado em declaração de fatos e cláusulas, relações lógicas e regras de inferência, para chegar a um objetivo.
    
    Indicação para uso: (a) é popular entre quem trabalha em universidades ou com inteligência artificial. Pode ser utilizado para projetos de comprovação de teoremas.
    
    Exemplos de linguagem que dão suporte: Absys, Ciao, Alice, Mercury, Prolog.
    
2. **Paradigma Funcional:**
    
    Recebe esse nome por se basear no uso de funções matemáticas puras. Ou seja, as aplicações são compostas por várias funções curtas que, como equações matemáticas, com certos valores de entrada resultam sempre os mesmos valores de saída.
    
    Aqui todas as variáveis tem escopo definido para a função. Assim, as funções não modificam nenhum valor fora do escopo e não são afetadas por nenhum valor fora do escopo delas.
    
    Indicação para uso: para casos em que há matemática envolvida diretamente na programação.
    
    Exemplos de linguagens que dão suporte: Haskell, Scala, Racket.
    
    Apesar destes exemplos citados, é importante salientar que muitas linguagens evoluíram no sentido de tirar benefícios deste paradigma. Várias linguagens hoje em dia têm bibliotecas nativas que trazem implementações imediatas do paradigma funcional. Um exemplo seria o Java, que desde o Java 8, mesmo sendo uma linguagem que foi originada para focar no paradigma de orientação a objetos, trouxe ferramentas para o uso do Paradigma Funcional.
    
    Outro exemplo, que nasceu do Paradigma Imperativo mas que é usado como exemplo de usabilidade Funcional é o JavaScript, por suas características de possuir funções de primeira classe e de ordem superior, recursão para iteração, imutabilidade e métodos de array de ordem superior.
    

Há também alguns paradigmas que são difíceis de enquadrar nesses dois grupos e vamos tratá-los em separado:

1. **Programação Baseada em Eventos:**
    
    Aplicações que são baseadas na ocorrência, captura e resposta a eventos.
    
    A indicação para uso é quando: desenvolvemos interfaces para usuários em navegadores web. Exemplos de linguagem que suportam isso: JavaScript.
    
2. **Programação Orientada a Aspectos (POA):**
    
    Que permite a modularização de aspectos transversais de uma aplicação, como logging e segurança. Com isso, esses setores da aplicação ficam separados do código principal. Exemplos de linguagem que suportam isso: AspectJ.
    

Por fim, gostaria de citar um tipo de linguagem que não entraria em nenhuma das listas por não se tratar de um tipo de "linguagem de programação completa", mas apenas de consulta, que seria o tipo de linguagem SQL. Utilizamos essas linguagens para fazer acesso de bancos de dados e retornar os dados persistidos neles. Por mais que não sejam utilizadas para construir aplicações é possível enquadrá-las, de certa forma, no paradigma descritivo. Já que quando as utilizamos não informamos os passos exatos que ela deve seguir. Além de terem um nível de abstração muito alto.

Este foi o primeiro artigo mais "teórico" do blog. Não pretendo trazer muitos desses, a princípio. Mas acredito que os temas teóricos mais básicos sejam importantes para que os desenvolvedores afinem entre si alguns entendimentos de nossa área de atuação.