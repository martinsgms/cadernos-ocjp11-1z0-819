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

**private** &rarr; somente na própria classe.

**package *(default)*** &rarr; na própria classe + pacote;

**protected** &rarr; na própria classe + pacote + filhas

**public**  &rarr; em todo o módulo.

### Ordem de Restritividade
private **é+q** package **é+q** protected **é+q** public

> Decoreba p/ decorar a ordem: (pri) pa-pro-pu

## Modificadores `abstract` e `final`
**Atenção!** Estes dois modificadores se contradizem, eles **NUNCA** podem ser usados juntos.

### Em classes
**final** &rarr; indica que a classe não poderá ser extendida (herança).  
**abstract** &rarr; indica que a classe não poderá ser instânciada.

### Em atributos
**final** &rarr; indica uma constante. A inicialização é obrigatória no momento da inicializaçãoe e seu valor não poderá ser alterado. 
**abstract** &rarr; N/A.

### Em métodos
**final** &rarr; indica que o método NÃO PODE ser sobrescrito.  
**abstract** &rarr; indica que o método PRECISA ser sobrescrito.

## Outros modificadores
**strictfp**  
**volatile**  
**transient**  
**native**  

## Interface

Representa uma *intenção*, que pode tomar diferentes formas (polimorfismo). Exemplo: A intenção é calcular um imposto, e eu posso calcular diferentes impostos, cada um a sua maneira.  
É o mais alto nível de abstração e de baixo acoplamento possível.

````Java
public interface Imposto {
    int getAliquota();
    int calculaImposto();
}
````

> Interfaces são ótimas para implementar polimorfismo. A intenção que definem pode tomar diferentes formas baseado no objeto que foi criado.

### Particularidades
1- Os **métodos** são `public abstract` por padrão (caracteristica que faz com que as classes *concretas* sejam obrigadas a implementar seus métodos).
````Java
interface Imposto {
	void getAliquota();
	Double calculaImposto();
}
````

- Também são permitidos métodos `default`, `private`, `static` e `strictfp`
- As **constantes** são `public static final` por padrão.