# [Java 11] Array
Autor: [Gabriel Martins dos Santos - *Java Software Engineer*](https://linkedin.com/in/martinsgms)

Caderno de estudos para certificação OCJP11 (1z0-819).

---

### Conteúdo

- [Introdução](#introducao)
- [Inicialização](#inicializacao)
- [Principais Exceptions](#exceptions)

---

## <a id="introducao"></a> Introdução
## <a id="declaracao"></a> Declaração
Para declarar o array, os colchetes `[]` podem aparecer em qualquer posição, exceto antes do tipo da referência.

````Java
int[] a;
int []b;
int c[];
int d[ ];
int[]e;
int [] f;
// []int g; compile error
````
Porém, ao utilizar `var`, os colchetes não são permitidos em nenhuma posição. Além disso, neste caso **o array deve obrigatóriamente ser inicializado com um tipo explícito**.

## <a id="inicializacao"></a> Inicialização
Existem basicamente 3 formas de inicializar um array, sendo elas:

**Forma 1**: `new` + tipo e capacidade  
O array é tradado como um objeto, portanto deve ser inicializado usando `new`, seguido do tipo e a capacidade.

**Forma 2**: `new` + tipo e elementos  
Logo após os colchetes, os elementos já são informados. Neste caso não é permitido informar a capacidade do array.

> Nota: para as duas formas acima, o colchetes `[]` possuem **posição fixa**, diferente do caso de declaração.

**Forma 3**: apenas elementos  
Logo após do operador de atribuição `=`, os elementos já são incluídos. A declaração e a inclusão deve ocorrer na "mesma linha" (instrução). Não é possível usar esta forma quando usa-se `var`.

````Java
int[] a = new int[3];
int []b = new int[]{1, 2, 3};
int c[] = {1, 2, 3};
// int z[] = new [3]int; compile error

var d = new int[3];
var e = new int[]{1, 2, 3};
// var f = {1, 2, 3}; compile error
````
Lembre-se que **não é possível definir a capacidade e incluir elementos ao mesmo tempo!**
````Java
// int []b = new int[3]{1, 2, 3}; compile error
````

### Capacidade negativa
Inicializar um array com capacidade negativa é permitido (compila). Porém, ao tentar acessar qualquer posição, ocorre exception em tempo de execução.
````Java
int[] a = new int[-1];
System.out.println(a[-1]);
````
*output*
````Java
Exception in thread "main" java.lang.NegativeArraySizeException: -1
````
### Capacidade zero
Semelhante ao caso acima, porém o output é uma outra exception.
````Java
int[] a = new int[0];
System.out.println(a[0]);
````
*output*
````Java
Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException: Index 0 out of bounds for length 0
````
### Arrays multidimensionais (array de array)
As três formas apresensatas acima também valem para arrays deste tipo.
````Java
int[][] a = new int[3][];
int []b[] = new int[][]{{1}, {2, 3}, new int[10]};
int c[][][] = {{new int [3]}, {}, {new int[7]}};

var d = new int[][]{{8}, {9, 10, 11}, new int[6]};
````
Um array pode ter várias dimensões. É obrigatório apenas que a capacidade da primeira dimensão seja informada. Todas as demais podem ter capacidades diferentes.
````Java
int[] []a [] = new int[2][][];
````
A quantidade de colchetes `[]` da declaração e da inicialização devem ser a mesma.  
Usar `var`, neste caso, é mais prático.
````Java
var a = new int[2][][];
````
Porém lembre-se que ao usar `var`, é necessário explicitar o tipo (*cf. Forma 3*).

### Inicialização das posições
Uma vez inicializado, todas as posições do array são preenchidas com o valor default do tipo de dado utilizado.
- tipos primitivos: valor default (`0`, `0.0`, `false` ...).
- referências: `null`;

### Casting entre arrays
Entre arrays de **tipos primitivos**, não é possível realizar casting.
````Java
int [] inteiros = new int[10];
long [] longs = (int[]) inteiros;
````
Conseguimos fazer esta operação apenas com referências. Na verdade, o que ocorre é o polimorfismo entre os tipos.
````Java
String [] strings = new String[10];
Object [] objects = strings;
````
O código acima compila e executa porque `String` **é um** `Object`(*cf. upcasting*).  
No exemplo abaixo, ocorre a situação inversa entre a hierarquia dos tipos. É possível forçar o casting, porém `java.lang.ClassCastException` será lançado em tempo de execução ao tentar chamar métodos de `String` (*cf. downcasting*).
````Java
Object [] objects = new Object[10];
String [] strings = (String[]) objects;

for (String s : strings) {
    s.concat("teste");
}
````
*output*
````Java
Exception in thread "main" java.lang.ClassCastException: class [Ljava.lang.Object; cannot be cast to class [Ljava.lang.String; [...]
````
## <a id="conversao"></a> ArrayList para Array
Para transformar `java.util.ArrayList` em um array, é possível utilizar o método `.toArray() : Object[] - ArrayList`.
````Java
List<String> nomes = new ArrayList<>();
Object[] array = nomes.toArray();
````
Observe que o retorno deste método é `Object[]`. Caso seja preciso um array de outro tipo, por exemplo, um `String[]`, existem as possibilidades:
````Java
String[] array = nomes.toArray(new String[nomes.size()]);
````
ou então
````Java
Object[] array = nomes.toArray(new String[0]);
````
No caso acima, internamente o método irá verificar que 0 pode ser uma capacidade insuficiente para transpor os elementos de `ArrayList` para o array e fará o redimensionamento necessário.

## <a id="arrays"></a> A classe utilitária `java.util.Arrays`

### Arrays.sort()
Ordena o array. Caso o array seja de referências, é necessário que `equals()` seja corretamente implementado, ou então seja fornecido uma implementação de `Comparator<T>` como segundo argumento.
````Java
int[] numeros = {4, 1, 7, 2, 5, 3, 8, 0};

Arrays.sort(numeros);

for (int n : numeros) {
    System.out.print(n);
}

// output: 01234578
````
> Perceba que, para array, diferente de `Collections`, não é possível utilizar o `toString()` default para exibir todos os elementos de uma vez só. Caso isso fosse feito, a saída seria o **endereço de memória** do array, já que ele é um objeto.
````Java
System.out.print(numeros);

// output: [I@1d251891
````

### Arrays.equals(T arr1, T arr2)
Compara se os elementos dos arrays são iguais, exatamente na mesma posição. Se um dos dois for `null`, o resultado é `false`.
````Java
int[] numeros = {1, 2, 3};
int[] numeros2 = {1, 2, 3};

System.out.println(Arrays.equals(numeros, numeros2));

// output: true
````
### Arrays.mismatch(T arr1, T arr2)
Verifica em qual index os arrays diferem.
````Java
int[] numeros = {1, 2, 3, 6, 4};
int[] numeros2 = {1, 2, 3, 5, 4};

System.out.println(Arrays.mismatch(numeros, numeros2));

// output: 3
````
> `Arrays.equals(T arr1, T arr2)` possui uma chamada interna para `Arrays.mismatch(T arr1, T arr2) < 0` para verificar se os arrays são iguais.

### Arrays.compare(T arr1, T arr2)
### Arrays.copyOf
### Arrays.asList
### Arrays.binarySearch
### Arrays.toString
### Arrays.stream





## <a id="exceptions"></a> Principais Exceptions
Confira as principais exceptions ao trabalhar com array.

`java.lang.NullPointerException`  
`java.lang.NegativeArraySizeException`
`java.lang.ArrayIndexOutOfBoundsException`