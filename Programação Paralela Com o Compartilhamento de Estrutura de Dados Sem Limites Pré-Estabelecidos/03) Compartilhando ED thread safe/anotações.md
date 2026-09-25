# Programação Paralela Com o Compartilhamento de Dados Sem Limites Pré-Estabelecidos Aula 03 - Iniciando e Encerrando Threads

### O Vector< >

```private Vector<Character> armazenamento``` : vetor que armazena caracteres

```public TarefaDoTipo1 (Vector<Character> armz) throws Exception```: construtor da classe que rebece o Vector<Character> (vetor que espera um caracter) chamado ```armz```

```this.armazento = armz```: vetor recebido pelo construtor e armazenado dentro do objeto, agora o ```run()``` consegue acessá-lo

Esse objeto pode ser usado em outras partes do programa, ou seja, uma thread pode acessar esse vetor.

### implements Runnable e o join
```java
public void join() throws InterruptedException
{
    this.tarefa.join();
}
```
Serve para **sincronizar** ela faz a chamou (por exemplo a thread TarefaDoTipo3) esperar que outra thread conclua 100% do trabalho. </br>
*Lógica:* "Eu preciso do resultado ou preciso garantir que você terminou antes de eu continuar. Vou sentar e esperar você terminar."

import java.util.Vector;

public class Programa
{
    public static void main (String[] args)
    {
        try
        {
            Vector<Character> armazenamento;
            armazenamento = new Vector<Character> ();

            System.out.println ("Tecle ENTER para ativar as tarefas e");
            System.out.println ("Tecle novamente ENTER para terminar o programa.");
            Teclado.getUmString();
            
            TarefaDoTipo1 t1 = new TarefaDoTipo1 (armazenamento);
            t1.start ();
            
            TarefaDoTipo2 t2 = new TarefaDoTipo2 (armazenamento);
            t2.start ();
            
            TarefaDoTipo3 t3 = new TarefaDoTipo3 (armazenamento);
            t3.start ();
            
            TarefaDoTipo4 t4 = new TarefaDoTipo4 (armazenamento);
            t4.start ();
            
            Teclado.getUmString();
                    
            t4.morra ();
            t3.morra ();
            t2.morra ();
            t1.morra ();
            
            t1.join();
            t2.join();
            t3.join();
            t4.join();
            
            System.out.println ("Execucao do programa finalizada.");
        }
        catch (Exception erro)
        {} // sei que não passei null para o construtor de nenhuma das tarefas
    }
}

### [Adicionar informações da main]