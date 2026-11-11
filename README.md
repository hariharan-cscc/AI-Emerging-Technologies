## AI-generated synthetic test data

### (a) How is AI being used to support this technology use-case

Software teams need realistic data to test applications, but copying real customer data into test environments creates privacy and compliance risks. Synthetic data generation uses AI to create fake records that behave like real data without belonging to any real person ([IBM: What is synthetic data?](https://www.ibm.com/think/topics/synthetic-data), [MIT News: Artificial data give the same results as real data](https://news.mit.edu/2017/artificial-data-give-same-results-as-real-data-0303)):

* Generative models learn patterns from real data, like age distributions, policy types, or transaction amounts, and produce new records with the same statistical patterns.
  
  * GANs (generative adversarial networks) train a generator to create fake data and a discriminator to tell real from fake, until the fakes are realistic ([Goodfellow et al., Generative Adversarial Networks, 2014](https://arxiv.org/abs/1406.2661)).
  * Variational autoencoders (VAEs) compress data into a simpler hidden representation and then sample new records from it ([Kingma and Welling, Auto-Encoding Variational Bayes](https://arxiv.org/abs/1312.6114)).
  * CTGAN and TVAE adapt GANs and VAEs specifically for tables with mixed numeric and category columns ([Xu et al., Modeling Tabular Data using Conditional GAN, NeurIPS 2019](https://arxiv.org/pdf/1907.00503), [open-source CTGAN library](https://github.com/sdv-dev/CTGAN)).
  * Large language models can also generate realistic table rows by treating each row as a sentence ([Borisov et al., Language Models are Realistic Tabular Data Generators, ICLR 2023](https://arxiv.org/pdf/2210.06280)).
* AI can keep relationships between tables intact (for example, a customer record that correctly links to their policies and claims), which simple random data generators struggle with.
  
  * MIT's Synthetic Data Vault models an entire relational database, including parent-child links between tables ([Patki et al., The Synthetic Data Vault, MIT](https://dspace.mit.edu/handle/1721.1/109616)).
  * The open-source SDV library's HMASynthesizer learns from multiple connected tables and generates synthetic data that respects those connections ([SDV docs: HMASynthesizer](https://docs.sdv.dev/sdv/multi-table-data/modeling/synthesizers/hmasynthesizer)).
* Teams can generate rare edge cases on demand, like unusual claim types or invalid inputs, to test systems more thoroughly.
  
  * Conditional sampling lets you fix specific values (for example, "claim type = flood" or an extreme amount) while the model fills in the rest realistically. SDV's documentation lists generating hypothetical and extreme scenarios as a main use ([SDV docs: Conditional Sampling](https://docs.sdv.dev/sdv/single-table-data/sampling/conditional-sampling)).
  * SMOTE is a classic technique for creating extra synthetic examples of rare cases ([Chawla et al., SMOTE: Synthetic Minority Over-sampling Technique, JAIR 2002](https://arxiv.org/pdf/1106.1813)).

### (e) Concerns with using AI for this use-case

* If the model memorizes its training data, it might reproduce real customer records, which defeats the privacy purpose.
  
  * Synthetic data does not automatically protect privacy, and unusual records (outliers) are especially exposed ([Stadler et al., Synthetic Data – Anonymisation Groundhog Day, USENIX Security 2022](https://www.usenix.org/system/files/sec22-stadler.pdf)).
  * Membership inference attacks can reveal whether a real person's record was used to train the generator ([van Breugel et al., Membership Inference Attacks against Synthetic Data through Overfitting Detection, 2023](https://arxiv.org/abs/2302.12580), [Zhang et al., Membership Inference Attacks Against Synthetic Health Data](https://pmc.ncbi.nlm.nih.gov/articles/PMC8766950/)).
  * Generative models can memorize and reproduce training examples ([Carlini et al., Extracting Training Data from Diffusion Models, USENIX Security 2023](https://arxiv.org/abs/2301.13188)).
* Synthetic data can look realistic but miss important real-world patterns, which means bugs could slip through testing.
  
  * Strong privacy protection often lowers data quality, and it is hard to prove the data is safe against every attack ([Alan Turing Institute: Synthetic Data Evaluation](https://alan-turing-institute.github.io/tea-techniques/techniques/synthetic-data-evaluation/)).
  * Models trained repeatedly on AI-generated data lose the rare "tail" patterns of the original data ([Shumailov et al., AI models collapse when trained on recursively generated data, Nature 2024](https://www.nature.com/articles/s41586-024-07566-y)).
* If the original data is biased, the synthetic data will copy that bias, and repeated use can make it worse ([Wyllie et al., Fairness Feedback Loops: Training on Synthetic Data Amplifies Bias, FAccT 2024](https://facctconference.org/static/papers24/facct24-144.pdf)).
* Mitigation: measure privacy risk (for example, checking how close synthetic records are to real ones), validate statistical similarity, and still test with masked production-like data when needed.
  
  * Privacy metrics such as distance to closest record (DCR) and membership inference testing are reviewed in [Steier et al., Synthetic Data Privacy Metrics, 2025](https://arxiv.org/html/2501.03941v1).
  * The open-source SDMetrics library compares synthetic data to real data for statistical quality ([SDMetrics on GitHub](https://github.com/sdv-dev/SDMetrics), [SDMetrics Quality docs](https://docs.sdv.dev/sdmetrics/data-metrics/quality)).
  * For a formal privacy guarantee, NIST explains differentially private synthetic data and common pitfalls ([NIST: Differentially Private Synthetic Data](https://www.nist.gov/blogs/cybersecurity-insights/differentially-private-synthetic-data), [NIST SP 800-226](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-226.pdf)).
