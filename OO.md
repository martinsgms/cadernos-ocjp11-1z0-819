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
Um **membro** é tudo aquilo que você pode acessar com `.` (ponto - operador de acesso). Isto é: 
- métodos
- atributos (variaveis e constantes)
- *inner classes*

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
#### 5. Constantes
As **constantes** são `public`, `static` e `final` por padrão. E podem ser acessadas via *herança*, ou de forma *direta*, i.e. `Interface.CONSTANTE`.
````Java
int ZERO = 0;
````
é o mesmo que 
````Java
public static final int ZERO = 0;
````