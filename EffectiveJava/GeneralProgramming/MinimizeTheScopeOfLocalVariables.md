# 57 Minimize the scope of local variables

> By minimizing the scope of local variables, you increase the **readability** and **maintainability**

- **Declare a local variable where it is first used.** Otherwise, it's clutter and user might forget about its ype and initial value and the scope of it can be too extended which could lead to disastrous consequence.
- **Try to make every local variable declaration contain an initializer.** Except for try-catch statement.
- **Prefer `for` loops to `while` loops if you don't need the contents of loop variable after the loop terminates.**

  - Prefer `for-each` to `for` but if you need access to the iterator, use `for` loop

    ```java
    for (Element e:c){
        ... // do something with e
    }
    ```

    ```java
    for (Iterator<Element> i = c.iterator(); i.hasNext();){
        Element e = i.next();
        ... // do something with e and i
    }
    ```

  - `while` loop can cause a copy-and-paste error which is silent and hard to detect, while in contrast, `for` loop would fail to compile in same situation.

  ```java
  Iterator<Element> i = c.iterator();
  while (i.hasNext()){
      doSomething(i.next());
  }
  ...

  Iterator<Element> i2 = c2.iterator();
  while (i.hasNext()){  // BUG! but it's hard to detect
      doSomething(i2.next());
  }
  ```

  ```java
  for (Iterator<Element> i = c.iterator(); i.hasNext()){
      Element e = i.next();
      ... // Do something with e and i
  }
  ...

  for(Iterator<Element> i2 = c2.iterator(); i.hasNext()){
      Element e2 = i2.next();
      ... // Do something with e2 and i2
  }
  ```

- Add additional loop variable if the loop test involves a method invocation that is guaranteed to return the same result on each iteration

```java
for (int i = 0, n = expensiveComputation(); i < n; i++){
    ... // Do something with i
}
```

- Keep method small and focused. Local variables to one activity may be in the scope of the code performing the other activity.
