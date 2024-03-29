# [Java 11] Orientação a Objetos em Java
Autor: [Gabriel Martins dos Santos - *Java Software Engineer*](https://linkedin.com/in/martinsgms)

Caderno de estudos para certificação OCJP11 (1z0-819).

---

## Modificadores de acesso (classes)
Classes só podem receber modificadores de acesso `public` e `abstract` ou `final`.
````Java
// MinhaClasse.java
public [abstract|final] class MinhaClasse {}
````
Classes `public` devem ser declaradas em seu próprio arquivo. Outras classes (ou enum, interface, annotation) podem ser declaradas no mesmo arquivo, desde que não sejam declaradas com `public`.
````Java
// MinhaClasse.java
public [abstract|final] class MinhaClasse {}
[abstract|final] class MinhaClasse2 {}
[abstract] interface MinhaInterface {}
[abstract] @interface MinhaAnnotation {}
enum MeuEnum {}
````
## O que é um membro?
Na prática, **membro** é tudo aquilo que você pode acessar com `.` (ponto - operador de acesso). Isto é: 
- métodos
- atributos (variaveis e constantes)
- *nested types* (*inner classes, enums, interfaces*)

**Membros são herdados por suas subclasses.**

## O que NÃO É um membro?
- construtores
- blocos de inicialização

## Modificadores de acesso (visibilidade)
Aplicável a membros e construtores. Aplicando o modificador, o membro será visível...

| Modificador  | Própria classe | Pacote | Filhas | Módulo
|-------|-------|-------|-------|-------|
**private** | :white_check_mark: | :x: | :x: | :x:
**package *(default)*** | :white_check_mark: | :white_check_mark: | :x: | :x:
**protected** | :white_check_mark: | :white_check_mark: | :white_check_mark: | :x:
**public** | :white_check_mark: | :white_check_mark: | :white_check_mark: | :white_check_mark:

### Ordem de Restritividade
private **é+q** package **é+q** protected **é+q** public

> Decoreba p/ decorar a ordem: (pri) pa-pro-pu

## Modificadores `abstract` e `final`
**Atenção!** Estes dois modificadores se contradizem, eles **NUNCA** podem ser usados juntos.

### Em classes

| Modificador  | Função |
|-------|-------|
**final** | indica que a classe não poderá ser extendida (herança).  
**abstract** | indica que a classe não poderá ser instânciada.

### Em atributos

| Modificador  | Função |
|-------|-------|
**final** | indica uma constante. A inicialização é obrigatória no momento da inicializaçãoe e seu valor não poderá ser alterado. 
**abstract** | N/A.

### Em métodos
| Modificador  | Função |
|-------|-------|
**final** | indica que o método NÃO PODE ser sobrescrito.  
**abstract** | indica que o método PRECISA ser sobrescrito.

## Outros modificadores
| Modificador  | Função |
|-------|-------|
**strictfp**  | ...
**volatile**  | ...
**transient**  | ...
**native**  | ...

## Abstract Class
São classes que não podem ser instanciadas. São destinadas a agrupar funcionalidades em comum, que poderão tomar diferente formas em cada classe concreta que a extende.  
Se assemelha à uma classe concreta. A diferença é que não pode ser instanciada diretamente e também pode declarar métodos abstract.
````Java
abstract class Animal {}
````
### Regras
- todo método `abstract` *precisa* ser implementado (em classes concretas)
- podem existir métodos concretos, que podem ser sobrescritos assim como os métodos `abstract`
- *abstract classes* podem ter atributos e *inner types*
- *abstract classes* não podem ser `final`. Estes modificadores juntos, se anulam.
- métodos abstract não podem ser `private`, porque senão nenhuma outra classe poderá acessa-lo
- métodos abstract não podem ser static, porque métodos static não podem ser sobrescritos
- métodos static não são válidos em sobrescrita
- construtor abstrato não existe

**Atenção!** Sempre observar se os construtores entre classe e subclasse estão satisfeitos. Lembrar que há uma chamada implícita para o *no-args constrcutor* nos construtores das subclasses.

### O que é Assinatura do Método?
Nome do método + parâmetros. Apenas.

## Diferença entre Overriding e Overloading
**Overloading** dá suporte a um método, disponibilizando mais opções de parâmetros.

**Overriding** ocorre entre classe concreta e superclasse, ao declarar um "mesmo método" na classe concreta, customizando seu comportamento (implementação).
> Obs: a anotação `@Override` não é obrigatória.

| Métodos devem ter... | Overload | Override
|-------|----------|---------|
| mesmo nome | :white_check_mark: | :white_check_mark:
| mesmo número de parâmetros | :x: | :white_check_mark:
| mesmo tipo e ordem de parâmetros | :x: | :white_check_mark:
| mesmo tipo de retorno (ou covariante) | irrelevante | :white_check_mark:
| mesma visibilidade, ou menos restritiva | irrelevante | :white_check_mark:
| throws | irrelevante | não obrigatório, mas se houver, a Exception precisa ser o tipo ou subtipo

A semelhança entre override e overload é basicamente que o método precisa ter o mesmo nome. Um método overload é um novo método.  
Métodos override ocorrem quando se quer extender um método, porque se quer, ou porque se é forçado.  
A principal vantagem do poliforfismo é poder chamar uma classe sem saber qual classe será chamada.

## Interface
É uma classe abstrata que 
representa uma ***intenção geral*, que pode tomar diferentes *formas específicas*** (polimorfismo). É o mais alto nível de abstração e de baixo acoplamento possível ao definir uma classe.  
Exemplo: A *intenção geral* é calcular um imposto, e eu posso calcular diferentes impostos, cada um de *forma específica*.  

> É uma forma de definir um (ou N) comportamento que será comum para um diferentes tipos. Cada tipo decide a forma como vai desempenhar o comportamento.

Interfaces mais conhecidas: `java.lang.Comparable`, `java.util.List`...

````Java
public interface Imposto {
    int getAliquota();
    int calculaImposto();
}
````

> Interfaces são ótimas para implementar polimorfismo. A intenção que definem pode tomar diferentes formas baseado no objeto que foi criado.

Além disso, **Interfaces são stateless**, isto é, não possuem estado, não carregam valores.

### O que é permitido em Interfaces
Uma interface pode conter...
- constantes: todas as variáveis são `public static final` por padrão.
- assinaturas de métodos
- métodos `private`* (java 9)
- métodos `default`* (java 8)
- métodos `static`* (java 8)
- *nested types (inner class, enum, interface)*

\* Apenas estes métodos devem possuir *body*. (`d.p.s`)

### Quem pode sobrescrever quem

| super/sub | default | static | private |
|---------|---------|---------|---------|
| **private** | :x: | :x: | :x:
| **default** | :white_check_mark: | :x: | :x:
| **static** | :white_check_mark: | :white_check_mark: | :x:

### Hiding
Métodos `default` podem "esconder" métodos abstratos

### Functional Interface
São interfaces que possuem **apenas um** método abstrato

### Particularidades
#### 1. Modificador *default* em métodos
Os **métodos** são `public abstract` por padrão (caracteristica que faz com que as classes *concretas* sejam obrigadas a implementar seus métodos). Isto é:
````Java
void getAliquota();
Double calculaImposto();
````
é o mesmo que:
````Java
public abstract void getAliquota();
public abstract Double calculaImposto();
````
#### 2. Métodos `private`
...
#### 3. Métodos `default`  
...
#### 4. Métodos `static`  
...
#### 5. Métodos `strictfp`
...
#### 6. Métodos `native`
...
#### 5. Constantes
As **constantes** são `public`, `static` e `final` por padrão. E podem ser acessadas via *herança*, ou de forma *direta*, i.e. `Interface.CONSTANTE`.
````Java
int ZERO = 0;
````
é o mesmo que 
````Java
public static final int ZERO = 0;
````

### Ambiguidade e Conflito

#### Entre métodos default de duas interfaces distintas
Uma classe, ao implementar (direta ou indiretamente) duas interfaces que possuam métodos default idênticos, irá gerar ambiguidade, logo, erro de compilação, como se vê no caso abaixo.
````Java
interface House {
	public default String getAddress() {
		return "House";
	}
}

interface Office {
	public default String getAddress() {
		return "Office";
	}
}

class HomeOffice implements House, Office { // 1 - compile error
	
}
````
Abaixo, exemplo deste mesmo caso através de herança indireta (ao extender `HomeOffice`, a classe `CoffeOffice` inderetamente também implementa `Office`, resultando no mesmo caso acima)
````Java
interface House {
	public default String getAddress() {
		return "House";
	}
}

interface Office {
	public default String getAddress() {
		return "Office";
	}
}

class HomeOffice implements Office {
	
}

class CoffeOffice extends HomeOffice implements House { // 1 - compile error
	
}
// 1 = Duplicate default methods named getAddress with the parameters () and () are inherited from the types Office and House
````
##### Solução
Para solucionar este conflito, a classe deve prover sua própria implementação do método `getAddress()`.
````Java
interface House {
	public default String getAddress() {
		return "House";
	}
}

interface Office {
	public default String getAddress() {
		return "Office";
	}
}

class HomeOffice implements House, Office {
    public String getAddress() {
		return "HomeOffice";
	}
}
````
Segundo exemplo:
````Java
interface House {
	public default String getAddress() {
		return "House";
	}
}

interface Office {
	public default String getAddress() {
		return "Office";
	}
}

class HomeOffice implements Office {
	
}

class CoffeOffice extends HomeOffice implements House { 
	public String getAddress() {
		return "Office";
	}
	
}
````
#### Entre métodos abstratos de duas interfaces distintas
Regra: o método sobrescrito deve satisfazer as duas interfaces, ao mesmo tempo, caso contrário, ocorrerá erro de compilação.  
O caso abaixo é um exemplo da regra acima.

````Java
interface House {
	public Number getNumber();
}

interface Office {
	public Integer getNumber();
}

class HomeOffice implements House, Office {
	@Override
	public Integer getNumber() {
		return 0;
	}
}
````
#### Entre constantes de duas interfaces distintas

### Interface x Classe abstrata
Escolha Interface para agrupar comportamentos em comum que podem ser customizáveis.  
Escolha Classe Abstrata quando os objetos concretos terão comportamentos em comum


## Herança
O Java suporta 3 tipos de herança:
- [1-1] Herança de **estado** (*inheritance of state*): atributos de instância.
- [1-1] Herança de **implementação** (*inheritance of implementation*): métodos de instância
- [1-N] Herança de **tipo** (*inheritance of type*): classe ou interface (*is a*).

Estado é representado por atributos de instância em classes. Considerando que uma classe pode herdar apenas uma classe, logo, **não há suporte para herança múltipla de estado**.  
Por outro lado, é possível implementar N interfaces em uma classe, portanto, **é possível a herança múltipla de tipo.**

Toda classe é subclasse de `java.lang.Object`.  
Para declarar uma herança explícita, usa-se a palavra `extends`.
````Java
public class B extends A {}
````
Além disso, consideramos que:
- Podemos dizer que `B` **é um** `A`. Mas não necessáriamente o contrário.  
- Apesar de não ser um membro, o construtor da superclasse é **sempre** invocado por suas subclasses.  
- Blocos de inicialização das superclasses não podem ser chamados por suas subclasses.  

### Regras na herança de membros
#### Regras gerais
- a subclasse não herda membros privados da superclasse*
- apenas membros public e protected são herdados, não importa o package onde esteja.
- caso estejam no mesmo package, membros package-default serão herdados também.  

\* mesmo assim, os membros private são instanciados.
#### Atributos herdados podem ser:
- usados diretamente da superclasse, como um atributo normal
- *hidding*: substituídos por um atributo de mesmo nome na subclasse (não recomendado)
- suplementados: a subclasse declara atributos que não existem na superclasse.  

#### Métodos Herdados podem ser:
- usados diretamente da superclasse, como um método normal
- sobrescritos (*overriden*): a subclasse declara um método com a mesma assinatura e retorno de um método da superclasse.
- *hidden*: a subclasse declara um método estático com a mesma assinatura de um método da superclasse.
- suplementados: a subclasse declara métodos que não existem na superclasse.  

### *Hiding*
Ocorre para:
- **atributo de instância:** criando um atributo com o mesmo nome e tipo da superclasse.
- **atributo estático:** criando um atributo estático com o mesmo nome e tipo da superclasse.
- **método estático:** criando um método estático com o meesmo nome e parâmetos da superclasse
- **método de instância:** não ocorre *hiding*, mas *overriding*

## Polymorphism Casting Object vs Referências

Não é possível alterar o tipo de referência de uma variável.
````Java
Object s = "oi";
String s = "oi";
````
Não é possível atribuir um valor que não é derivado do tipo da variável (em tempo de compilação).
````Java
Thread t = "oi"; 
````
**Polimorfismo ocorre em runtime** e serve para forçarmos um objeto a tomar uma forma diferente para produzir um resultado desejado.  
Para sinalizar esta "troca" ao compilador, usamos o **casting**, que pode ser usado em 
- atribuições
- expressões
- em parâmetros  

O casting pode ter duas direções:
- **Downcasting**: casting para um tipo **mais específico**
- **Upcasting**: casting para um tipo **mais genérico**

## Sobrescrita de métodos
Checklist para identificar uma sobrescrita válida, observar:
- nome (devem ser iguais)
- parâmetros (devem ser iguais)
- retorno (tipo ou subtipo)
- throws (nenhum, tipo ou subtipo)

### Casos específicos
*Generic type erasure*: **Em parâmetros**, o método da subclasse pode "apagar" o tipo genérico da superclasse. Mas não o contrário.
````Java
// superclass signature
transform(Set<Integer> list)
// valid overriding
transform(Set<Integer> list)
transform(Set list)
````