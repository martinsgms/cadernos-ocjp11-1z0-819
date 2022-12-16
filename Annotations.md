# [Java 11] Annotation
Autor: [Gabriel Martins dos Santos - *Java Software Engineer*](https://linkedin.com/in/martinsgms)

Caderno de estudos para certificação OCJP11 (1z0-819).

---
## Introdução

Todas as *annotation interfaces* extendem implicitamente `java.lang.annotation.Annotation`. Elas não possuem lógica ou processamento algum. São exclusivamente para fornecerem *metadados* (dados sobre um dado).

### Por que são importantes?
- Fornecem *metadados* para compilação e execução
- Podem ser usadas em conjunto com a *Reflection API*.
- São o destaque de *frameworks* como JUnit e Spring e especificações como JPA e Servlet, por exemplo.
- Aproveitam o compilador para validar a configuração
- Uma excelente alternativa ao XML
- Padronizam configurações e melhoram sua legibilidade
- Podem "gerar código", como no caso do Lombok.

### Annotations mais conhecidas
`````Java
@Override
@Deprecated
@SuppressWarnings
// @Autowired ~Spring
// @Entity ~JPA
// @Test ~JUnit
// @WebServlet ~Servlet
// @Getter ~Lombok
`````

## Sintaxe básica
A sintaxe de uma annotation é semelhante à declaração de uma interface. No corpo da annotation, são declarados os parâmetros, que podem ser apenas String ou tipos primitivos, seguido (ou não) de seu valor padrão.  
TODOS os parâmetros que não possuem valor default precisam ser preenchidos.
````Java
public @interface [NOME-ANNOTATION] {
    [TIPO (STRING/PRIMITIVOS)] [NOME-PARÂMETRO]() [default] [VALOR];
}
````
*Exemplo*
````Java
public @interface MinhaAnnotation {
    int value() default 0;
}
````
*Exemplo de uso*
````Java
@MinhaAnnotation(value = 1) // 1
@MinhaAnnotation(1) // 2
@MinhaAnnotation //3
````
Onde:  
`// 1` &rarr; É a forma básica de utilizar uma *annotation*. Neste caso os parâmetros são preenchidos nominalmente, e não importa a ordem.

`// 2` &rarr; Se a interface possui o parâmetro chamado `value`, podemos passar o seu valor sem referenciar seu nome. Se existirem outros parâmetros, este exemplo só será válido caso todos os outros parâmetros tenham valor *default*.

`// 3` &rarr; O parâmetro `value` possui o valor *default* 0, então não somos obrigados a preencher o seu valor.

## Target
*A fazer...*
## Retention Policy
*A fazer...*
## Repeatable
*A fazer...*
## Herança
*A fazer...*
## Annotations importantes
`@SuppressWarnings`  
`@SafeVarargs`  
`@Override`  

## Referências
https://docs.oracle.com/javase/tutorial/java/annotations/index.html
