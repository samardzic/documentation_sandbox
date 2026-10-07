## 6. Average Temperature Calculation

Write the algorithm and draw the flowchart for a program that takes the temperature of 7 days, finds the average temperature, and displays it.

---
### ✔ Pseudocode

```text

START

    SET Sum = 0
    
    For counter = 1 TO 7

        INPUT Temp

        Sum = Sum  + Temp

    ENDFOR

    Average = Sum / 7

    PRINT Average

END

```

### ✔ Flowchart

```mermaid

flowchart TD

        A(["Start"]) --> B["Set Sum =0"]
        B --> C["Set Counter = 1"]
        C --> D{"Counter <= 7 ?"}
        D --> |Yes| E[/Input Temp/]
        D --> |No| F(["End"])
        E --> G["Calculate Sum = Sum + Temp"]
        G --> H["Counter = Counter + 1"]
        H --> D
        H --> I["Calculate Average = Sum/7"]
        I --> J[/Display Average/]
        J --> F

```



    
