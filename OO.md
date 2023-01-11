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


````Java
public interface Imposto {
    int getAliquota();
    int calculaImposto();
}
````

> Interfaces são ótimas para implementar polimorfismo. A intenção que definem pode tomar diferentes formas baseado no objeto que foi criado.

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

## Herança
O Java suporta 3 tipos de herança:
- [1-1] Herança de **estado** (*inheritance of state*): atributos
- [1-1] Herança de **implementação** (*inheritance of implementation*): métodos
- [1-N] Herança de **tipo** (*inheritance of type*): classe ou interface (*is a*).

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