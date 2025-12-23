---
title: "Noções básicas de Clean Code (Código Limpo)"
datePublished: Wed May 22 2024 04:10:28 GMT+0000 (Coordinated Universal Time)
cuid: clwhb35on000209mf3soia77d
slug: nocoes-basicas-de-clean-code-codigo-limpo
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1715665158624/0ac37d5e-cfe0-4f80-8e50-4c23c96f3e78.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1716351000320/0d3dd717-c850-484b-a707-725f0ef90791.png
tags: clean-code

---

Clean Code é um agrupado de boas práticas e recomendações para tornar seu código mais fácil de entender, manter e modificar. Esse conceito surgiu a partir do livro "*Clean Code: A Handbook of Agile Software Craftsmanship*", do autor Robert C. Martin. Ele nos ensina que a eficiência do seu código depende não só dos resultados que ele gera, mas da clareza dele também.

As "regras" do código limpo são várias e mudam um pouco de autor para autor. Mas, vamos listar abaixo algumas que consideramos mais basais.

### 1) Dê nomes significativos

Nomeie suas variáveis, funções, métodos, etc. com descrições significativas e claras daquilo que eles representam.  
Nada de nomear aquela sua variável com apenas uma letra, ou com algo que não remete diretamente ao dado que ela vai guardar ou retornar.  
Não faça isso:

```java
int d = 1;
double s_2133 = 2000.50;
```

Faça isso:

```java
int dia = 1;
double saldo = 2000.50;
```

### 2) Use nomes passíveis de busca

Nomeie suas variáveis, funções, métodos, etc. com nomes passíveis de busca, ou seja, que evitem ambiguidade. Um nome passível de busca deve seguir a regra de ser significativo, mas também ser específico o suficiente pra que seja encontrado rapidamente numa busca dentro do código.  
Com o exemplo ficará mais fácil de entender.  
Não faça isso:

```java
// Nem significativo e nem passível de busca
String e = "Erro ao processar o pedido";

// Passível de busca, mas não significativo
String errDet = "Erro ao processar o pedido";

// Significativo, mas não passível de busca
String erro = "Erro ao processar o pedido";
```

Faça isso:

```java
String exceptionDetail = "Erro ao processar o pedido";
```

### 3) Trocar "números mágicos" por constantes

Números mágicos são valores numéricos que quando são encontrados por você num código você geralmente se pergunta: "o que esse número representa"?  
São valores desprovidos de contextualização.  
Não faça isso:

```java
if (idade > 18) {
    System.out.println("Você é um adulto.");
}
```

Faça isso:

```java
public static final int IDADE_ADULTA = 18;

if (idade > IDADE_ADULTA) {
    System.out.println("Você é um adulto.");
}
```

### 4) Utilizar booleanos de forma implícita

Procure sempre retornar diretamente o resultado de uma expressão booleana. Isso deixará seu código mais conciso, claro e menos redundante. Além de tudo, evitará bugs por conta de um entendimento errado da expressão booleana por parte do desenvolvedor.  
Não faça isso:

```java
public boolean isPositive(int number) {
    if (number > 0) {
        return true;
    } else {
        return false;
    }
}
```

Faça isso:

```java
public boolean isPositive(int number) {
    return number > 0;
}
```

### 5) Evitar condicionais negativos

Geralmente, temos mais dificuldades para ler e entender proposições negativas do que positivas.  
A prova disso é que: "não sabemos o quanto poderíamos criar se não faltassem as habilidades que não possuímos". Difícil, né?  
Piadas à parte, isso ocorre por conta das proposições negativas da frase. Por isso devemos optar, quando possível, por condicionais positivos.  
Evite isso:

```java
if (!isValid) {
    // TO DO
}
```

Privilegie usar isso quando possível:

```java
if (isValid) {
    // TO DO
}
```

### 6) Encapsule condicionais

Quando uma expressão condicional já não for simples por ela mesma é interessante movê-la para um método separado e dar a ele um nome significativo. Isso vai ajudar na clareza e legibilidade dessa expressão, além de deixá-la pronta para reutilização.  
Não faça isso:

```java
public void verificarAcesso(Usuario usuario) {
    if (usuario.getPapel().equals("ADMIN") && usuario.estaAtivo()) {
        // Concede acesso
        System.out.println("Acesso concedido.");
    }
}
```

Faça isso:

```java
public void verificarAcesso(Usuario usuario) {
    if (temAcessoAdministrador(usuario)) {
        // Concede acesso
        System.out.println("Acesso concedido.");
    }
}

private boolean temAcessoAdministrador(Usuario usuario) {
    return usuario.getPapel().equals("ADMIN") && usuario.estaAtivo();
}
```

### 7) "Inverta if's", ou melhor, retorno antecipado para evitar aninhamento excessivo de blocos

É comum que desenvolvedores menos experientes acabem por usar muitos blocos "if/else". Geralmente também usem muitos blocos de "do/while" e "for"s também. E que, além disso, acabem por colocar vários desses blocos uns dentro dos outros.  
Isso gera um código complexo e difícil de manter. Mas, ao "inverter um condicional if" é possível evitar o bloco "else" e tornar o código mais limpo e eficiente.  
Veja o exemplo:

```java
public void processarPedido(Pedido pedido) {
    if (pedido.isPago()) {
        processarPedidoPago(pedido);
    } else {
        processarPedidoNaoPago(pedido);
    }
}
```

E agora veja com a inversão do "if":

```java
public void processarPedido(Pedido pedido) {
    if (!pedido.isPago()) {
        processarPedidoNaoPago(pedido);
        return;
    }
    
    processarPedidoPago(pedido);
}
```

Perceba que, neste caso específico, é preferível utilizar uma expressão condicional negativa, diferente do que falamos na regra número 5. Mas, porque neste caso o ganho é muito maior. Um ganho de concisão no código. E também, um ganho na estrutura que vai moldá-lo, a fim de evitar que se tenha muitos blocos dentro de outros blocos conforme a lógica for sendo incrementada.

### 8) Não fazer comentários irrelevantes

O que seria um comentário irrelevante? Segue a lista: (a) comentários óbvios, (b) comentários desatualizados, (c) comentários redundantes, (d) comentários de histórico (aqueles que tendem a versionar o código... Sendo que, temos ferramentas pra isso, como o Git), etc.  
Mas nem todo comentário é ruim. Alguns tipos deles estão na lista das boas práticas. A saber:  
(a) *Explicativos*: que explicam porque uma decisão de design não óbvia foi tomada;  
(b) *Clarificadores*: que explicam partes complexas e/ou contraintuitivas;  
(c) *Temporários*: que indicam necessidade de revisão ou remoção (os famosos "TO DO"s);  
(d) *Documentação de Funções*: aqueles que documentam a funcionalidade e o uso de funções e métodos.

Mas, lembre-se: COMENTÁRIOS *NÃO* DEVEM EXPLICAR O CÓDIGO. PELO CONTRÁRIO, OS NOMES DAS FUNÇÕES OU MÉTODOS É QUE DEVEM.

### 9) Dar preferência a funções nativas

Funções nativas são aquelas fornecidas pela linguagem especifica usada ou pelas bibliotecas implementadas. Elas costumam ser bem otimizadas para um melhor desempenho. Ou seja, são mais rápidas, mais confiáveis e serão de manutenção mais fácil por parte de outros desenvolvedores futuros.  
Não faça isso:

```java
List<String> lista = Arrays.asList("a", "b", "c");
int tamanho = 0;
for (String item : lista) {
    tamanho++;
}
```

Faça isso:

```java
List<String> lista = Arrays.asList("a", "b", "c");
int tamanho = lista.size();
```

### 10) Quebrar funções maiores em menores

Aqui temos um *crossover* entre "código limpo" e o princípio SRP do "*SOLID*". Inclusive, este último será tema de uma publicação futura aqui no blog.  
Mas voltando ao tema, essa regra vai no sentido do princípio de que uma função ou método deve ter apenas uma responsabilidade. Ou seja, deve fazer apenas uma coisa.  
Isso vai melhorar seu código nos quesitos de legibilidade, reutilização, manutenção e testabilidade.  
Veja no exemplo abaixo que temos um método antes de qualquer quebra. Veja que ele estará fazendo mais de uma função:

```java
public class Calculadora {

    public double calcularPrecoFinal(double precoInicial, double taxaImposto, double desconto) {
        double precoComImposto = precoInicial * (1 + taxaImposto);
        double precoComDesconto = precoComImposto * (1 - desconto);
        return precoComDesconto;
    }
}
```

Agora vamos aplicar o princípio de SRP e vamos dividir as funcionalidades de cálculo de imposto e desconto em dois outros métodos separados.  
Pode parecer a um desenvolvedor mais inexperiente que estamos complexificando demais, ou gastando mais linhas de código sem necessidade. Mas, na verdade, a medida em que encontramos métodos com regras de negócio mais complexas, e com mais necessidades, os benefícios de ter optado por esse formato seccionado vão ser muitos, deixando nosso código muito eficiente.

```java
public class Calculadora {

    public double calcularPrecoFinal(double precoInicial, double taxaImposto, double desconto) {
        double precoComImposto = calcularPrecoComImposto(precoInicial, taxaImposto);
        double precoComDesconto = calcularPrecoComDesconto(precoComImposto, desconto);
        return precoComDesconto;
    }

    private double calcularPrecoComImposto(double precoInicial, double taxaImposto) {
        return precoInicial * (1 + taxaImposto);
    }

    private double calcularPrecoComDesconto(double preco, double desconto) {
        return preco * (1 - desconto);
    }
}
```

### 11) Evitar "flags" em funções e métodos

Se você não sabe o que é uma flag eu explico. Elas são booleanos passados como parâmetros dentro de uma função ou método para que uma condicional dela faça ou não uma determinada tarefa. E, se você está com a regra anterior fresca na memória, vai perceber que isso indica que essa função está quebrando o princípio SRP: possivelmente esteja fazendo mais de uma coisa.  
Para solucionar isso é possível usar duas saídas: (a) quebrar ela em duas ou mais funções com parâmetros específicos ou (b) usar o *polimorfismo*.  
Vamos ver um exemplo de código com flag:

```java
public void processarPedido(Pedido pedido, boolean enviarEmail) {
    // Processamento do pedido
    System.out.println("Pedido processado.");
    if (enviarEmail) {
        enviarEmailConfirmacao(pedido);
    }
}
```

Agora vamos quebrar esse método para respeitar o SRP removendo a flag:

```java
public void processarPedido(Pedido pedido) {
    // Processamento do pedido
    System.out.println("Pedido processado.");
}

public void processarPedidoComEmail(Pedido pedido) {
    processarPedido(pedido);
    enviarEmailConfirmacao(pedido);
}
```

E podemos também usar o polimorfismo, fazendo duas classes que implementam uma interface:

```java
public interface PedidoProcessor {
    void processarPedido(Pedido pedido);
}

public class PedidoProcessorComEmail implements PedidoProcessor {

    @Override
    public void processarPedido(Pedido pedido) {
        // Processamento do pedido
        // (...)
        enviarEmailConfirmacao(pedido);
    }

    private void enviarEmailConfirmacao(Pedido pedido) {
        // Lógica para enviar o e-mail de confirmação
    }
}

public class PedidoProcessorSemEmail implements PedidoProcessor {

    @Override
    public void processarPedido(Pedido pedido) {
        // Processamento do pedido
    }
}
```

Em conclusão, espero ter ajudado a dar uma introdução rápida sobre "código limpo" e sobre sua importância. Espero que tenha ajudado e que inspire mesmo quem está no início da carreira. Existe muito mais sobre o assunto, e a ideia é trazer mais das questões relacionadas no futuro.

Referência:  
[https://medium.com/desenvolvendo-com-paixao/2-clean-code-boas-pr%C3%A1ticas-para-escrever-c%C3%B3digos-impec%C3%A1veis-361997b3c8b5](https://medium.com/desenvolvendo-com-paixao/2-clean-code-boas-pr%C3%A1ticas-para-escrever-c%C3%B3digos-impec%C3%A1veis-361997b3c8b5) (acesso em 01/2024).

Martin(2008) Robert C. Martin. Clean Code - A Handbook of Agile Software Craftsmanship. Prentice Hall.