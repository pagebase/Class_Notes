# Table of contents

1. [Market Basket Analysis](#market-basket-analysis)

---
# Market Basket Analysis

Of course! Here is a comprehensive, step-by-step guide to solving Market Basket Analysis (MBA) problems, from the fundamental concepts to practical implementation.

### What is Market Basket Analysis?

Market Basket Analysis (MBA) is a data mining technique used to discover patterns of co-occurrence in data. It's most famously used in retail to understand the purchase behavior of customers by identifying items that are frequently bought together. The classic example is the discovery that **"customers who buy diapers often buy beer as well."**

The technique is based on measuring the strength of the relationship between items using key metrics: **Support, Confidence, and Lift.**

---

### Step 1: Understand the Core Concepts & Metrics

You cannot solve an MBA problem without these three metrics. They are the foundation.

#### 1. Itemset
A collection of one or more items. For example, `{Milk, Bread}` is a 2-itemset.

#### 2. Support
This measures how *frequently* an itemset appears in all transactions. It's about popularity.

*   **Formula:** `Support(A) = (Number of transactions containing A) / (Total number of transactions)`
*   **What it tells you:** A high support value means the itemset is very common. You often set a *minimum support threshold* to filter out rare, uninteresting itemsets.

#### 3. Confidence
This measures how often the rule `A -> B` has been found to be true. It's about the *reliability* of the inference.

*   **Formula:** `Confidence(A -> B) = Support(A ∪ B) / Support(A)`
    *   (Read as: "Support of A *and* B together, divided by the Support of just A")
*   **What it tells you:** "If a customer buys A, how likely are they to also buy B?" A high confidence (e.g., 80%) means the rule is very reliable. However, it can be misleading if B is very popular on its own.

#### 4. Lift
This measures how much more likely item B is to be purchased when item A is purchased, **while controlling for how popular B is**. It's the true measure of the *strength* of the association.

*   **Formula:** `Lift(A -> B) = Confidence(A -> B) / Support(B)`
    *   This can also be written as: `Lift(A -> B) = Support(A ∪ B) / (Support(A) * Support(B))`
*   **Interpreting Lift:**
    *   **Lift = 1:** A and B are independent. Buying A does not affect the probability of buying B.
    *   **Lift > 1 (e.g., 3):** A and B are positively correlated. Buying A makes it **3 times more likely** that B will be bought. This is a valuable rule.
    *   **Lift < 1 (e.g., 0.5):** A and B are negatively correlated. Buying A makes it *less* likely that B will be bought.

---

### Step 2: The Apriori Algorithm - The How-To

The most common algorithm for performing MBA is the **Apriori Algorithm**. Its power comes from its "downward closure property" (the Apriori principle):
> *"If an itemset is frequent, then all of its subsets must also be frequent."*
> Conversely, if a subset is infrequent, its supersets cannot be frequent.

This principle drastically reduces the number of itemsets we need to check.

The Apriori algorithm works in two main stages and iterates through itemset sizes (k=1, k=2, k=3, etc.):

1.  **Generate Candidate Itemsets:** Generate all possible k-itemsets.
2.  **Prune Itemsets:** Prune (remove) any itemset that has a support lower than your minimum threshold OR that contains an infrequent subset (using the Apriori principle).

Let's see this in action with a classic example.

---

### Step 3: A Detailed Walkthrough Example

**Transaction Data:**
| Transaction ID | Items Bought                     |
| :------------: | -------------------------------- |
|       1        | {Milk, Bread, Butter}            |
|       2        | {Beer, Bread, Diapers, Eggs}     |
|       3        | {Milk, Bread, Diapers, Beer}     |
|       4        | {Bread, Butter, Milk}            |
|       5        | {Beer, Diapers, Coke}            |

**Our Goal:** Find strong association rules (e.g., `{Diapers} -> {Beer}`) with a **minimum support of 40%** (i.e., an itemset must appear in at least 2 out of 5 transactions) and a **minimum confidence of 60%**.

#### Step 3.1: Find Frequent Itemsets

**Iteration 1 (k=1): Find Frequent 1-itemsets.**
Scan the database and count the occurrence of each single item.

| Itemset | Support Count | Support (%) |
| :-----: | :-----------: | :---------: |
|  Milk   |       3       |     60%     |
|  Bread  |       4       |     80%     |
| Butter  |       2       |     40%     |
|  Beer   |       3       |     60%     |
| Diapers |       3       |     60%     |
|  Eggs   |       1       |     20%     |
|  Coke   |       1       |     20%     |

**Prune:** Remove itemsets with support < 40% (2 transactions). So, we remove `{Eggs}` and `{Coke}`.
**Frequent 1-itemsets (F1):** `{Milk}`, `{Bread}`, `{Butter}`, `{Beer}`, `{Diapers}`

**Iteration 2 (k=2): Find Frequent 2-itemsets.**
*   **Generate Candidates (C2):** Join F1 with itself to get all possible pairs: `{Milk, Bread}`, `{Milk, Butter}`, `{Milk, Beer}`, `{Milk, Diapers}`, `{Bread, Butter}`, `{Bread, Beer}`, `{Bread, Diapers}`, `{Butter, Beer}`, `{Butter, Diapers}`, `{Beer, Diapers}`.
*   **Scan Database & Calculate Support:**

|       Itemset        | Support Count | Support (%) |
| :------------------: | :-----------: | :---------: |
|    {Milk, Bread}     |       3       |     60%     |
|    {Milk, Butter}    |       2       |     40%     |
|    {Milk, Beer}      |       1       |     20%     |
|   {Milk, Diapers}    |       1       |     20%     |
|   {Bread, Butter}    |       2       |     40%     |
|    {Bread, Beer}     |       2       |     40%     |
|   {Bread, Diapers}   |       2       |     40%     |
|   {Butter, Beer}     |       0       |      0%     |
|  {Butter, Diapers}   |       0       |      0%     |
|   {Beer, Diapers}    |       3       |     60%     |

*   **Prune:**
    1.  Remove itemsets with support < 40%: `{Milk, Beer}`, `{Milk, Diapers}`, `{Butter, Beer}`, `{Butter, Diapers}`.
    2.  Also, check the Apriori principle: All subsets of the remaining itemsets are in F1, so no further pruning is needed.
*   **Frequent 2-itemsets (F2):** `{Milk, Bread}`, `{Milk, Butter}`, `{Bread, Butter}`, `{Bread, Beer}`, `{Bread, Diapers}`, `{Beer, Diapers}`

**Iteration 3 (k=3): Find Frequent 3-itemsets.**
*   **Generate Candidates (C3):** Join F2 with itself. The only possible 3-itemsets whose all 2-item subsets are frequent are:
    *   `{Milk, Bread, Butter}` (subsets: {Milk,Bread}, {Milk,Butter}, {Bread,Butter} are all in F2)
    *   `{Bread, Beer, Diapers}` (subsets: {Bread,Beer}, {Bread,Diapers}, {Beer,Diapers} are all in F2)
*   **Scan Database & Calculate Support:**

|         Itemset         | Support Count | Support (%) |
| :---------------------: | :-----------: | :---------: |
|   {Milk, Bread, Butter}   |       2       |     40%     |
|  {Bread, Beer, Diapers} |       2       |     40%     |

*   **Prune:** Both meet the minimum support threshold. Their subsets are all frequent by design.
*   **Frequent 3-itemsets (F3):** `{Milk, Bread, Butter}`, `{Bread, Beer, Diapers}`

We stop here as no 4-itemsets can be generated.

#### Step 3.2: Generate Association Rules from Frequent Itemsets

Now, we generate rules from our frequent itemsets (F2 and F3) and calculate their Confidence and Lift. We'll only keep rules with Confidence >= 60%.

**Example 1: From the itemset {Beer, Diapers} (Support = 60%)**
*   Candidate Rule 1: `{Beer} -> {Diapers}`
    *   Confidence = Support({Beer, Diapers}) / Support({Beer}) = 0.6 / 0.6 = **1.0 (100%)**
    *   Lift = Confidence / Support({Diapers}) = 1.0 / 0.6 ≈ **1.67**
*   Candidate Rule 2: `{Diapers} -> {Beer}`
    *   Confidence = Support({Beer, Diapers}) / Support({Diapers}) = 0.6 / 0.6 = **1.0 (100%)**
    *   Lift = 1.0 / 0.6 ≈ **1.67**

Both rules have 100% confidence and a lift greater than 1, indicating a strong, positive relationship. **This is our "Diapers and Beer" rule!**

**Example 2: From the itemset {Milk, Bread} (Support = 60%)**
*   Rule: `{Milk} -> {Bread}`
    *   Confidence = Support({Milk, Bread}) / Support({Milk}) = 0.6 / 0.6 = **1.0 (100%)**
    *   Lift = 1.0 / Support({Bread}) = 1.0 / 0.8 = **1.25**
*   Rule: `{Bread} -> {Milk}`
    *   Confidence = 0.6 / 0.8 = **0.75 (75%)**
    *   Lift = 0.75 / 0.6 = **1.25**

The rule `{Bread} -> {Milk}` has 75% confidence (>60%), so it's a strong rule. The lift is 1.25, meaning buying bread makes it 1.25x more likely to buy milk.

**Example 3: From the itemset {Bread, Beer, Diapers} (Support = 40%)**
*   Rule: `{Bread, Beer} -> {Diapers}`
    *   Confidence = Support(All) / Support({Bread, Beer}) = 0.4 / 0.4 = **1.0 (100%)**
*   ...and so on for other combinations.

---

### Step 4: Interpretation and Action

You would now present the strongest rules (high Confidence and Lift > 1) to a business stakeholder.

*   **Rule:** `{Diapers} -> {Beer}` (Confidence: 100%, Lift: 1.67)
*   **Business Insight:** "Every customer who buys diapers also buys beer. This is a very strong and reliable pattern. These products are highly associated."
*   **Possible Actions:**
    *   Place diapers and beer in the same aisle or on adjacent shelves to encourage this natural behavior.
    *   Create a "Saturday Night" bundle promotion for these items.
    *   **Do not** separate them, as they are frequently bought together.

---

### Step 5: Practical Implementation with Code (Python)

In the real world, you don't do this manually. You use libraries. Here's how it's done in Python using `mlxtend`.

```python
import pandas as pd
from mlxtend.frequent_patterns import apriori, association_rules
from mlxtend.preprocessing import TransactionEncoder

# 1. Load your data (this is our example data)
dataset = [['Milk', 'Bread', 'Butter'],
           ['Beer', 'Bread', 'Diapers', 'Eggs'],
           ['Milk', 'Bread', 'Diapers', 'Beer'],
           ['Bread', 'Butter', 'Milk'],
           ['Beer', 'Diapers', 'Coke']]

# 2. One-hot encode the data
te = TransactionEncoder()
te_ary = te.fit(dataset).transform(dataset)
df = pd.DataFrame(te_ary, columns=te.columns_)

# 3. Find frequent itemsets using Apriori
frequent_itemsets = apriori(df, min_support=0.4, use_colnames=True)
print("Frequent Itemsets:")
print(frequent_itemsets)

# 4. Generate all association rules
rules = association_rules(frequent_itemsets, metric="confidence", min_threshold=0.6)
print("\nAssociation Rules:")
print(rules[['antecedents', 'consequents', 'support', 'confidence', 'lift']])

# 5. (Optional) Filter for the most interesting rules
strong_rules = rules[(rules['lift'] > 1.2) & (rules['confidence'] > 0.8)]
print("\nStrong Rules (Lift > 1.2 & Confidence > 0.8):")
print(strong_rules[['antecedents', 'consequents', 'support', 'confidence', 'lift']])
```

This code will automatically output the frequent itemsets and all association rules, calculating support, confidence, and lift for you.

### Summary Checklist for Solving an MBA Problem

1.  [ ] **Understand the Problem & Metrics:** Know what Support, Confidence, and Lift mean.
2.  [ ] **Set Thresholds:** Define your min_support (to find common itemsets) and min_confidence (to find reliable rules).
3.  [ ] **Find Frequent Itemsets:** Use the Apriori algorithm (manually or with code) to find all itemsets that meet the min_support threshold.
4.  [ ] **Generate Rules:** For each frequent itemset, generate all possible rules.
5.  [ ] **Calculate Metrics:** For each rule, calculate its Confidence and Lift.
6.  [ ] **Filter Rules:** Keep only the rules that meet your min_confidence threshold and have a Lift > 1.
7.  [ ] **Interpret & Recommend:** Translate the strongest rules into actionable business insights.
