# BankAccount Metrics

- WMC: 20
- NOM: 20
- CC `withdrawMoney()`: 5

## Méthode actuelle

```java
public boolean withdrawMoney(double withdrawAmount) {
    // Decision Point 1: Le "if" principal
    // Decision Point 2: L'opérateur "&&" (court-circuit si le montant est négatif)
    // Decision Point 3: L'opérateur "&&" (court-circuit si le solde est insuffisant)
    // Decision Point 4: L'opérateur "&&" (court-circuit si la limite cumulée est dépassée)
    if (withdrawAmount >= 0 
    && balance >= withdrawAmount 
    && withdrawAmount < withdrawLimit 
    && withdrawAmount + amountWithdrawn <= withdrawLimit) 
    {

        balance = balance - withdrawAmount;
        success = true;
        amountWithdrawn += withdrawAmount;
    } else {
        success = false;
    }
    return success;
}
```
## Refactoring proposé :

```java
public boolean withdrawMoney(double withdrawAmount) {
    if (withdrawAmount >= 0 && balance >= withdrawAmount 
        && (withdrawAmount + amountWithdrawn <= withdrawLimit)) {
        
        balance -= withdrawAmount;
        amountWithdrawn += withdrawAmount;
        return true;
    }
    return false;
}
```
## Cyclomatic Complexity (CC) par méthode
- `withdrawMoney(double arg0)`: 4
