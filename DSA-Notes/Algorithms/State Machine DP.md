# State Machine DP

## 📌 Definition
DP where the state explicitly models a finite state machine — transitions between modes like hold/not-hold, cooldown, or number of transactions.

## ⚙️ How It Works
- Define all possible states (e.g., `hold`, `sold`, `rest`)
- At each step, transition between states with associated costs
- `hold[i] = max(hold[i-1], rest[i-1] - price[i])`
- `sold[i] = hold[i-1] + price[i]`
- `rest[i] = max(rest[i-1], sold[i-1])`
- Answer: best terminal state after processing all inputs

## 🧠 When to Use
- Stock trading with transaction limits
- Cooldown constraints
- At-most-k transactions

## 🧪 Problems
- [[DSA Problem Bank#Best Time to Buy and Sell Stock with Cooldown]]
- [[DSA Problem Bank#Best Time to Buy and Sell Stock III]]
- [[DSA Problem Bank#Best Time to Buy and Sell Stock IV]]

## ❌ Mistakes
- Incorrect state transitions (draw state diagram first)
- Initialising `hold` state to 0 instead of -∞

## 🔗 Related Topics
[[Dynamic Programming]] | [[Greedy Algorithms]]

---
- [ ] Learned  - [ ] Practiced  - [ ] Mastered
#DSA #DynamicProgramming #StateMachine #Algorithm
