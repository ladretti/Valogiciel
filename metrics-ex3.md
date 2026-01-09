| Class | WMC | CBO | LCOM | Quick notes |
|-------|-----|-----|-----|-------------|
| Bank | 14 | 4 | 0 | Highest complexity per method (AMC) and highest coupling. |
| BankAccount | 20 | 3 | 44 | Largest file, but well-encapsulated (DAM: 1.0). |
| Person | 23 | 3 | 79 | Highest number of methods and worst cohesion. |

## Analysis & Answers

### 1. Which class has the highest WMC?

**Person** has the highest WMC at **23**.

**Interpretation:** This means Person has the most methods. While WMC can sometimes be weighted by cyclomatic complexity, even at a raw count, this class is doing the most "work" in terms of available actions.

### 2. Which class has the highest CBO?

**Bank** has the highest CBO at **4**.

**Interpretation:** The Bank class is connected to the most outside classes. High coupling makes a class "fragile"—if you change one of the classes it depends on, the Bank class is the most likely to break or require a rewrite.

### 3. Looking at WMC + CBO + LCOM together: Which class would you worry about most?

**Person** is the most concerning for future maintenance.

**Why?** While Bank is more coupled (CBO), Person exhibits a significant cohesion problem:

- **High LCOM (79):** An LCOM of 79 is quite high. It indicates methods don't share the same data fields.
- **The Smell:** This signals a God Class—unrelated logic bundled together, making it hard to maintain.
- **Maintenance Risk:** Bug fixes in one area (e.g., "name formatting") may inadvertently break others (e.g., "identity verification").
