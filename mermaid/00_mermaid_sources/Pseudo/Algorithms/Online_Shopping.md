## 11. Online Shopping Delivery Eligibility

Write the algorithm and draw the flowchart for a program that inputs a customer's purchase amount and displays **"Free Delivery"** if the amount is 500 SEK or more; otherwise display **"Delivery Charge Applies"**.

---

### ✔ Pseudocode

```text

START

    INPUT Purchase_Amount

    IF Purchase_Amount >= 500

        PRINT "Free Delivery"

    ELSE

        PRINT "Delivery Charge Applies"

    ENDIF

END

```

### ✔ Flowchart

```mermaid

flowchart TD
    
    A(["Start"]) --> B[/Input Purchase_Amount/]
    B --> C{"Purchase_Amount >= 500 ?"}
    C  --> |Yes| D["Display Free Delivery"]
    C --> |No| E["Display Delivery Charge Applies"]
    D --> F(["End"])
    E --> F
    

```