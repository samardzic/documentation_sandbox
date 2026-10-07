## 3. Display Multiplication Table

Create an algorithm and flowchart that input a number and display its multiplication table from 1 to 10 using a loop.

---

### ✔ PseudoCode

```text

START

    Input num

    For i = 1 TO 10

        Prodcut = number * i

        PRINT Product

    ENDFOR
    
END  

```

### ✔ Flowchart

```mermaid

flowchart TD
    A(["Start"]) --> B[\Input any number\]
    B --> C["Initiate Counter = 1"]
    C --> D{"Counter <= 10 ?"} 
    D --> |Yes| E[" Product = num * Counter"]
    D --> |No| F["End"]
    E --> G["Increment Counter"]
    G --> D
    G --> H[/Display Product Table /]
    H --> F

```
