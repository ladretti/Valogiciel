# Exercise 4: SonarQube Static Analysis & Quick Fixes

## Three Important Issues Identified

### 1. Cognitive Complexity of Methods (java:S3776)
**File:** BankAccountApp.java:18

The `main` method contains deeply nested loops and multiple conditional structures (if, switch), making it difficult to follow and maintain. Refactoring into smaller methods would improve readability and reduce bug risk.

### 2. Duplicated String Literals (java:S1192)
**File:** BankAccountApp.java:139

The string `"Account dosen't exist"` is repeated multiple times. Defining it as a constant would centralize the message and facilitate future modifications.

### 3. Try-with-resources should be used (java:S2093)
**File:** Bank.java:106

Many resources in Java need to be closed after use. If not, the garbage collector cannot reclaim their memory, leading to leaks and performance issues. Java 7 introduced try-with-resources, which automatically closes resources. This syntax is safer than manual try-catch-finally. The rule flags methods not using try-with-resources for closeable resources, improving code reliability.


## Fix at least 2 of these issues

### Fix for Issue 2: Duplicated String Literals (java:S1192)
- **File modified:** BankAccountApp.java
- **Summary:** Replaced duplicated string literals with a constant to centralize the message and improve maintainability.
- **Details:** Added a constant `ACCOUNT_NOT_FOUND_MESSAGE` and replaced 4 occurrences of `"Account dosen't exist"` in the code.

**Before:**
```java
System.out.println("Account dosen't exist");
```

**After:**
```java
private static final String ACCOUNT_NOT_FOUND_MESSAGE = "Account dosen't exist";
// ...
System.out.println(ACCOUNT_NOT_FOUND_MESSAGE);
```

### Fix for Issue 3: Try-with-resources should be used (java:S2093)
- **Files modified:** BankAccount.java, Bank.java, Person.java, BankAccountApp.java
- **Summary:** Refactored methods to use try-with-resources for automatic resource management instead of manual closing. This prevents resource leaks and improves code safety.
- **Details:**
  - BankAccount.java (loadFromText): Used try-with-resources for FileInputStream and Scanner.
  - Bank.java (saveAccounts): Used try-with-resources for FileOutputStream and OutputStreamWriter.
  - Person.java (constructor): Used try-with-resources for Scanner.
  - BankAccountApp.java (main): Wrapped Scanner in try-with-resources.

**Example: Bank.java saveAccounts method (before and after)**

*Before:*
```java
public void saveAccounts(Bank accManager) {
    FileOutputStream fos = null;
    OutputStreamWriter osw = null;
    try {
        fos = new FileOutputStream("C:\\Users\\jay4k\\Desktop\\stuff\\Bankaccountinfo\\BankAccountinfotext.text");
        osw = new OutputStreamWriter(fos);
        for (int i = 0; i < Accounts.size(); i++) {
            BankAccount tmp = Accounts.get(i);
            osw.write(tmp.convertToText(tmp));
        }
    } catch (IOException e) {
        System.out.println("Error writing to file");
    } finally {
        if (osw != null) {
            try {
                osw.close();
            } catch (IOException e) {
                // no action
            }
        }
        if (fos != null) {
            try {
                fos.close();
            } catch (IOException e) {
                // no action
            }
        }
    }
}
```

*After:*
```java
public void saveAccounts(Bank accManager) {
    try (FileOutputStream fos = new FileOutputStream("C:\\Users\\jay4k\\Desktop\\stuff\\Bankaccountinfo\\BankAccountinfotext.text");
         OutputStreamWriter osw = new OutputStreamWriter(fos)) {
        for (int i = 0; i < Accounts.size(); i++) {
            BankAccount tmp = Accounts.get(i);
            osw.write(tmp.convertToText(tmp));
        }
    } catch (IOException e) {
        System.out.println("Error writing to file");
    }
}
```

## Confirmation après re-run SonarLint
Après avoir re-lancé l'analyse SonarLint sur le même scope (dossier jay-bank), les issues corrigées ont disparu de la vue des problèmes :
- L'issue "Duplicated String Literals (java:S1192)" n'apparaît plus pour la chaîne `"Account dosen't exist"`.
- L'issue "Try-with-resources should be used (java:S2093)" n'apparaît plus dans Bank.java et les autres fichiers modifiés.
- Les variables inutilisées (`averageBalance` et `initMoneyAmount`) ne génèrent plus d'alertes.

## Analyse WMC/CBO
Oui, les issues SonarLint apparaissent plus fréquemment dans les classes avec WMC/CBO élevés. Par exemple, BankAccountApp.java (WMC élevé dû aux boucles imbriquées) concentre les problèmes de complexité cognitive et duplication de chaînes, tandis que des classes plus simples comme Person.java (CBO bas) en ont moins. Cela confirme que les métriques de complexité prédisent les zones à risque pour les code smells.