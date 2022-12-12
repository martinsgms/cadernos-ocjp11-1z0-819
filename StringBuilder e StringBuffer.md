# [Java 11] StringBuilder e StringBuffer

Autor: [Gabriel Martins dos Santos - *Java Software Engineer*](https://linkedin.com/in/martinsgms)

Caderno de estudos para certificação OCJP11 (1z0-819).

---

### Conteúdo

- [Definição](#definicao)
- [Construtores](#construtores)
- [Igualdade](#igualdade)
- [Principais métodos](#metodos)
- [Pegadinhas da prova](#pegadinhas)
---

## <a id="definicao"></a> Definição
`java.lang.StringBuilder` e `java.lang.StringBuffer` são classes **mutáveis** para trabalhar com `String` de maneira mais **performática**.  
A diferença entre as duas é que `StringBuffer` possui acesso sincronizado a seus métodos (thread-safe).  
No mais, ambas possuem a mesma finalidade.

> Por ser **mutável**, métodos chamados no objeto refletem as operações no mesmo objeto, em vez de retornar um novo, como ocorre em `String`.

````Java
public final class StringBuilder
    extends AbstractStringBuilder
    implements java.io.Serializable, Comparable<StringBuilder>, CharSequence
{
    // ...
}
````
## <a id="construtores"></a> Construtores
````Java
    new StringBuilder()
                     (CharSequence seq)
                     (int capacity)
                     (String str)
````

## <a id="igualdade"></a> Igualdade
o `equals(Object):boolean` neste caso faz a comparação de **referência**.
```` Java
    StringBuilder sb = new StringBuilder();
    StringBuilder sb2 = new StringBuilder();
    
    sb.append("gabriel");
    sb2.append("gabriel");
    
    System.out.println(sb.equals(sb2)); // false
````
Para compara o *conteúdo*, prefira usar `compareTo(StringBuilder):int`.

## <a id="metodos"></a> Principais métodos

| Nome  | O que faz
|-------|-------|
| `toString()` : String | retorna um `new` String.

### Capacidade

| Nome  | O que faz
|-------|-------|
| `capacity()` : int | retorna a capacidade.
| `ensureCapacity(int min)` : void | garante uma capacidade mínima. Caso a capacidade seja ultrapassada, o próprio objeto ajusta a capacidade dinâmicamente.

### Tamanho

| Nome  | O que faz
|-------|-------|
| <sup>1</sup>`setLenght()` : void | redimensiona o tamanho. Se o tamanho for > lenght, os espaços restantes são preenchidos com `Character.MIN_VALUE`.
```` Java
    sb.append("gabriel");

    sb.setLength(40);
    System.out.println(sb); // sem aspas "gabriel              "
    
    sb.setLength(4);
    System.out.println(sb); // sem aspas "gabr"
````
### Manipulação de conteúdo

| Nome  | O que faz
|-------|-------|
|`append()` : void | 
|`insert()` : void | 
|`reverse()` : void | 
|`delete()` : void | 
```` Java
   // ...
````
---
> <sup>1</sup> &rarr; Lança `java.lang.StringIndexOutOfBoundsException.StringIndexOutOfBoundsException` caso o index for < 0.

### <a id="pegadinhas"></a> Pegadinhas da prova