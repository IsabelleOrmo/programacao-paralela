# Programação Paralela Com o Compartilhamento de Dados Com Limites Pré-Estabelecidos Aula 06 - Por Semáforos Não Thread Safe com Exclusão Mútua

## O que é o mutex?
```java 
private Semaphore mutex;
```
Ele serve para fazer exclusão mútua, ou seja, garante que somente uma thread por vez mexa no ArrayList substituindo o ```synchronized (this.armazenamento)```

```java
this.mutex.acquireUninterruptibly();
this.armazenamento.add(caractere);
this.mutex.release();
```

- acquire() → a thread pega o mutex e entra na região protegida. (1 → 0)
- add() → modifica o ArrayList.
- release() → libera o mutex para outra thread poder usar.

Se outra thread tentar pegar o mutex enquanto ele estiver sendo usado, ela precisa esperar.

**Resumo:** mutex evita que várias threads modifiquem o ArrayList ao mesmo tempo.

