---
title: Decorator & Memento 
theme: white
css: style.css

revealOptions:
    transition: 'fade'
    controls: false
    progress: false
---

### Engenharia de Software

---

### Decorator Pattern

---

### Objetivo 

Adicionar mais responsabilidades a um objeto dinâmicamente. Decorators dão uma alternativa flexivel à criação de sublcasses para o aumento de funcionalidade.

---

O Decorator Pattern te permite mudar o comportamento de um objeto em tempo de execução, usando-o em um objeto de classe decorator.

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
