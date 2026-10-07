## 15. Store Checkout with Multiple Items

Write the algorithm and draw the flowchart for a program that inputs the number of items purchased, calculates the total purchase amount using a loop, and applies a **15% discount** if the total exceeds 5000 SEK.

---

### ✔ Pseudocode

```text

START

    INPUT Items

    SET Counter = 1

    SET Total_Purchase_Amount = 0

    WHILE Counter <= Items

        INPUT Amount

        Total_Purchase_Amount = Total_Purchase_Amount + Amount

        Counter = Counter + 1

    ENDWHILE

    IF Total_Purchase_Amount > 5000 

        Discount = Total_Purchase_Amount * 0.15

        Total_Amount = Total_Purchase_Amount - Discount

    ELSE

        Total_Amount = Total_Purchase_Amount

   ENDIF

   PRINT Total_Amount

END 


```

### ✔ Flowchart

```mermaid

flowchart TD

    A(["Start"]) --> B[/Input Items/]
    B --> C["Counter = 1"]
    C --> D[Total Purchase Amount = 0]
    D --> E{"Counter <= Items ?"}
    E --> |Yes| F[/Input Amount/]
    F --> G["Calculate Total Purchase Amount = Total Purchase Amount + Amount"]
    G --> H["Counter = Counter + 1"]
    H --> E
    E --> |No| I{"Total Purchase Amount > 5000 ?"}
    I --> |Yes| J["Calculate Discount = Total Purchase Amount * 0.15 "]
    J --> K["Calculate Total Amount = Total Purchase Amount - Discount"]
    K --> L[/Display Total Amount/]
    I --> |No| M["Total_Amount = Total_Purchase_Amount"]
    M --> L
    L --> N(["End"])
