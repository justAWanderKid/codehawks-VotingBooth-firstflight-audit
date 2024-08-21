
### [HIGH-1] Incorrect Reward Calculation for Voters (Financial Discrepancy)

**Description**

The reward distribution logic contains an incorrect calculation for `rewardPerVoter`. The code divides `totalRewards` by `totalVotes`, which includes both `For` and `Against` votes. However, the rewards should only be distributed to `For` voters, leading to an inaccurate amount being sent to each eligible voter.

**Impact**

This vulnerability leads to an incorrect distribution of rewards, where each `For` voter receives less than their rightful share. This discrepancy could undermine the fairness and integrity of the voting process, leading to dissatisfaction among participants.

**Proof of Concepts**

Assume there are 5 voters: 3 vote `For`, and 2 vote `Against`. The contract mistakenly calculates `rewardPerVoter` by dividing the `totalRewards` by 5 instead of 3. This results in each `For` voter receiving only 60% of the intended reward.

```javascript
    // rewardPerVoter = 1 ether / 5 = 200e15 this is how much each `for` voter will receive
    // the contract will leave with `400e15`
    uint256 rewardPerVoter = totalRewards / totalVotes;
```

**Recommended mitigation**

The calculation of `rewardPerVoter` should be modified to divide `totalRewards` by the number of `For` voters (`totalVotesFor`) instead of the total votes (`totalVotes`). This ensures that only eligible voters receive the correct amount of rewards.

```diff
-    uint256 rewardPerVoter = totalRewards / totalVotes;
+    uint256 rewardPerVoter = totalRewards / totalVotesFor;
```

This modification will ensure that the rewards are distributed correctly, maintaining the integrity and fairness of the voting process.


### [HIGH-2] Execution of Arbitrary OS Commands via `ffi` 

**Description**

inside the test suite The test function `testPwned` leverages Foundry's `ffi` (Foreign Function Interface) to execute arbitrary OS commands within the testing environment. Specifically, it uses the `touch` command to create a file named "youve-been-pwned-remember-to-turn-off-ffi!". This capability poses a severe security risk, as it allows arbitrary code execution on the host machine, potentially leading to system compromise.

**Impact**

an attacker could perform a wide range of malicious activities, including but not limited to file manipulation, data exfiltration, or even complete system compromise. It exposes the development and CI/CD environments to severe risks, including the possibility of unauthorized access and data loss.

**Recommended mitigation**

set ffi to `false` inside the `foundry.toml`.


### [MED-1] Incompatibility with Arbitrum Due to Solidity Version (Deployment Issue)

**Description**

The contract is written in Solidity version 0.8.23, which introduces the `PUSH0` opcode. This opcode is not supported on the Arbitrum network. As a result, attempting to deploy this contract on Arbitrum will fail, leading to incompatibility issues.

**Impact**

Contracts intended for deployment on Arbitrum will be unable to deploy successfully, causing operational disruption.

**Recommended mitigation**

To ensure compatibility with Arbitrum, downgrade the Solidity version to `0.8.19` or earlier, where the `PUSH0` opcode is not present. Alternatively, deploy the contract on a network that supports the opcode if compatibility with Arbitrum is not a requirement.
