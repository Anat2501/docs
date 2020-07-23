---
description: This page outlines the tables in Kasparov's relational database schema.
---

# Database Schema

## blocks

This table contains the blocks in the blockDAG.

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| id | BIGINT UNSIGNED | Primary Key |
| block\_hash | CHAR\(64\) | Index |
| accepting\_block\_id | BIGINT UNSIGNED | References blocks\(id\) |
| version | INT |  |
| hash\_merkle\_root | CHAR\(64\) |  |
| accepted\_id\_merkle\_root | CHAR\(64\) |  |
| utxo\_commitment | CHAR\(64\) |  |
| timestamp | DATETIME | Index |
| bits | INT UNSIGNED | Compact representation of the target the block header hash must be less than or equal to. |
| nonce | BIGINT UNSIGNED |  |
| blue\_score | BIGINT UNSIGNED |  |
| is\_chain\_block | TINYINT | Index |
| mass | BIGING |  |

## parent\_blocks

This One-to-Many relation table contains the blockDAG edges \(arrows between each block and its parent blocks\). Each block may point to several parent blocks and be pointed at by several child blocks.

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| block\_id | BIGINT UNSIGNED | Primary Key, References blocks\(id\) |
| parent\_block\_id | BIGINT UNSIGNED | Primary Key, References blocks\(id\) |

{% hint style="info" %}
The primary key is compound.
{% endhint %}

## raw\_blocks

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| block\_id | BIGINT UNSIGNED | Primary Key, References blocks\(id\) |
| block\_data | MEDIUMBLOB |  |

## subnetworks

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| id | BIGINT UNSIGNED | Primary Key |
| subnetwork\_id | CHAR\(64\) | Index |
| gas\_limit | BIGINT UNSIGNED | The subnetwork's gas limit |

## transactions

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| id | BIGINT UNSIGNED | Primary Key |
| accepting\_block\_id | BIGINT UNSIGNED | Index, References blocks\(id\) |
| transaction\_hash | CHAR\(64\) | Unique Index |
| transaction\_id | CHAR\(64\) | Index. Actual TransactionID as defined by Kaspa Protocol |
| lock\_time | BIGINT UNSIGNED |  |
| subnetwork\_id | BIGINT UNSIGNED |  |
| gas | BIGINT UNSIGNED |  |
| payload\_hash | CHAR\(64\) |  |
| payload | BLOB |  |
| mass | BIGINT |  |

## transactions\_to\_blocks

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| transaction\_id | BIGINT UNSIGNED | Primary Key, References transactions\(id\) |
| block\_id | BIGINT UNSIGNED | Primary Key, References blocks\(id\) |
| index | INT UNSIGNED | Index |

{% hint style="info" %}
The primary key is compound.
{% endhint %}

## transaction\_inputs

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| id | BIGINT UNSIGNED | Primary Key |
| transaction\_id | BIGINT UNSIGNED | Index, References transactions\(id\) |
| transaction\_output\_id | BIGINT UNSIGNED | Index, References transaction\_outputs\(id\) |
| index | INT UNSIGNED |  |
| signature\_script | BLOB |  |
| sequence | BIGINT UNSIGNED |  |

## transaction\_outputs

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| id | BIGINT UNSIGNED | Primary Key |
| transaction\_id | BIGINT UNSIGNED | Index. References transactions\(id\) |
| index | INT UNSIGNED |  |
| value | BIGINT UNSIGNED |  |
| is\_spent | TINYINT | Index |
| script\_pub\_key | BLOB |  |
| address\_id | BIGINT UNSIGNED | References addresses\(id\) |

## addresses

| Column Name | Type | Notes |
| :--- | :--- | :--- |
| id | BIGINT UNSIGNED | Primary Key |
| address | VARCHAR\(64\) | Unique Index |

