---
title: "Merkle Tree: Complete Guide with Mermaid Diagrams"
created: 2025-11-17
updated: 2025-11-18
type: source
status: reference
source: "Confluence · TK - Team Kepler"
url: https://axonivy.atlassian.net/wiki/spaces/TK/pages/48884776996/Merkle+Tree+Complete+Guide+with+Mermaid+Diagrams
confluence_id: "48884776996"
confluence_path: "Team Kepler > Developer note > LUZ Audit Refactor- 2025-2026 > Luz Audit System - Performance Optimization Proposal"
tags: [confluence, luz-audit, performance]
---

# Merkle Tree: Complete Guide with Mermaid Diagrams

*Confluence source · Team Kepler › Developer note › LUZ Audit Refactor- 2025-2026 › Luz Audit System - Performance Optimization Proposal · [view original](https://axonivy.atlassian.net/wiki/spaces/TK/pages/48884776996/Merkle+Tree+Complete+Guide+with+Mermaid+Diagrams) · updated 2025-11-18*

## Table of Content

------------------------------------------------------------------------

### What is a Merkle Tree?

A **Merkle Tree** (also called a **Hash Tree**) is a cryptographic data structure that allows efficient and secure verification of large data sets. Named after Ralph Merkle who patented it in 1979, it's fundamental to blockchain technology, distributed systems, and version control systems like Git.

#### Core Concept

A Merkle Tree is a binary tree where:

- **Leaf nodes** contain hashes of data blocks

- **Non-leaf nodes** contain hashes of their child nodes

- **Root node** (Merkle Root) represents the hash of the entire dataset

![[image-20251118-001610.png]]

### Why Use Merkle Trees?

#### Benefits

1.  **Efficient Verification**: Verify data integrity in O(log n) time

2.  **Space Efficient**: Store only hashes, not full data

3.  **Tamper Detection**: Any change propagates to root hash

4.  **Partial Verification**: Prove specific data exists without downloading entire dataset

5.  **Parallel Construction**: Build tree concurrently

#### Use Cases

- **Blockchain**: Bitcoin, Ethereum transaction verification

- **Distributed Systems**: IPFS, BitTorrent file integrity

- **Databases**: Apache Cassandra anti-entropy

- **Version Control**: Git commit verification

- **Certificate Transparency**: TLS certificate auditing

### Building a Merkle Tree: Step-by-Step

#### Step 1: Hash the Data Blocks

First, apply a cryptographic hash function (e.g., SHA-256) to each data block.

![[image-20251118-004438.png]]

> [!note]- Code Example
>
>
>
> ```
> import java.security.MessageDigest;
> import java.security.NoSuchAlgorithmException;
> import java.util.Arrays;
> import java.util.List;
> import java.util.stream.Collectors;
>
> public class MerkleTreeHelper {
>
>     public static String hashData(String data) {
>         try {
>             MessageDigest digest = MessageDigest.getInstance("SHA-256");
>             byte[] hash = digest.digest(data.getBytes());
>             return bytesToHex(hash);
>         } catch (NoSuchAlgorithmException e) {
>             throw new RuntimeException("SHA-256 algorithm not found", e);
>         }
>     }
>
>     private static String bytesToHex(byte[] bytes) {
>         StringBuilder hexString = new StringBuilder();
>         for (byte b : bytes) {
>             String hex = Integer.toHexString(0xff & b);
>             if (hex.length() == 1) hexString.append('0');
>             hexString.append(hex);
>         }
>         return hexString.toString();
>     }
> }
>
> // Usage
> List<String> dataBlocks = Arrays.asList("Data A", "Data B", "Data C", "Data D");
> List<String> leafHashes = dataBlocks.stream()
>     .map(MerkleTreeHelper::hashData)
>     .collect(Collectors.toList());
>
> // Result:
> // leafHashes = [
> //   "3f4b2a1c...",  // Hash A
> //   "7a2c9e5d...",  // Hash B
> //   "9e1d3f8b...",  // Hash C
> //   "2b8f4c7a..."   // Hash D
> // ]
> ```
>
>
>

#### Step 2: Pair and Hash (Level 1)

Combine adjacent hashes and hash the result.

![[image-20251118-004733.png]]

> [!note]- Code Example:
>
>
>
> ```
> public class MerkleTreeHelper {
>     // ... previous hashData method ...
>
>     public static List<String> pairwiseHash(List<String> hashes) {
>         List<String> level = new ArrayList<>();
>
>         for (int i = 0; i < hashes.size(); i += 2) {
>             String left = hashes.get(i);
>             // Duplicate if odd number of elements
>             String right = (i + 1 < hashes.size()) ? hashes.get(i + 1) : left;
>             String combined = hashData(left + right);
>             level.add(combined);
>         }
>
>         return level;
>     }
> }
>
> // Usage
> List<String> level1 = MerkleTreeHelper.pairwiseHash(leafHashes);
> // level1 = ["5d7e...", "8c3a..."]  // [Hash AB, Hash CD]
> ```
>
>
>

#### Step 3: Continue Until Root

Repeat the process until only one hash remains - the Merkle Root.

![[image-20251118-004916.png]]

#### Complete Merkle Tree Flow

![[image-20251118-004954.png]]

> [!note]- Complete Implementation
>
>
>
> ```
> import java.security.MessageDigest;
> import java.security.NoSuchAlgorithmException;
> import java.util.*;
> import java.util.stream.Collectors;
>
> public class MerkleTree {
>     private List<String> leaves;
>     private List<List<String>> layers;
>     private String root;
>
>     public MerkleTree(List<String> dataBlocks) {
>         this.leaves = dataBlocks.stream()
>             .map(MerkleTree::hashData)
>             .collect(Collectors.toList());
>         this.layers = new ArrayList<>();
>         this.layers.add(new ArrayList<>(this.leaves));
>         this.root = buildTree();
>     }
>
>     private String buildTree() {
>         List<String> currentLevel = new ArrayList<>(this.leaves);
>
>         while (currentLevel.size() > 1) {
>             currentLevel = pairwiseHash(currentLevel);
>             this.layers.add(new ArrayList<>(currentLevel));
>         }
>
>         return currentLevel.get(0); // Root hash
>     }
>
>     public String getRoot() {
>         return this.root;
>     }
>
>     private static List<String> pairwiseHash(List<String> hashes) {
>         List<String> level = new ArrayList<>();
>
>         for (int i = 0; i < hashes.size(); i += 2) {
>             String left = hashes.get(i);
>             String right = (i + 1 < hashes.size()) ? hashes.get(i + 1) : left;
>             String combined = hashData(left + right);
>             level.add(combined);
>         }
>
>         return level;
>     }
>
>     private static String hashData(String data) {
>         try {
>             MessageDigest digest = MessageDigest.getInstance("SHA-256");
>             byte[] hash = digest.digest(data.getBytes());
>             return bytesToHex(hash);
>         } catch (NoSuchAlgorithmException e) {
>             throw new RuntimeException("SHA-256 algorithm not found", e);
>         }
>     }
>
>     private static String bytesToHex(byte[] bytes) {
>         StringBuilder hexString = new StringBuilder();
>         for (byte b : bytes) {
>             String hex = Integer.toHexString(0xff & b);
>             if (hex.length() == 1) hexString.append('0');
>             hexString.append(hex);
>         }
>         return hexString.toString();
>     }
>
>     // Usage example
>     public static void main(String[] args) {
>         List<String> dataBlocks = Arrays.asList("Data A", "Data B", "Data C", "Data D");
>         MerkleTree tree = new MerkleTree(dataBlocks);
>         System.out.println("Merkle Root: " + tree.getRoot());
>     }
> }
> ```
>
>
>

### Handling Odd Numbers of Nodes

#### Approach 1: Duplication (Bitcoin)

Duplicate the last node when there's an odd number at any level.

![[image-20251118-005858.png]]

**Bitcoin Method**:

- Odd node at each level is duplicated

- Creates balanced tree structure

- May result in multiple duplications (EE → EEEE)

#### Approach 2: Promotion (Ethereum)

Promote the odd node directly to the next level without duplication.

![[image-20251118-010047.png]]

**Ethereum Method**:

- Odd node is promoted to next level unchanged

- No duplication, more efficient storage

- Creates asymmetric tree structure

#### Approach 3: Padding with Zero/Empty Hash

Pad with a predefined empty hash (e.g., hash of empty string).

![[image-20251118-010334.png]]

**Padding Method**:

- Add empty/zero hashes to make even number

- Results in balanced, symmetric tree

- Empty hash is typically hash(empty string) or 0x000...000

### Merkle Proof: Verification Without Full Data

A **Merkle Proof** allows verifying a specific data block exists in the tree without downloading all data.

#### Proof Structure for Verifying "Data B"

![[image-20251118-010455.png]]

#### Verification Path (Flowchart)

![[image-20251118-010654.png]]

#### Proof Components

**Merkle Proof for Data B**:

```
{
  "leaf": "Hash B",
  "proof": [
    {"position": "left", "hash": "Hash A"},
    {"position": "right", "hash": "Hash CD"}
  ],
  "root": "Hash ABCD"
}
```

> [!note]- Proof Implementation
>
>
>
> ```
> public class MerkleTree {
>     // ... previous code ...
>
>     // Inner class for proof elements
>     public static class ProofElement {
>         private final String position; // "left" or "right"
>         private final String hash;
>
>         public ProofElement(String position, String hash) {
>             this.position = position;
>             this.hash = hash;
>         }
>
>         public String getPosition() { return position; }
>         public String getHash() { return hash; }
>     }
>
>     public List<ProofElement> getProof(int dataIndex) {
>         List<ProofElement> proof = new ArrayList<>();
>         int index = dataIndex;
>
>         // Traverse from leaf to root
>         for (int level = 0; level < this.layers.size() - 1; level++) {
>             List<String> currentLayer = this.layers.get(level);
>             boolean isRightNode = index % 2 == 1;
>             int siblingIndex = isRightNode ? index - 1 : index + 1;
>
>             if (siblingIndex < currentLayer.size()) {
>                 String position = isRightNode ? "left" : "right";
>                 proof.add(new ProofElement(position, currentLayer.get(siblingIndex)));
>             }
>
>             index = index / 2;
>         }
>
>         return proof;
>     }
>
>     public static boolean verify(String data, List<ProofElement> proof, String root) {
>         String computedHash = hashData(data);
>
>         for (ProofElement element : proof) {
>             if (element.getPosition().equals("left")) {
>                 computedHash = hashData(element.getHash() + computedHash);
>             } else {
>                 computedHash = hashData(computedHash + element.getHash());
>             }
>         }
>
>         return computedHash.equals(root);
>     }
>
>     // Usage example
>     public static void main(String[] args) {
>         List<String> dataBlocks = Arrays.asList("Data A", "Data B", "Data C", "Data D");
>         MerkleTree tree = new MerkleTree(dataBlocks);
>
>         // Get proof for Data B (index 1)
>         List<ProofElement> proof = tree.getProof(1);
>
>         // Verify the proof
>         boolean isValid = MerkleTree.verify("Data B", proof, tree.getRoot());
>         System.out.println("Proof valid: " + isValid); // true
>     }
> }
> ```
>
>
>

### Proof Size Comparison

#### Complexity Table

|              |             |                |               |                   |
|--------------|-------------|----------------|---------------|-------------------|
| Dataset Size | Tree Height | Proof Elements | Full Download | Complexity        |
| 4            | 2           | 2              | O(n)          | O(log₂ 4) = 2     |
| 16           | 4           | 4              | O(n)          | O(log₂ 16) = 4    |
| 1,024        | 10          | 10             | O(n)          | O(log₂ 1024) = 10 |
| 1,048,576    | 20          | 20             | O(n)          | O(log₂ 1M) ≈ 20   |

> **Key Insight**: Verification complexity is O(log n) instead of O(n)

### Updating a Merkle Tree

When data changes, only the path from leaf to root needs recomputation:

#### Before: Original Tree

![[image-20251118-011410.png]]

#### After: Data B → Data B' (Updated)

![[image-20251118-011519.png]]

**Update Path**: B' → AB' → AB'CD (only 3 hashes recomputed)

**Unchanged**: A, C, D, CD (no recomputation needed)

**Key Insight**: Only O(log n) nodes need updating, not O(n)

> [!note]- Update Implementation
>
>
>
> ```
> public class MerkleTree {
>     // ... previous code ...
>
>     public void updateLeaf(int index, String newData) {
>         // Update leaf
>         this.layers.get(0).set(index, hashData(newData));
>
>         // Recompute path to root
>         int currentIndex = index;
>         for (int level = 0; level < this.layers.size() - 1; level++) {
>             int siblingIndex = (currentIndex % 2 == 0)
>                 ? currentIndex + 1
>                 : currentIndex - 1;
>
>             int left = Math.min(currentIndex, siblingIndex);
>             int right = Math.max(currentIndex, siblingIndex);
>
>             List<String> currentLayer = this.layers.get(level);
>             String leftHash = currentLayer.get(left);
>             String rightHash = (right < currentLayer.size())
>                 ? currentLayer.get(right)
>                 : leftHash;
>
>             int parentIndex = currentIndex / 2;
>             this.layers.get(level + 1).set(parentIndex, hashData(leftHash + rightHash));
>
>             currentIndex = parentIndex;
>         }
>
>         this.root = this.layers.get(this.layers.size() - 1).get(0);
>     }
>
>     // Usage example
>     public static void main(String[] args) {
>         List<String> dataBlocks = Arrays.asList("Data A", "Data B", "Data C", "Data D");
>         MerkleTree tree = new MerkleTree(dataBlocks);
>
>         System.out.println("Original Root: " + tree.getRoot());
>
>         // Update Data B to Data B'
>         tree.updateLeaf(1, "Data B'");
>
>         System.out.println("Updated Root: " + tree.getRoot());
>     }
> }
> ```
>
>
>

### Security Properties

#### Tamper Detection

##### Original Tree (Valid)

![[image-20251118-011948.png]]

##### Tampered Tree (Attack Detected!)

![[image-20251118-012230.png]]

> **Detection**: Root hash mismatch reveals tampering immediately!
>
> **Attack Path**: B → X causes Hash B → Hash X, which cascades to Hash AB → Hash AX, then Hash ABCD → Hash AXCD
>
> **Verification**: User compares computed root (Hash AXCD) with trusted root (Hash ABCD) → **Mismatch detected!**

#### Attack Resistance Flow

![[image-20251118-012509.png]]

#### Second Preimage Resistance

![[image-20251118-012726.png]]

> [!note]- Mitigation Strategy: Prefix leaf/node hashes differently
>
>
>
> ```
> import java.security.MessageDigest;
> import java.security.NoSuchAlgorithmException;
>
> public class SecureMerkleTree {
>
>     // Hash leaf nodes with prefix "0x00"
>     public static String hashLeaf(String data) {
>         try {
>             MessageDigest digest = MessageDigest.getInstance("SHA-256");
>             byte[] hash = digest.digest(("0x00" + data).getBytes());
>             return bytesToHex(hash);
>         } catch (NoSuchAlgorithmException e) {
>             throw new RuntimeException("SHA-256 algorithm not found", e);
>         }
>     }
>
>     // Hash internal nodes with prefix "0x01"
>     public static String hashNode(String left, String right) {
>         try {
>             MessageDigest digest = MessageDigest.getInstance("SHA-256");
>             byte[] hash = digest.digest(("0x01" + left + right).getBytes());
>             return bytesToHex(hash);
>         } catch (NoSuchAlgorithmException e) {
>             throw new RuntimeException("SHA-256 algorithm not found", e);
>         }
>     }
>
>     private static String bytesToHex(byte[] bytes) {
>         StringBuilder hexString = new StringBuilder();
>         for (byte b : bytes) {
>             String hex = Integer.toHexString(0xff & b);
>             if (hex.length() == 1) hexString.append('0');
>             hexString.append(hex);
>         }
>         return hexString.toString();
>     }
> }
> ```
>
>
>

### Real-World Applications

#### 1. Bitcoin Transaction Verification (SPV)

![[image-20251118-013015.png]]

**SPV Benefits**:

- Light wallets store only ~80 bytes per block header

- Verify transaction with ~10 hashes (~320 bytes) instead of ~1MB full block

- Mobile wallets can run on limited storage/bandwidth

#### 2. Git Version Control

![[image-20251118-013138.png]]

**Git Properties**:

- Changing any file cascades hash changes to commit

- Entire repository history is tamper-evident

- Efficient change detection (compare tree hashes)

#### 3. IPFS Content Addressing

![[image-20251118-013327.png]]

**IPFS Benefits**:

- Content-addressable: Same content → Same hash

- Automatic deduplication across entire network

- Verifiable data retrieval from untrusted sources

### Performance Characteristics

#### Time Complexity

![[image-20251118-013600.png]]

#### Space Complexity

|           |          |                                 |
|-----------|----------|---------------------------------|
| Component | Space    | Example (1M items)              |
| Full tree | O(2n)    | ~2M hashes (~64MB with SHA-256) |
| Proof     | O(log n) | ~20 hashes (~640 bytes)         |
| Root only | O(1)     | 1 hash (32 bytes)               |

### Advanced Variants

#### Multiway Merkle Trees

![[image-20251118-013807.png]]

**Trade-off**: Shorter tree (log₃ n) but larger proofs (need 2 siblings per level instead of 1)

#### Merkle Mountain Ranges

For append-only logs with efficient appends:

![[image-20251118-014247.png]]

#### Sparse Merkle Trees

For key-value stores with large address space (e.g., Ethereum state):

![[image-20251118-014355.png]]

**Properties**:

- Fixed depth (e.g., 256 levels for SHA-256 address space)

- Most leaves are empty (use default value)

- Store only non-empty subtrees → space efficient

### Best Practices

#### Common Pitfalls

![[image-20251118-014637.png]]

#### Security Best Practices

> [!note]- Click here to expand...
>
>
>
> ```
> import java.security.MessageDigest;
> import java.security.NoSuchAlgorithmException;
>
> public class MerkleTreeBestPractices {
>
>     // 1. Prefix leaf and node hashes differently
>     public static String hashLeaf(String data) {
>         return hashData("LEAF:" + data);
>     }
>
>     public static String hashNode(String left, String right) {
>         return hashData("NODE:" + left + right);
>     }
>
>     // 2. Use strong hash functions (SHA-256 for production)
>     private static String hashData(String data) {
>         try {
>             MessageDigest digest = MessageDigest.getInstance("SHA-256"); // Production-ready
>             byte[] hash = digest.digest(data.getBytes());
>             return bytesToHex(hash);
>         } catch (NoSuchAlgorithmException e) {
>             throw new RuntimeException("SHA-256 algorithm not found", e);
>         }
>     }
>
>     // 3. Validate tree structure
>     public static void validateTree(MerkleTree tree) {
>         int expectedHeight = (int) Math.ceil(Math.log(tree.getLeavesCount()) / Math.log(2));
>         assert tree.getLayersCount() == expectedHeight + 1
>             : "Invalid tree height";
>
>         // Verify all parent-child relationships
>         for (int level = 0; level < tree.getLayersCount() - 1; level++) {
>             // ... verification logic
>         }
>     }
>
>     // 4. Use length prefixing to prevent collisions
>     public static String safeHash(String left, String right) {
>         String prefixed = left.length() + left + right.length() + right;
>         return hashData(prefixed);
>     }
>
>     private static String bytesToHex(byte[] bytes) {
>         StringBuilder hexString = new StringBuilder();
>         for (byte b : bytes) {
>             String hex = Integer.toHexString(0xff & b);
>             if (hex.length() == 1) hexString.append('0');
>             hexString.append(hex);
>         }
>         return hexString.toString();
>     }
> }
> ```
>
>
>

### Decision Flow: When to Use Merkle Trees

![[image-20251118-014936.png]]

### Summary

#### Key Takeaways

**Structure**:

- Binary tree of hashes

- Leaf nodes contain data hashes

- Root represents entire dataset hash

**Building**:

- O(n) construction time

- Hash pairwise bottom-up approach

- Handle odd nodes by duplication

**Verification**:

- O(log n) proof size

- O(log n) verification time

- Exponential efficiency gain over O(n)

**Updates**:

- O(log n) recomputation

- Only path to root changes

- Efficient for large datasets

**Security**:

- Tamper-evident structure

- Cryptographic hash properties

- Collision resistant

- Second preimage resistant

**Applications**:

- Blockchain: Bitcoin, Ethereum transaction verification

- Distributed Systems: IPFS content addressing

- Version Control: Git commit integrity

- Databases: Cassandra anti-entropy

- Certificate Transparency: TLS auditing

#### When to Use Merkle Trees

##### ✓ Use When:

- Efficient verification of large datasets needed

- Building tamper-evident data structures

- Implementing light client protocols (blockchain SPV)

- Distributed system consistency checks required

- Content-addressed storage systems (IPFS)

- Version control with integrity verification (Git)

##### ✗ Don't Use When:

- Dataset is very small (\<10 items) - overhead not worth it

- Need fast random access - use hash table instead

- Frequent random updates - recomputation expensive

- No verification requirements - simpler structures suffice

- Simple in-memory data structures work fine

#### Complexity Summary

|                     |          |          |                    |
|---------------------|----------|----------|--------------------|
| Operation           | Time     | Space    | Notes              |
| Build               | O(n)     | O(2n)    | One-time cost      |
| Proof               | O(log n) | O(log n) | Efficient!         |
| Verify              | O(log n) | O(1)     | Very fast          |
| Update              | O(log n) | O(1)     | Path recomputation |
| Storage (root only) | \-       | O(1)     | 32 bytes (SHA-256) |

------------------------------------------------------------------------

### Further Reading

- [Bitcoin Whitepaper](https://bitcoin.org/bitcoin.pdf) - Section 7: Simplified Payment Verification

- [Certificate Transparency](https://certificate.transparency.dev/) - Real-world Merkle tree application

- [IPFS Merkle DAG](https://docs.ipfs.tech/concepts/merkle-dag/) - Content addressing with Merkle trees

- [Ethereum Yellow Paper](https://ethereum.github.io/yellowpaper/paper.pdf) - Patricia Merkle Trees
