# Wallet Risk Scoring

This project computes a risk score (0–1000) for each Ethereum wallet based on synthetic behavioral data mimicking interactions with DeFi lending protocols such as Compound or Aave.

##  Overview

Given a list of wallet addresses, the system generates synthetic financial and activity features, normalizes them, and calculates a weighted risk score. A higher score implies more trustworthy behavior.


**Data Collection Method**
We start with a list of wallet addresses in a CSV file. Instead of fetching real blockchain data, we create example data that mimics what real transaction data would look like. This way, we can still run the scoring process and understand how it works.

**Feature Selection**
We use these features because they help us understand how risky each wallet is when borrowing from Compound:

Total borrowed amount: More borrowed money means more risk.

Number of liquidations: If a wallet got liquidated before, it’s risky.

Largest borrow: Having one very big loan is riskier than many small ones.

Repayment ratio: If a wallet repays loans well, it’s less risky.

Collateralization ratio: If the collateral is low compared to the borrowed amount, it’s risky.

Active assets: Using many different assets spreads risk, so it’s safer.

Protocol age: Wallets that have been using the protocol longer usually manage risk better.

**Scoring Method**
We convert all feature values to a scale from 0 to 1, so we can compare them fairly.

For things that are “good” (like repayment ratio or having many assets), we flip the scale so a higher value means lower risk.

We give each feature a weight depending on how important it is for risk.

We add up all these weighted values and multiply by 1000 so the score is between 0 (low risk) and 1000 (high risk).

**Justification**
Liquidations show if someone had trouble paying back.

Borrowed amount and largest borrow show how much money is at stake.

Repayment rate and collateral amount show how responsible the user is.

Having many assets means less chance of sudden loss.

Being active for a long time shows experience and better risk handling.

This basic approach helps us quickly spot risky wallets based on their borrowing behavior and history.

##  How to Run

### 1. Requirements

 Python 3.7+
 pandas
 numpy

### 2. Install dependencies
pip install pandas numpy


### 3. Place your wallet list

Add or modify `Wallet.csv` with a `wallet_id` column listing your target wallet addresses.

### 4. Run the script

python main.py


### 5. Output

Scores will be written to `wallet_scores.csv` in the same directory.

## Handling Inactive Wallets

Wallets that do not match expected criteria or are missing data are handled gracefully using synthetic generation. This allows all wallet IDs to receive a score, avoiding disruptions in batch processing.

## Example

**Input (`Wallet.csv`):**

wallet_id
0xabc123...
0xdef456...


**Output (`wallet_scores.csv`):**
wallet_id,score
0xabc123...,783
0xdef456...,412


