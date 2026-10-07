## 1. Check Even or Odd Number

Design an algorithm and flowchart that take a number as input and
determine whether it is even or odd.

### ✔ Pseudocode

```text
START

    INPUT number

    IF number % 2 == 0 THEN

        PRINT Even

    ELSE

        PRINT Odd

    ENDIF

END
```

### ✔ Flowchart

```mermaid
  flowchart TD
    start(["Start"]) --> input[/"Input number n"/]
    input --> check{"Is n mod 2 = 0 ?"}
    check -->|Yes| even["Output: Even"]
    check -->|No| odd["Output: Odd"]
    even --> finish(["End"])
    odd --> finish

```


