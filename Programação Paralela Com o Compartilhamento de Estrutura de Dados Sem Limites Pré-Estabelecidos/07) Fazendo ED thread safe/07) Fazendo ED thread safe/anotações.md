NÃO REVISADO!!!!!

# Vector, Generics, Cloneable e Reflection

## 1. `Vector<X>`

A classe `Vector<X>` é uma implementação própria de uma estrutura de dados que armazena elementos de qualquer tipo `X`.

Exemplos:

```java
Vector<String>
Vector<Integer>
Vector<Pessoa>
```

O `X` é um **tipo genérico**, permitindo reutilizar a mesma classe para diferentes tipos de dados.

---

## 2. Armazenamento interno

```java
private X[] elem;
private int qtd;
```

* `elem` → array que armazena os elementos.
* `qtd` → quantidade de elementos atualmente armazenados.

O Vector começa com:

```java
TAMANHO_INICIAL = 10;
qtd = 0;
```

Como o Java não permite criar diretamente:

```java
new X[10]
```

é usado:

```java
(X[]) new Object[10];
```

---

## 3. `synchronized`

Os principais métodos possuem `synchronized`:

```java
public synchronized void add(X x)
public synchronized X get(int posicao)
public synchronized void remove(int posicao)
```

Isso garante **exclusão mútua**.

Ou seja:

> Quando uma thread está executando um método `synchronized` daquele objeto, outra thread precisa esperar para executar outro método `synchronized` no mesmo objeto.

Isso torna o acesso ao Vector mais seguro quando várias threads compartilham a mesma estrutura.

---

## 4. `add()`

```java
public synchronized void add(X x)
```

Adiciona um elemento ao Vector.

Primeiro verifica se o array está cheio:

```java
if (this.qtd == this.elem.length)
    this.redimensioneSe(2.0F);
```

Se estiver cheio, aumenta seu tamanho.

Depois adiciona o elemento e aumenta `qtd`:

```java
this.qtd++;
```

### Clonagem no `add()`

```java
if (x instanceof Cloneable)
    this.elem[this.qtd] = new Clonador<X>().clone(x);
else
    this.elem[this.qtd] = x;
```

Se o objeto implementar `Cloneable`, é armazenada uma **cópia** dele.

Caso contrário, o próprio objeto é armazenado.

---

## 5. `get()`

```java
public synchronized X get(int posicao)
```

Retorna o elemento de determinada posição.

A posição precisa ser válida:

```text
0 até qtd - 1
```

Se o objeto armazenado for `Cloneable`, o método retorna uma cópia dele.

---

## 6. `remove()`

```java
public synchronized void remove(int posicao)
```

Remove um elemento.

Depois da remoção, os elementos seguintes são deslocados uma posição para trás.

Exemplo:

```text
[C, C++, Java]
```

`remove(1)`:

```text
[C, Java]
```

Se o Vector ficar muito vazio, seu tamanho pode ser reduzido para economizar espaço.

---

## 7. `size()`

```java
public synchronized int size()
```

Retorna a quantidade de elementos armazenados:

```java
return this.qtd;
```

---

## 8. `toString()`

```java
public synchronized String toString()
```

Permite imprimir o Vector de maneira organizada.

Exemplo:

```java
System.out.println(v);
```

Resultado:

```text
[C, C++, Java]
```

---

## 9. `equals()`

```java
public synchronized boolean equals(Object obj)
```

Compara dois Vectors.

Verifica se possuem:

* o mesmo tipo;
* a mesma quantidade de elementos;
* elementos iguais nas mesmas posições.

---

## 10. `hashCode()`

```java
public synchronized int hashCode()
```

Gera um código numérico baseado no conteúdo do Vector.

É utilizado junto com `equals()` em estruturas que dependem de comparação e hashing.

---

# 11. Construtor de cópia

```java
public Vector(Vector<X> modelo)
```

Cria um novo Vector baseado em outro Vector.

Exemplo:

```java
Vector<String> v2 = new Vector<String>(v1);
```

Assim, `v2` recebe uma cópia dos elementos de `v1`.

---

# 12. `Cloneable`

```java
public class Vector<X> implements Cloneable
```

`Cloneable` indica que a classe permite que seus objetos sejam clonados.

O método:

```java
public synchronized Object clone()
```

cria um novo Vector usando o construtor de cópia:

```java
ret = new Vector<X>(this);
```

---

# 13. Classe `Clonador<X>`

A classe:

```java
public class Clonador<X>
```

serve para clonar objetos de forma genérica.

Ela utiliza **Reflection** para descobrir a classe do objeto:

```java
Class<?> classe = x.getClass();
```

Depois procura o método `clone()`:

```java
metodo = classe.getMethod("clone", null);
```

E executa esse método:

```java
ret = (X) metodo.invoke(x, null);
```

### Em resumo:

O `Clonador` permite executar o `clone()` de um objeto sem precisar conhecer diretamente sua classe.

---

# 14. Reflection

**Reflection** permite que o programa descubra informações sobre classes e métodos durante a execução.

Neste código, ela é usada para:

1. descobrir a classe do objeto;
2. procurar o método `clone()`;
3. executar esse método.

Principais métodos usados:

```java
x.getClass()
```

→ obtém a classe do objeto.

```java
classe.getMethod(...)
```

→ procura um método.

```java
metodo.invoke(...)
```

→ executa o método encontrado.

---

# 15. Redimensionamento

O Vector pode aumentar ou diminuir seu array interno.

### Quando fica cheio:

```text
10 → 20 → 40 → 80 → ...
```

Usa:

```java
redimensioneSe(2.0F);
```

### Quando fica muito vazio:

```text
40 → 20
```

Usa:

```java
redimensioneSe(0.5F);
```

Isso evita desperdício de memória.

---

# 16. Programa principal

O programa testa dois Vectors.

### Vector oficial do Java:

```java
java.util.Vector<String> v1;
```

### Vector criado pela própria aula:

```java
Vector<String> v2;
```

Ambos recebem:

```text
[C, C++, Java]
```

Depois:

```java
remove(1);
```

remove `C++`:

```text
[C, Java]
```

---

# Resumo geral

| Conceito           | Função                                                    |
| ------------------ | --------------------------------------------------------- |
| `X`                | Tipo genérico                                             |
| `Array`            | Armazena os elementos                                     |
| `qtd`              | Quantidade de elementos                                   |
| `add()`            | Adiciona elemento                                         |
| `get()`            | Obtém elemento                                            |
| `remove()`         | Remove elemento                                           |
| `size()`           | Retorna quantidade                                        |
| `synchronized`     | Garante exclusão mútua                                    |
| `Cloneable`        | Permite clonagem                                          |
| `clone()`          | Cria uma cópia                                            |
| `Clonador`         | Faz clonagem usando Reflection                            |
| `Reflection`       | Permite descobrir e executar métodos em tempo de execução |
| `redimensioneSe()` | Aumenta ou diminui o array                                |
