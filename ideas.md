Recursion is easier to understand when you stop trying to look at the whole picture.

Use classes for actions.

```C#
using System;

interface List<T> {
  int Length();
}

class Nil<T> : List<T> {
  public int Length() { return 0; }
}

class Cons<T>: List<T> {
  public Cons(T head, List<T> tail) {
    Head = head;
    Tail = tail;
  }
  public T Head;
  public List<T> Tail;

  public int Length() {
    return 1 + Tail.Length();
  }
}

class MainClass {
  public static void Main (string[] args) {
    Console.WriteLine (new Cons<int>(1, new Cons<int>(2, new Nil<int>())).Length());
  }
}
```
