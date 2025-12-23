---
title: "Como testar o desempenho de requisições no Entity Framework do C#"
datePublished: Fri May 31 2024 07:08:47 GMT+0000 (Coordinated Universal Time)
cuid: clwucf53j001p09l52pt1f4la
slug: como-testar-o-desempenho-de-requisicoes-no-entity-framework-do-c
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1717113525702/9030c072-37e5-4a1e-baec-6f79b6289c75.jpeg
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1717139314735/f5589d2e-405c-4bd5-ae1c-5eac300d46c4.jpeg
tags: entity-framework, net, benchmark, dotnetcore

---

Se você é um desenvolvedor iniciante ainda não deve ter se preocupado diretamente com o desempenho de requisições feitas por suas aplicações aos bancos de dados. E realmente, essa preocupação é secundária no momento em que você está aprendendo os primeiros passos de como montar *querys* ou utilizar ferramentas de *ORM* para ter o acesso ou persistir novos dados no banco. Mas, lhe asseguro que ao fazer trabalhos que exigem mais profissionalidade, e isso acontece bem cedo nessa carreira, essa necessidade vai se tornar urgente.  
Não demora muito pra que aquela aplicação bem polida que você fez, toda trabalhada nos princípios do *SOLID* e do *Clean Code*, comece a demorar demais quando o assunto é fazer um simples *select* na *database*.  
Por isso, hoje vamos aprender a fazer uma aplicação de console para que seja possível fazer benchmarks das suas requisições no Entity Framework do C#. E, para isso, vamos utilizar da biblioteca *BenchmarkDotNet*.

Vamos iniciar criando um projeto tipo "*Console App*" no Visual Studio Community. Para esse exemplo, vou estar usando a versão .NET 8.0.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1717130477216/a0fa2881-4b85-4e4c-9f5c-5f156009d3dc.png align="center")

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1717130528113/f47d8b16-f538-4a23-93b9-159c14b3a7a9.png align="center")

Dei o nome de *EFPerformanceTest* para este projeto.  
Você também pode criar ele pelo Terminal usando o seguinte comando:

```bash
dotnet new console -n EFPerformanceTest
cd EFPerformanceTest
```

No Visual Studio Community você pode utilizar a ferramenta visual do NuGet para instalar os seguintes packages/bibliotecas:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1717130763744/acd62872-6fdc-4b87-9758-f18ade86f549.png align="center")

Estaremos instalando aqui todos os pacotes relacionados ao Entity Framework, inclusive um deles para caso você queira usar o SQL Lite e outro para SQL Server, além do pacote do BenchmarkDotNet.  
É possível também instalar via linha de comando:

```bash
dotnet add package Microsoft.EntityFrameworkCore
dotnet add package Microsoft.EntityFrameworkCore.SqlLite
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
dotnet add package Microsoft.EntityFrameworkCore.Tools
dotnet add package BenchmarkDotNet
```

Feito isso, vamos configurar nosso projeto.  
Para os testes que desejo mostrar vamos criar algumas entidades: *Person*, *Address* e *Graduation*. Vamos criar uma pasta *Models* na raiz do projeto e colocar essas classes dentro dela.

```csharp
namespace EFPerformanceTest.Models;

public sealed class Person
{
    public int Id { get; set; }
    public required string Name { get; set; }
    public required int Age { get; set; }
    public int? AddressId { get; set; }
    public Address? Address { get; set; }

    public List<Graduation> Graduations { get; set; }
}
```

```csharp
namespace EFPerformanceTest.Models;

public sealed class Address
{
    public int Id { get; set; }
    public required string Street {  get; set; }
    public required int Number{  get; set; }
    public required string City { get; set; }
    public required string State { get; set; }
    public required string Zipcode { get; set; }
}
```

```csharp
namespace EFPerformanceTest.Models;

public sealed class Graduation
{
    public int Id { get; set; }
    public required string Name { get; set; }
    public required DateOnly AccomplishDate { get; set; }

    public List<Person> Persons { get; set; }
}
```

Note que cada Pessoa (Person) possui uma lista de Graduações (Graduations) possíveis.

Em seguida, vamos configurar o nosso *DbContext* dentro da pasta *Data*:

```csharp
using EFPerformanceTest.Models;
using Microsoft.EntityFrameworkCore;

namespace EFPerformanceTest.Data;

public sealed class AppDbContext : DbContext
{
    public DbSet<Person> Person { get; set; }
    public DbSet<Address> Address { get; set; }
    public DbSet<Graduation> Graduation { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        //optionsBuilder.UseSqlite("Data Source=EFPerformanceTest.db",
        //    options => options.MigrationsAssembly(typeof(AppDbContext).Assembly.FullName));

        optionsBuilder.UseSqlServer("Server=(localdb)\\mssqllocaldb;Database=EFPerformanceTestDb;Trusted_Connection=True;",
            options => options.MigrationsAssembly(typeof(AppDbContext).Assembly.FullName));
    }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Person>()
            .HasOne(p => p.Address)
            .WithOne()
            .IsRequired(false)
            .HasForeignKey<Person>(p => p.AddressId)
            .HasConstraintName("FK_Person_Address")
            .OnDelete(DeleteBehavior.Cascade);

        modelBuilder.Entity<Person>()
            .HasMany(p => p.Graduations)
            .WithMany(g => g.Persons)
            .UsingEntity<Dictionary<string, object>>("PersonsGraduations", config =>
            {
                 config.HasOne<Person>().WithMany().HasForeignKey("PersonId");
                 config.HasOne<Graduation>().WithMany().HasForeignKey("GraduationId");
            });
    }
}
```

Note também que, aqui, nós deixamos comentada a opção de utilizar o SQL Lite. Isso porque, vamos utilizar a ferramenta de desenvolvimento *"SQL Server Express LocalDB".* Este é um banco de dados local, para fins de testes, que geralmente já se encontra instalado junto com o Visual Studio Community.  
Para acessá-lo basta utilizar o *SQL Server Object Explorer* abrindo o VS Community e clicando em **View &gt; SQL Server Object Explorer.** E nele nós criamos a database que vamos utilizar, dando o nome de *EFPerformanceTestDb* conforme visto na string de conexão do DbContext.  
Outra coisa que você pode notar é que o formato que deixamos o método *OnConfiguring* mostra que usaremos Migrations para modelar nosso banco de dados no formato *CodeFirst*. E que, se quisermos, podemos ter migrations separadas para ser usadas tanto em um banco SQL Server como em um banco SQL Lite.  
E, por fim, em *OnModelCreating* criamos os relacionamentos importantes para estabelecer as ligações entre nossas entidades.

O próximo passo é configurar a classe que vai configurar nossas requisições para o banco e estabelecê-las como benchmarks. vamos criar uma pasta chamada *Benchmarks* e vamos criar a classe *EFPerformanceBenchmarks* dentro dela:

```csharp
using BenchmarkDotNet.Attributes;
using EFPerformanceTest.Data;
using EFPerformanceTest.Models;
using Microsoft.EntityFrameworkCore;

namespace EFPerformanceTest.Benchmarks;

[MemoryDiagnoser]
public class EFPerformanceBenchmarks
{
    private readonly AppDbContext _context;

    public EFPerformanceBenchmarks()
    {
        _context = new AppDbContext();
    }

    [GlobalSetup]
    public void Setup()
    {
        _context.Database.Migrate();
    }

    [Benchmark]
    public void AddPerson()
    {
        var person = new Person
        {
            Name = "John Doe",
            Age = 30,
            Graduations =
            [
                new() 
                {
                    Name = "Psychology",
                    AccomplishDate = new DateOnly(2010, 05, 21)
                }
            ]
        };
        _context.Person.Add(person);
        _context.SaveChanges();

        var person2 = new Person
        {
            Name = "Alexandre Lopes",
            Age = 35,
            Graduations =
            [
                new()
                {
                    Name = "Psychology",
                    AccomplishDate = new DateOnly(2010, 05, 21)
                },
                new()
                {
                    Name = "Systems Development",
                    AccomplishDate = new DateOnly(2020, 01, 31)
                }
            ]
        };
        _context.Person.Add(person2);
        _context.SaveChanges();
    }

    [Benchmark]
    public void QueryPersonsIncludeWithFilterInside()
    {
        var personList = 
            _context.Person
                .AsNoTracking()
                .Include(p => p.Graduations
                    .Where(g => g.Name == "Systems Development"))
                .ToList();
    }

    [Benchmark]
    public void QueryPersonsWithoutIncludeAndAddGraduationAfter()
    {
        var personList =
            _context.Person
                .AsNoTracking()
                .ToList();

        foreach (var person in personList)
        {
            var filteredGraduations = _context.Entry(person)
                .Collection(p => p.Graduations)
                .Query()
                .Where(g => g.Name == "Systems Development")
                .ToList();

            person.Graduations = filteredGraduations;
        }
    }

    [Benchmark]
    public void QueryPersonsProjectIncludesInsideNewObject()
    {
        var personList =
            _context.Person
                .AsNoTracking()
                .Select(p => new Person
                {
                    Id = p.Id,
                    Name = p.Name,
                    Age = p.Age,
                    AddressId = p.AddressId,
                    Address = p.Address,
                    Graduations = p.Graduations
                        .Where(g => g.Name == "Systems Development")
                        .ToList()
                })
                .ToList();
    }
}
```

Cada um dos métodos com a annotation *Benchmark* vai ser chamado num teste separado, ter seus dados de performance medidos e retornados no final do processo.  
Também adicionamos um método de *Setup* para garantir que todas as migrations vão ser atualizadas no banco antes de iniciar um benchmark.

Nosso *Program.cs* ficará assim:

```csharp
using BenchmarkDotNet.Running;
using EFPerformanceTest.Benchmarks;

BenchmarkRunner.Run<EFPerformanceBenchmarks>();
```

Para executar as migrations antes de rodarmos os testes podemos usar os seguintes comandos:

```bash
# Criar o arquivo de migrations colocando ele numa pasta output chamada:
# SqlServerMigrations. Para definir para qual banco estamos criando as migrations
# devemos comentar 'optionsBuilder.Use...' não desejados no DbContext
dotnet ef migrations add InitialCreate -o SqlServerMigrations
# aplicar as migrations na base de dados:
dotnet ef database update
```

Enfim, para rodar o benchmark é necessário ir pela linha de comando e utilizar os seguintes:

```bash
dotnet build -c Release
dotnet run -c Release
```

Se tudo foi configurado da maneira correta o benchmark se iniciará e o terminal vai nos mostrar todas as análises que estiver fazendo e os possíveis erros que cometemos.

Na documentação do BenchmarkDotNet ([aqui](https://fransbouma.github.io/BenchmarkDotNet/HowItWorks.htm)) encontramos a seguinte explicação do que é feito:

> Ele gera um projeto isolado para cada método de benchmark, job e parâmetro, e então compila esse projeto em modo Release (e talvez por isso, quando usamos o SQL Lite não conseguimos verificar os dados persistidos no banco, após todas as operações finalizarem).  
> Em seguida, ele executa o método de benchmark várias vezes para medir seu desempenho.  
> Durante a execução, o BenchmarkDotNet realiza diferentes tipos de iterações, incluindo Pilot, IdleWarmup, IdleTarget, MainWarmup e MainTarget. Cada uma dessas iterações serve a um propósito específico, como selecionar o melhor número de operações, avaliar o overhead do BenchmarkDotNet e realizar medidas principais.  
> Após todas as medições, o BenchmarkDotNet cria uma instância da classe Summary contendo todas as informações sobre as execuções dos benchmarks, além de arquivos com resumos em formatos legíveis por humanos e máquinas, e conjuntos de gráficos.

Ao final do processo, teremos a saída dos dados no terminal e também um arquivo de log dessa mesma saída numa pasta chamada *BenchmarkDotNet.Artifacts.*  
A princípio os dados que vão nos interessar são os seguintes. E eles vão ser apresentados assim:

```plaintext
// * Summary *

BenchmarkDotNet v0.13.12, Windows 11 (10.0.22631.3593/23H2/2023Update/SunValley3)
AMD Ryzen 5 3600, 1 CPU, 12 logical and 6 physical cores
.NET SDK 8.0.204
  [Host]     : .NET 8.0.4 (8.0.424.16909), X64 RyuJIT AVX2
  DefaultJob : .NET 8.0.4 (8.0.424.16909), X64 RyuJIT AVX2


| Method                                          | Mean     | Error    | StdDev   | Gen0       | Gen1       | Gen2      | Allocated |
|------------------------------------------------ |---------:|---------:|---------:|-----------:|-----------:|----------:|----------:|
| AddPerson                                       | 127.7 ms | 18.06 ms | 52.96 ms | 17210.9375 | 140.6250   | 23.4375   | 139.03 MB |
| QueryPersonsIncludeWithFilterInside             | 142.6 ms | 0.77 ms  | 0.69 ms  | 4750.0000  | 1750.0000  | 750.0000  |  39.27 MB |
| QueryPersonsWithoutIncludeAndAddGraduationAfter | 6.830 s  | 0.0882 s | 0.0736 s | 98000.0000 | 33000.0000 |           | 783.19 MB |
| QueryPersonsProjectIncludesInsideNewObject      | 154.1 ms | 0.80 ms  | 0.71 ms  | 4000.0000  | 1750.0000  | 750.0000  |  33.12 MB |
```

#### Legendas:

* **Method**: O nome do método benchmark.
    
* **Mean**: O tempo médio gasto na execução do método.
    
* **Error**: O erro padrão do tempo médio.
    
* **StdDev**: O desvio padrão do tempo médio.
    
* **Allocated**: A quantidade de memória alocada durante a execução do benchmark.
    

Perceba que temos os dados dos quatro métodos que queremos analisar da nossa classe *EFPerformanceBenchmarks*.  
Decorrendo desses dados, podemos verificar que o método *QueryPersonsIncludeWithFilterInside()* foi o mais performático, sendo que deveríamos optar por ele no caso de uma implementação de um caso real similar a este.  
Vemos que não é trivial essa escolha. Isso, porque temos uma diferença significativa entre o mais performático, que levou **0,1426 s**, e o menos performático que levou **6,830 s.** Uma diferença de 47 vezes entre um e outro. Diferença que tenderia a aumentar no caso desse método sofrer alterações para adicionar mais *Includes*, num caso muito comum de adições posteriores na manutenção do código.

Espero ter ajudado a entender o quão importante testes como estes são para a longevidade e eficácia das suas aplicações.

Uma versão um pouco modificada deste código pode ser encontrada no meu GitHub. Lá adicionei também a possibilidade de verificar os dados retornados pelos métodos que analisamos aqui num arquivo de log. O link para o projeto é:  
[https://github.com/AlexandreGLopes/EntityFrameworkPerformanceTest](https://github.com/AlexandreGLopes/EntityFrameworkPerformanceTest)