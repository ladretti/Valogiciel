# Class Metrics

| Classe | LOC | NOM | Évaluation |
|--------|-----|-----|-----------|
| Bank | 412 | 12 | Trop gros : 393 lignes pour 14 méthodes. |
| BankAccount | 469 | 18 | Très gros : 405 lignes pour 20 méthodes. |
| Person | 324 | 21 | Surdimensionné : 21 méthodes et 294 lignes pour une classe simple. |
| BankAccountApp | 482 | 2 | Anormal : 447 lignes pour 2 méthodes. Main() énorme |


# ⇒ Do you feel its size roughly matches its responsibility?
Non. Les classes sont surdimensionnées pour leurs responsabilités :

Bank et BankAccount : trop de lignes pour le nombre de méthodes (ratios LOC/NOM élevés)
Person : 21 méthodes pour une classe simple - violation du SRP (Single Responsibility Principle)
BankAccountApp : anormal - 482 lignes concentrées dans 2 méthodes (Main() énorme)
