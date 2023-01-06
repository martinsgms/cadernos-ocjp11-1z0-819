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
| Valor  | Tipo| 
|-------|-------|
| 1 | `int` |
| 1.0 | `double` |
| 1.0l | `long` |
| 1.0d | `double` |
| 1.0f | `float` |
| '\u0000' | `char` |

> Valores `int` literais podem ser atribuídos a tipos menores sem casting.

#### Base numérica alternativa
Podem ser atribuídas a qualquer tipo primitivo numérico e `char`.
- `01`  &rarr; octal
- `0x1` &rarr; hexadecimal
- `0b1` &rarr; binário

### Referências
São ponteiros que apontam para um objeto na memória.

## <a id="narrowing"></a> Narrowing
Conversão para um tipo de menor capacidade.  Possui risco de overflow.  
````Java
// int para byte OK
byte value = 1;
````
Este procedimento deve ocorrer em tempo de compilação, caso contrário, ocorre erro.

````Java
int intValue = 1
byte value = intValue; // compile error
````
O compilador não executará o código para saber o valor que `intValue` tem. Logo o narrowing não ocorre automaticamente.

Para double não ocorre narrowing 
implicitamente para outros tipos. É sempre necessário casting explícito.
````Java
// float value = 1.0 NÃO COMPILA
float value = (float) 1.0 
````
### Overflow
Se o valor atribuído ultrapassar a capacidade do tipo, causará **overflow**.  
No caso byte possui um range de -128 a 127. Logo, um valor fora deste intervalo causará overflow (ou underflow) e precisará de casting explícito para compilar. Porém, mesmo compilando, o resultado será imprevisível.
````Java
byte value = (byte) 130;
// value terá um valor imprevisível devido ao overflow
````
**Em double e float nunca ocorre overflow**, porque eles utilizam notação científica para representar o valor (isso causa perda de precisão).

## <a id="widening"></a> Widening
Conversão para um tipo de maior capacidade. Basicamente atribuir um tipo menor a um maior.
````Java
// widening válido. int cabe em long.
long number = 1;
````

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
Escopo é a região onde um membro pode ser utilizado no programa, através de seu nome simples. É delimitado por `{}`.

### Shadowing
É permitido que variávis locais e variáveis membro tenham o mesmo nome. As variáveis locais terão precedência no uso direto, com nome simples.
````Java
class Pessoa {
    String nome; //1

    String getNome() {
        String nome = "teste"; //2
        return nome;
    }
}
// Será retornado o conteúdo de //2
````
Outro exemplo
````Java
class Pessoa {
    String nome; //1

    String getNome() {
        String nome = "teste"; //2
        return this.nome;
    }
}
// Será retornado o conteúdo de //1
````
### Problemas comuns em escopo
#### Redeclarar parâmetro
Parâmetros contam como variáveis definidas no escopo e não podem ser redeclaradas.
````Java
int soma(int n1, int n2) {
    int n1 = 0; // compile error
}
````

#### Redeclarar acumulador
A primeira seção de um for é reservado para declarar variávies. Portanto não é permitido que possuam o mesmo nome de variáveis existentes no escopo
````Java
int j = 0;
for (int i = 1, j = 2;;) {} // compile error
````

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

## <a id="resumo"></a> Resumo
- Variáveis locais precisam ser inicializadas antes de utilizadas.
- Membros estáticos e de instância são inicializados por default
- Literais inteiros são `int`
- Literais decimais são `float`
- Em `double` e `float` não ocorre overflow
- `null` não é um valor válido para tipos primitivos.