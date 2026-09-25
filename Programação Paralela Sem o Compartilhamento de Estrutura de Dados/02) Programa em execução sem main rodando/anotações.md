# Programação Paralela Sem o Compartilhamento de Dados Aula 02 -  Main e controle de Threads

### Main e controle de Threads

A main é a Thread principal do programa, ela pode criar e iniciar outras Threads e continuar sua própria execução enquanto as outras tarefas trabalham.

**Exemplo:**

A main pode criar e iniciar três tarefas:

TarefaDoTipo1 t1 = new TarefaDoTipo1();</br>
t1.start();

TarefaDoTipo2 t2 = new TarefaDoTipo2();</br>
t2.start();

TarefaDoTipo3 t3 = new TarefaDoTipo3();</br>
t3.start();
*Atenção:* Nesse caso a TarefaDoTipo3 é ```public class TarefaDoTipo3 implements Runnable``` assim ela já é inicializada como thread 

Podemos representar assim:
```
                    MAIN
                      |
          ┌───────────┼───────────┐
          ↓           ↓           ↓
      Tarefa 1    Tarefa 2    Tarefa 3
          |           |           |
        run()       run()       run()
```

As três tarefas podem executar concorrentemente iniciando e encerrando as tarefas pela Main.

### main da aula 01 e main da aula 02

**Aula 01:** inicia e depois encerra as tarefas:
```
t1.start();
t2.start();
t3.start();
```
```
t3.morra();
t2.morra();
t1.morra();
```
**Fluxo:** As threads iniciam e encerram, mas a main continua


**Aula 02**: Ela apenas inicializa as threads sem chamar o morra()

```
TarefaDoTipo1 t1 = new TarefaDoTipo1();
t1.start();

TarefaDoTipo2 t2 = new TarefaDoTipo2();
t2.start();

TarefaDoTipo3 t3 = new TarefaDoTipo3();
t3.start();
```

**Fluxo:** A main termina, mas as tarefas continuam rodando. 


