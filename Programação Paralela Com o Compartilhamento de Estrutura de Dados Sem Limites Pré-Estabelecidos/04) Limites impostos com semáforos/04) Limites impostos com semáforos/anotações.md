# Programação Paralela Com o Compartilhamento de Dados Com Limites Pré-Estabelecidos Aula 04 - Por Semáforos e Thread Safe

### Semaphore

Importado com: ```import java.util.concurrent.Semaphore;``` 
**Função:** Usado para controlar o acesso a um recurso compartilhado, no caso do explo, o  ```private Vector<Character> armazenamento```

```java
    private Vector<Character> armazenamento;
    private Semaphore livre; // quantos espaços do vetor estão livres?
    private Semaphore ocupado; // quantos elementos podem ser retirados?
    
    // verifica se os valores recebidos não foram null, por exemplo
    public TarefaDoTipo1 (Vector<Character> armz, Semaphore lvr, Semaphore ocp) throws Exception
    {
        if (armz==null)
            throw new Exception ("Armazenamento ausente");
            
        if (lvr==null)
            throw new Exception ("Livre ausente");
            
        if (ocp==null)
            throw new Exception ("Ocupado ausente");
            
        this.armazenamento = armz; // O armazenamento deste objeto será o Vector que recebi no parâmetro arm
        this.livre         = lvr;
        this.ocupado       = ocp;
    }
```

**Como os espaço são consumidos?**
```java
    this.livre.acquireUninterruptibly(); 
    this.armazenamento.add (caractere);
    this.ocupado.release();
```
Em ```this.livre.acquireUninterruptibly()``` o caracter diz que vai consumir um espaço livre, se esse espaço não existir a thread fica **bloqueada** (significa que ela para de executar naquele ponto e fica esperando alguma condição acontecer, nesse caso, liberação de espaço).

Quando existe um espaço livre, a thread executa ```this.armazenamento.add (caractere)``` que é onde ela realmente vai adicionar o caracter ao armazenamento

**O que o ```this.ocupado.release()``` faz?**
Ele serve para "avisar" para threads que podem querer consumir esse caracter adicionado em armazenamento que ele está disponível, basicamente dizendo que agora existe X elementos a mais para serem retirados.

**Fluxo:**
```
             PRODUTOR
                │
                │
                ▼
       livre.acquire()
       "pega um espaço"
                │
                ▼
       armazenamento.add(A)
       "coloca A"
                │
                ▼
       ocupado.release()
       "agora tem A para retirar"
                │
                ▼
             CONSUMIDOR
                │
                ▼
       ocupado.acquire()
       "pega A"
                │
                ▼
       armazenamento.remove()
       "retira A"
                │
                ▼
       livre.release()
       "agora tem um espaço livre"
```

### Como funciona a Main?

```Vector<Character> armazenamento = new Vector<Character>();``` cria o vetor deverá ser compartilhado entre todas as threads

```java
    Semaphore livre = new Semaphore (1024,true); // Inicialmente, temos 1024 posições livres no vetor
    Semaphore ocupado = new Semaphore (0,true); // Ininicalmente vazio, porque não temos nenhum caracter para ser retirado do vetor
```

**Fluxo:**
```
               PROGRAMA
                   │
                   ▼
       cria Vector vazio
                   │
                   ▼
       cria livre = 1024
                   │
                   ▼
        cria ocupado = 0
                   │
                   ▼
             espera ENTER
                   │
                   ▼
       ┌──────────────────────┐
       │ inicia T1             │
       │ inicia T2             │
       │ inicia T3             │
       │ inicia T4             │
       └──────────────────────┘
                   │
                   ▼
          as threads trabalham
                   │
                   ▼
             espera ENTER
                   │
                   ▼
          chama morra() em T1-T4
                   │
                   ▼
        threads terminam aos poucos
                   │
                   ▼
     "Execucao finalizada."
```