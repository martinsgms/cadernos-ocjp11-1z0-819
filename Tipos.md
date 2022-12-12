# [Java 11] Tipos de dados e operações

Autor: [Gabriel Martins dos Santos - *Java Software Engineer*](https://linkedin.com/in/martinsgms)

Caderno de estudos para certificação OCJP11 (1z0-819).

---

### Conteúdo

- [Introdução](#introducao)
- [Casting](#casting)
- [Escopo](#escopo)
- [Operadores](#operadores)
- [Type promotion](#promotion)
- [Wrappers Primitivos](#wrappersp)
- [Autoboxing/Unboxing](#boxing)
- [Naming Rules](#naming)
- [Inferência de Tipo (`var`)](#var)
- [Varags](#varargs)

---

## <a id="introducao"></a> Introdução

Java é uma linguagem *fortemente tipada*. Isso significa que cada variavel precisa ter um tipo (implícito ou explícito) e este tipo não muda ao longo do programa.  
Os tipos são divididos em dois grandes grupos:

- Tipos primitivos
- Referências

### Tipos Primivitos
São aqueles que possuem diretamente um valor associado. **Tipos primitivos nunca podem ser null**.
  
#### Inteiros

| Tipo  | Min | Max | Tamanho
|-------|-------|-------|-------|
| `byte` | -128 | 127 | 8 bits
| `short` | ≈-32k | ≈32k | 16 bits
| `int` | ≈-2.1bi |  ≈2.1bi | 32 bits
| `long` | ≈-9.2q* | ≈9.2q* | 64 bits

> \* q = quintilhão (10<sup>18</sup>)

#### Caractere

| Tipo  | Min | Max | Tamanho
|-------|-------|-------|-------|
| `char` | 0 | 65.535 | 16 bits

#### Ponto flutuante

| Tipo  | Min | Max | Tamanho
|-------|-------|-------|-------|
| `float` | 1.4x10<sup>-45</sup> | 3.4028235x10<sup>38</sup> | 32 bits
| `double` | 4.9x10<sup>-324</sup> | 1.7976931348623157x10<sup>308</sup> | 64 bits

#### Lógico 

| Tipo  | Valores | Tamanho
|-------|-------|-------|
| `boolean` | `false` ou `true` | 1 bit

#### Valores Literais
Exemplos:
- `1` &rarr; é um `int`.
> Valores `int` literais podem ser atribuídos a tipos menores sem casting.
- `1.0` &rarr; é um `double`.
- `1.0d` &rarr; é um `double`.
- `1.0f` &rarr; é um `float`.

#### Base numérica alternativa
Podem ser atribuídas a qualquer tipo primitivo numérico e `char`.
- `01`  &rarr; octal
- `0x1` &rarr; hexadecimal
- `0b1` &rarr; binário

### Referências
São ponteiros que apontam para um objeto na memória.

## <a id="casting"></a> Casting

### Tipos primitivos

#### Overflow
**Regra**: tipos menores  cabem em tipos maiores e não pedem casting explícito. Mesmo assim, pode ocorrer **overflow**, isto é, quando o tipo maior excede o valor máximo do tipo menor, resultando `-1`.

````Java
    byte b = (byte) Integer.MAX_VALUE;
    System.out.println(b); // -1
````

#### Downcasting
Casting do tipo **maior para menor**.  
É necessário **casting explícito** e observar casos de **overflow**.

````Java
    int i = 10;
    
    byte b = (byte) i;
    short s = (short) i;
    char c = (char) i;
````
> Exceção: de `float` para `double` não precisa de cast.

> Nota: `char` **sempre irá precisar de cast explícito** por não ser "puramente" um tipo inteiro numérico.

#### Upcasting
Casting do tipo **menor para maior**.  
Ocorre casting implícito, por conta da regra que um tipo menor cabe em um tipo maior.
````Java
    int i = 10;

    float f = i;
    double d = i;
````

### Referências
#### Downcasting
É necessário casting explícito;
````Java
    String s = (String) new Object();
    System.out.println(s.concat("oi"));
````
Compila mas resulta em Exception em runtime. ` java.lang.ClassCastException: class java.lang.Object cannot be cast to class java.lang.String (java.lang.Object and java.lang.String`.

#### Upcasting
Vale a regra de que um tipo menor "cabe" em um tipo maior sem casting explícito.
````Java
    Object o = new String("oi");
````
Compila mas a referência `o` não possuirá os métodos de `String`, somente de `Object`.  
Ao tentar chamar um método de `String`, o código não compila.
````Java
    Object o = new String("oi");
    // System.out.println(o.concat("oi")); NÃO COMPILA!
````
> Quem determina os métodos é a **referência**.  
Quem determina como os métodos são implementados é o **objeto**.

## <a id="escopo"></a> Escopo

## <a id="operadores"></a> Operadores
### Precedência de operações
> **Pegadinha de prova!** `if (x != y = !x)` causa erro de compilação. Porque `x != y` resulta em um valor primitivo (`boolean`), que não pode receber a atribuição do valor de `!x`. Já neste caso `if (x = y != x)` temos uma operação válida.

## <a id="promotion"></a> Type promotion
## <a id="wrappersp"></a> Wrappers Primitivos
## <a id="boxing"></a> Autoboxing/Unboxing
## <a id="naming"></a> Naming Rules
### Boas práticas

## <a id="var"></a> Inferência de Tipo (`var`)
> **Pegadinha de prova!** Ao declarar um array, a sintaxe `[]` não é permitida do lado esquerdo da atribuição (`=`). E no lado direito, o tipo do array deve ser declarado com `new int[] {1, 3}`, `new int[2]` ou `new int[2][]`, por exemplo. A atribuição literal usando diretamente `{}` causara erro de compilação.

## <a id="varargs"></a> Varags