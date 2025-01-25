# Decentralized Peer-Review Publishing System

## Overview
This Clarity smart contract implements a decentralized peer-review publishing system for research papers. It allows authors to submit research papers, reviewers to evaluate them, and rewards reviewers for their efforts. The system also provides mechanisms to challenge reviews and manage reviewer stakes, promoting transparency and fairness in the peer-review process.

## Key Features
- **Paper Submission**: Authors can submit research papers with an associated IPFS hash and unique paper ID.
- **Reviewer Registration**: Participants can register as reviewers by staking a specified amount of STX tokens.
- **Paper Reviews**: Registered reviewers can submit reviews for research papers, including a score and an IPFS hash of their comments.
- **Reviewer Rewards**: Reviewers receive STX tokens as a reward for each review.
- **Challenges**: Reviews can be challenged by other participants for transparency and accountability.
- **Paper Status Management**: Authors can update the status of their papers (e.g., "pending," "reviewed," "accepted," "rejected").
- **Administrative Controls**: The contract owner can update contract settings and manage reviewer statuses.

---

## Contract Components

### Constants
The contract defines error codes for various failure scenarios to ensure robustness:
- `ERR-NOT-AUTHORIZED`: Unauthorized access.
- `ERR-PAPER-NOT-FOUND`: Paper does not exist.
- `ERR-ALREADY-REVIEWED`: Paper already reviewed by the user.
- `ERR-INVALID-SCORE`: Invalid review score.
- `ERR-INSUFFICIENT-BALANCE`: Insufficient balance to perform the operation.
- `ERR-NOT-REVIEWER`: User is not a registered reviewer.
- `ERR-PAPER-EXISTS`: Paper with the given ID already exists.
- `ERR-EMPTY-HASH`: Missing IPFS hash.
- `ERR-INVALID-ID`: Invalid paper ID.
- `ERR-EMPTY-REASON`: Challenge reason cannot be empty.
- `ERR-SELF-CHALLENGE`: Self-challenging reviews is not allowed.
- `ERR-EMPTY-STATUS`: Status cannot be empty.
- `ERR-INVALID-STATUS`: Invalid paper status.
- `ERR-ALREADY-REGISTERED`: User already registered as a reviewer.

### Data Variables
- `min-stake`: Minimum STX required to stake as a reviewer.
- `review-reward`: STX reward per review.
- `contract-owner`: The address of the contract owner.

### Data Maps
- **Papers**: Stores information about submitted papers, including:
  - Author's address
  - IPFS hash of the paper
  - Paper status (e.g., "pending")
  - Review count and total score
  - Submission timestamp

- **Reviews**: Stores reviews submitted by reviewers, including:
  - Reviewer’s address
  - Review score and IPFS hash of the comment
  - Review timestamp and status

- **Reviewers**: Tracks registered reviewers, including:
  - Stake amount, review count, and reputation
  - Status (e.g., "active," "paused")

---

## Public Functions

### 1. `submit-paper`
- **Description**: Allows authors to submit a new research paper.
- **Parameters**:
  - `ipfs-hash`: IPFS hash of the paper (max 64 characters).
  - `paper-id`: Unique identifier for the paper.
- **Validations**:
  - `ipfs-hash` must not be empty.
  - `paper-id` must be unique.

### 2. `register-reviewer`
- **Description**: Allows users to register as reviewers by staking STX tokens.
- **Validations**:
  - User must not already be registered.
  - User must have sufficient STX balance to meet the minimum stake.

### 3. `submit-review`
- **Description**: Allows registered reviewers to submit reviews for a paper.
- **Parameters**:
  - `paper-id`: ID of the paper to review.
  - `score`: Review score (0 to 100).
  - `comment-hash`: IPFS hash of the review comment.
- **Validations**:
  - Reviewer must be registered and active.
  - Review score must be within the valid range.
  - Reviewer cannot review their own paper.

### 4. `withdraw-stake`
- **Description**: Allows paused or inactive reviewers to withdraw their staked tokens.
- **Validations**:
  - Reviewer must have "paused" or "inactive" status.

### 5. `challenge-review`
- **Description**: Allows users to challenge a review by staking tokens and providing a reason.
- **Parameters**:
  - `paper-id`: ID of the paper.
  - `reviewer`: Address of the reviewer.
  - `reason`: Reason for the challenge (max 256 characters).
- **Validations**:
  - Challenge reason must not be empty.
  - User cannot challenge their own review.

### 6. `update-paper-status`
- **Description**: Allows authors to update the status of their papers.
- **Parameters**:
  - `paper-id`: ID of the paper.
  - `new-status`: New status of the paper (e.g., "accepted").
- **Validations**:
  - New status must be valid and non-empty.
  - Only the author of the paper can update its status.

### 7. `update-settings`
- **Description**: Allows the contract owner to update the minimum stake and review reward amounts.
- **Parameters**:
  - `new-min-stake`: New minimum stake amount.
  - `new-review-reward`: New review reward amount.
- **Validations**:
  - Only the contract owner can perform this action.

### 8. `pause-reviewer`
- **Description**: Allows the contract owner to pause a reviewer.
- **Parameters**:
  - `reviewer`: Address of the reviewer to pause.
- **Validations**:
  - Only the contract owner can perform this action.

---

## Read-Only Functions

### 1. `get-paper-details`
- **Description**: Retrieves details of a specific paper.
- **Parameters**:
  - `paper-id`: ID of the paper.

### 2. `get-review-details`
- **Description**: Retrieves details of a specific review.
- **Parameters**:
  - `paper-id`: ID of the paper.
  - `reviewer`: Address of the reviewer.

### 3. `get-reviewer-details`
- **Description**: Retrieves details of a specific reviewer.
- **Parameters**:
  - `reviewer`: Address of the reviewer.

### 4. `get-reviewer-earnings`
- **Description**: Calculates the total earnings of a reviewer based on the number of reviews completed.
- **Parameters**:
  - `reviewer`: Address of the reviewer.

---

## Administrative Functions

### `update-settings`
- Allows the contract owner to update the minimum stake and review reward values.

### `pause-reviewer`
- Allows the contract owner to pause a reviewer’s activity.

---

## Deployment & Usage
- Deploy the smart contract using a Clarity-compatible blockchain (e.g., Stacks blockchain).
- Use Clarity SDKs or a compatible frontend to interact with the contract.
- Ensure users have sufficient STX balance to cover staking and transaction costs.

---

## Future Enhancements
- Implement penalties for fraudulent challenges or invalid reviews.
- Add a reputation decay mechanism for inactive reviewers.
- Enable multi-sig functionality for administrative controls.
- Integrate advanced analytics to assess reviewer performance.

