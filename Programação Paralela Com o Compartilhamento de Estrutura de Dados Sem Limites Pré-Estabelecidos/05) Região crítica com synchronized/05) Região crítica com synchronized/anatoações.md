# Programação Paralela Com o Compartilhamento de Dados Com Limites Pré-Estabelecidos Aula 05 - Por Semáforos Não Thread Safe

### ArrayList

```private ArrayList<Character> armazenamento```: Diferente do *Vector* o ArrayList não possui uma sinconização interna em seus metódos, por isso é necessário fazer isso "manualmente".

```java
synchronized (this.armazenamento)
{
    this.armazenamento.add(caractere);
}
```
Essa função faz a sinconização dizendo ao programa: *"enquanto eu estiver mexendo no ArrayList, nenhuma outra thread pode entrar nesse bloco sincronizado usando o mesmo objeto como monitor."*