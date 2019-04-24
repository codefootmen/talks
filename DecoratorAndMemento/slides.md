---
title: Decorator & Memento
theme: white
css: style.css

revealOptions:
    transition: 'fade'
    controls: false
    progress: false
---

## Engenharia de Software
Filipe Jessé - Flávio Barci - Otávio Pereira

---

### Decorator Pattern

---

### Objetivo

Adicionar mais responsabilidades a um objeto dinâmicamente. Decorators dão uma alternativa flexivel à criação de subclasses para o aumento de funcionalidade.

---

O Decorator Pattern te permite mudar o comportamento de um objeto em tempo de execução, usando-o em um objeto de classe decorator.

---

### Aplicabilidade

 O padrão Decorator deve ser utilizado para acrescentar responsabilidades a objetos individuais de forma dinâmica e transparente, ou seja, sem afetar outros objetos.
 Para responsabilidades que podem ser removidas.

---

Quando a extensão através do uso de subclasses não é prática. Ás vezes, um grande número de extensão independentes é possível e isso poderia produzir uma explosão de subclasses para suportar cada combinação. Ou a definição de uma classe pode estar oculta ou não estar disponível para a utilização de subclasses.

---

# Exemplo

---

```
public interface Troll {
  void attack();
  int getAttackPower();
  void fleeBattle();
}
```

---

```
public class SimpleTroll implements Troll {

  private static final Logger LOGGER = LoggerFactory.getLogger(SimpleTroll.class);

  @Override
  public void attack() {
    LOGGER.info("The troll tries to grab you!");
  }
  @Override
  public int getAttackPower() {
    return 10;
  }
  @Override
  public void fleeBattle() {
    LOGGER.info("The troll shrieks in horror and runs away!");
  }
}
```

---

```
public class ClubbedTroll implements Troll {

  private static final Logger LOGGER = LoggerFactory.getLogger(ClubbedTroll.class);

  private Troll decorated;

  public ClubbedTroll(Troll decorated) {
    this.decorated = decorated;
  }
  @Override
  public void attack() {
    decorated.attack();
    LOGGER.info("The troll swings at you with a club!");
  }
  @Override
  public int getAttackPower() {
    return decorated.getAttackPower() + 10;
  }
  @Override
  public void fleeBattle() {
    decorated.fleeBattle();
  }
}
```

---

```
// simple troll
Troll troll = new SimpleTroll();
troll.attack();
// The troll tries to grab you!
troll.fleeBattle();
// The troll shrieks in horror and runs away!

// change the behavior of the simple troll by adding a decorator
Troll clubbedTroll = new ClubbedTroll(troll);
clubbedTroll.attack();
// The troll tries to grab you! The troll swings at you with a club!
clubbedTroll.fleeBattle();
// The troll shrieks in horror and runs away!
```
---
### Memento Pattern

---

### Objetivo

É um padrão de projeto que permite armazenar o estado interno de um
objeto em um determinado momento, para que seja possível retorná-lo
a este estado, caso necessário.

---

Podemos pensar em várias formas de se guardar um “print” de um objeto para podermos refazer uma operações feitas sobre ele. O padrão Memento traz uma forma de se implementar esta funcionalidade preservando o princípio do encapsulamento.

---

### Aplicabilidade

Memento é uma alternativa para sistemas onde os usuários fazem muitas
simulações e são necessários salvar ou desfazer essas
configurações em tempo de execução.

---

# Exemplo

---

```
class Memento {
    private String state;

    public Memento(String state) {
        this.state = state;
    }

    public String getState() {
        return state;
    }
}

```

---

```
class Originator {
    private String state;
   /* lots of memory consumptive private data that is not necessary to define the
    * state and should thus not be saved. Hence the small memento object. */

    public void setState(String state) {
        System.out.println("Originator: Setting state to " + state);
        this.state = state;
    }

    public Memento save() {
        System.out.println("Originator: Saving to Memento.");
        return new Memento(state);
    }
    public void restore(Memento m) {
        state = m.getState();
        System.out.println("Originator: State after restoring from Memento: " + state);
    }
}

```

---

```
class Caretaker {
    private ArrayList<Memento> mementos = new ArrayList<>();

    public void addMemento(Memento m) {
        mementos.add(m);
    }

    public Memento getMemento() {
        return mementos.get(1);
    }
}

```

---

```

public class MementoDemo {
    public static void main(String[] args) {
        Caretaker caretaker = new Caretaker();
        Originator originator = new Originator();
        originator.setState("State1");
        originator.setState("State2");
        caretaker.addMemento( originator.save() );
        originator.setState("State3");
        caretaker.addMemento( originator.save() );
        originator.setState("State4");
        originator.restore( caretaker.getMemento() );
    }
}

```