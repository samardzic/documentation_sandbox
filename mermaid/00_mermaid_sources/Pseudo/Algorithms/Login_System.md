## 14. Login System (Maximum 3 Attempts)

Create an algorithm and flowchart for a login system that allows a user up to 3 attempts to enter the correct password. Display **"Access Granted"** if the password is correct; otherwise display **"Account Locked"** after 3 failed attempts.

---

### ✔ Pseudocode

```text

START

    SET Counter = 1

    SET Attempt = Failed

    WHILE Counter <= 3

        INPUT Password

        IF Password = StoredPassword

            Attempt = Success

            BREAK

        ENDIF

        Counter = counter + 1

    ENDWHILE

    IF Attempt = Success

        PRINT "Access Granted"

    ELSE

        PRINT "Account Locked"

    ENDIF

END

```

### ✔ Flowchart

```mermaid

flowchart TD

    A(["Start"]) --> B["Counter = 1"]
    B --> C["Attempt = Failed"]
    C --> D{"Counter <= 3?"}
    D -->|Yes| E[/Input Password/]
    E --> F{"Password = Stored Password?"}
    F -->|Yes| G["Attempt = Success"]
    G --> J{"Attempt = Success?"}
    F -->|No| H["Counter = Counter + 1"]
    H --> D
    D -->|No| J
    J -->|Yes| K[/Display "Access Granted"/]
    J -->|No| L[/Display "Account Locked"/]
    K --> M(["End"])
    L --> M

```