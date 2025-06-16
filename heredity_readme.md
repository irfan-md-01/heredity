# Heredity - Genetic Trait Probability Calculator

An AI system that assesses the likelihood that a person will have a particular genetic trait using Bayesian networks and probability theory.

## Project Overview

This project models genetic inheritance patterns using the GJB2 gene, which is one of the leading causes of hearing impairment in newborns. The AI system calculates probability distributions for:
- How many copies of a gene each person has (0, 1, or 2 copies)
- Whether each person exhibits a particular trait based on their genes

## Background

The system models genetic inheritance using a Bayesian Network that considers:
- **Gene Variables**: Number of copies of a particular gene (0, 1, or 2)
- **Trait Variables**: Whether a person exhibits the trait (True/False)
- **Inheritance Patterns**: How genes are passed from parents to children
- **Mutation Probability**: Chance of gene mutation during inheritance

## Features

- Models genetic inheritance across family generations
- Handles incomplete information (unknown gene counts or traits)
- Calculates joint probabilities for complex genetic scenarios
- Normalizes probability distributions for accurate inference
- Supports families with multiple generations

## Installation

1. Download the distribution code:
   ```bash
   wget https://cdn.cs50.net/ai/2023/x/projects/2/heredity.zip
   unzip heredity.zip
   cd heredity
   ```

2. Ensure Python 3.12 is installed on your system.

## Usage

Run the program with a CSV data file:

```bash
python heredity.py data/family0.csv
```

### Sample Output

```
Harry:
  Gene:
    2: 0.0092
    1: 0.4557
    0: 0.5351
  Trait:
    True: 0.2665
    False: 0.7335

James:
  Gene:
    2: 0.1976
    1: 0.5106
    0: 0.2918
  Trait:
    True: 1.0000
    False: 0.0000
```

## Data Format

Input data should be in CSV format with the following columns:
- `name`: Person's name
- `mother`: Mother's name (empty if unknown)
- `father`: Father's name (empty if unknown)
- `trait`: Whether person has the trait (1 for yes, 0 for no, empty if unknown)

### Example CSV:
```csv
name,mother,father,trait
Harry,Lily,James,
James,,,1
Lily,,,0
```

## Implementation Details

The system implements three core functions:

### 1. `joint_probability(people, one_gene, two_genes, have_trait)`
Calculates the joint probability of a specific genetic configuration across all family members.

**Parameters:**
- `people`: Dictionary of family members and their information
- `one_gene`: Set of people with exactly one copy of the gene
- `two_genes`: Set of people with exactly two copies of the gene
- `have_trait`: Set of people who exhibit the trait

**Returns:** Joint probability as a float

### 2. `update(probabilities, one_gene, two_genes, have_trait, p)`
Updates the probability distributions by adding the joint probability to appropriate categories.

**Parameters:**
- `probabilities`: Dictionary of probability distributions for each person
- `p`: Joint probability to add to the distributions

### 3. `normalize(probabilities)`
Normalizes all probability distributions so they sum to 1 while maintaining relative proportions.

## Probability Constants

The system uses predefined probabilities in the `PROBS` dictionary:

- **Gene Distribution**: `PROBS["gene"]` - Unconditional probability of having 0, 1, or 2 copies
- **Trait Given Gene**: `PROBS["trait"]` - Probability of having trait given gene count
- **Mutation Rate**: `PROBS["mutation"]` - Probability of gene mutation (default: 1%)

## Algorithm

1. **Enumerate all possible configurations** of gene counts and traits for all family members
2. **Calculate joint probability** for each configuration using inheritance rules
3. **Update probability distributions** by accumulating joint probabilities
4. **Normalize distributions** to ensure they sum to 1

## Key Concepts

- **Bayesian Networks**: Models dependencies between genetic variables
- **Conditional Probability**: Trait probability depends on gene count
- **Genetic Inheritance**: Children inherit one gene from each parent
- **Mutation**: Genes can mutate during inheritance with small probability

## Testing

Test your implementation:
```bash
check50 ai50/projects/2024/x/heredity
```

Check code style:
```bash
style50 heredity.py
```

## Example Calculation

For a family where:
- Lily has 0 genes, no trait
- James has 2 genes, has trait  
- Harry (child) has 1 gene, no trait

The joint probability calculation involves:
1. Lily: P(0 genes) × P(no trait | 0 genes) = 0.96 × 0.99 = 0.9504
2. James: P(2 genes) × P(trait | 2 genes) = 0.01 × 0.65 = 0.0065
3. Harry: P(1 gene from parents) × P(no trait | 1 gene) = 0.9802 × 0.44 = 0.431288

Joint probability = 0.9504 × 0.0065 × 0.431288 ≈ 0.00266

## Files

- `heredity.py` - Main implementation file
- `data/` - Directory containing sample family data files
  - `family0.csv` - Sample family data
  - `family1.csv` - Additional test data
  - `family2.csv` - Additional test data

## Requirements

- Python 3.12
- No external dependencies (uses only Python standard library)
- Optional: numpy or pandas (if familiar)

## Academic Integrity

This project is part of CS50's Introduction to Artificial Intelligence with Python. Implement the required functions yourself without using external AI tools or copying solutions.

## License

This project is part of CS50's coursework and follows Harvard's academic policies.