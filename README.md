## Sequence Aggregation Sybil Detection
1.Reported Addresses
A total of 24 groups with the same behavior were found. The list of sybils after deduplication is 542. For a detailed list, see the linked document.new_group_sybil_rst_df.xlsx

[final sybil group]sheet Field description:
new_group_id: a cluster with the same behavior, belonging to an entity control
seq_n: has a continuous sequence of the same behavior, length is 5
seq_dt_nunique: the number of days that the sequence repeats
seq_life_start_time: The time when the sequence first started
seq_life_start_hour: The time when the sequence first started (accurate to the hour)
seq_life_end_time: The time when the sequence last ended
seq_life_end_hour: The time when the sequence last ended (accurate to the hour)
New_sybil_flag: 1- newly identified ; 0-sybil initialList wallet

[sybil group network]sheet description:
Each group has a direct URL linking to the Arkham network graph, which shows the source of funds for the sybil wallets, Please note that the time range for the first deposit of the wallets has been filtered.


Example - Group 17 network:

2.Description
Sybils usually use scripts to operate a large number of accounts in a short period of time, and perform batch consistency of actions. Based on this, a long sequence can be used to detect the homogeneous operation of a batch of accounts in a short period of time, and observe whether the same sequence of operations can occur in multiple days. Identified as a sybil.
It is particularly important to note that after the first stage of sybil surrender and institutional detection, most of the low-quality mechanized sybil have been detected, so many of the "unidentified sybil" we can detect now are "high-quality accounts", although they Multi-chain interaction and relatively high balance (>100$), but the core sybil identification factors remain unchanged:
Short-term batch operations
Repeatedly for many days
Number of group members>20
Funding sources and trading networks are related
3.Detailed Methodology & Walkthrough
Each day's data is processed separately, and is spliced ​​into one EVENT by splicing "SOURCE_CHAIN"-"PROJECT"-"DESTINATION_CHAIN";
Obtaining the user's N consecutive EVENTs in time sequence is seq_N;
For example, "DFK-DeFi Kingdoms-Klaytn Mainnet Cypress;DFK-DeFi Kingdoms-Klaytn Mainnet Cypress;DFK-DeFi Kingdoms-Klaytn Mainnet Cypress;DFK-DeFi Kingdoms-Klaytn Mainnet Cypress;DFK-DeFi Kingdoms-Klaytn Mainnet Cypress" represents the user's continuous Executed 5 transactions from DFK to Klaytn using DeFi Kingdoms;
Example - Group 12:

Screen wallets whose sequence behavior occurs within a short period of time (5 minutes);
Filter wallet behavior sequences that first appear in the same hour in all time ranges,Last seen within the same hour is a group;
Screen wallets that have the same sequence behavior for multiple days (>1days);
Eliminate those already identified as sybil initialList wallet;


#Model parameters
SEQ_LEN =
MIN_GROUP_SIZE = 20
MAX_DURATION_SECONDS = 300
MIN_SEQ_DT_NUNIQUE = 2

4. Model verification
1. After Dune query, the repeatability of layerzero transactions in all wallets under the group is very high.
Example - Group 12:

2. Observing the group in batches on Arkham shows that the source of funds is the same and the dates are consistent, and the CounterParties and Dapps have a high overlap.
Example - Group 12:

3.Wallet portfolio and transactions are all very similar, even the Debank Avatar is the same, them attack all Potential airdrop protocol,Not just L0,Example under group_id 12 wallet,daily Still there continued constantly farming；
Example - Group 12 Portfolio:



5. Reward Address (If Eligible)
0xb8e18be0acdf99a5bd2fd27c501e9c8709af20ea

