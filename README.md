# AI-Generated Synthetic Test Data

## (a) The technology and how AI supports it

Software teams need realistic data to test applications, but copying real customer data into test environments creates privacy and compliance risks. Synthetic data generation uses AI to create fake records that behave like real data without belonging to any real person:

1. Generative models learn patterns from real data, like age distributions, policy types, or transaction amounts, and produce new records with the same statistical patterns.
2. AI can keep relationships between tables intact (for example, a customer record that correctly links to their policies and claims), which simple random data generators struggle with.
3. Teams can generate rare edge cases on demand, like unusual claim types or invalid inputs, to test systems more thoroughly.

## (e) Concerns

1. If the model memorizes its training data, it might reproduce real customer records, which defeats the privacy purpose.
2. Synthetic data can look realistic but miss important real-world patterns, which means bugs could slip through testing.
3. If the original data is biased, the synthetic data will copy that bias.
4. **Mitigation:** Measure privacy risk (for example, checking how close synthetic records are to real ones), validate statistical similarity, and still test with masked production-like data when needed.

