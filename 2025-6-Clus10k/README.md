# Extrating  Economic Sector in  Corporate Business Profiles Extracted from Form 10-K Reports

> **Goal**: Cluster firms by industry using SEC data.

> **Why?**: To help analyze companies by comparing them to their peers.

## Methods

### Step 1: **Data Collection**

* **Download 10-K Filings**: Obtain the latest 10-K filings from the SEC’s [EDGAR database](https://www.sec.gov/edgar/searchedgar/companysearch).
* **Extract Item 1 – Business Section**: This section describes the company’s core activities, products, markets, and strategy. Use tools like Python’s `sec-edgar-downloader` or custom scrapers to extract text.

### Step 2: **Preprocessing Text**

* **Clean Text**: Remove HTML tags, punctuation, special characters, and normalize whitespace.
* **Lowercase**: Convert all text to lowercase.
* **Stopwords Removal**: Remove common English stopwords (e.g., “the”, “and”, “of”).
* **Tokenization**: Split text into words or meaningful n-grams.
* **Stemming/Lemmatization**: Reduce words to their root forms (optional).

### Step 3: **Feature Extraction**

* **TF-IDF Vectorization**: Convert text into numerical vectors capturing term frequency and inverse document frequency — emphasizes important words.
* **Word Embeddings**: Use pretrained models (e.g., Word2Vec, GloVe, or transformers like BERT) to create dense vector representations that capture semantic meaning.
* **Domain Keywords**: Optionally, use domain-specific dictionaries or extract key phrases (e.g., “software”, “cloud services”, “pharmaceuticals”).

### Step 4: **Dimensionality Reduction**

* **Techniques**: Use PCA, t-SNE, or UMAP to reduce vector dimensions while preserving meaningful structure.
* Helps to visualize and improves clustering performance.

### Step 5: **Clustering**

* **Choose a clustering algorithm** based on your data and goals:

  * **K-Means**: Good for well-separated spherical clusters.
  * **Hierarchical Clustering**: Builds dendrograms; good for nested clusters.
  * **DBSCAN**: Density-based; identifies noise and clusters of arbitrary shape.
  * **Gaussian Mixture Models (GMM)**: For probabilistic clustering.
* Select the number of clusters (`k`) based on:

  * Domain knowledge
  * Elbow method or silhouette scores for internal validation

## NER

> Does not work for "Asana is a work management platform that helps organizations orchestrate work, from daily tasks to cross-functional strategic initiatives. Over 139,000 paying customers use Asana to manage everything from product launches to marketing campaigns to organization-wide goal setting. Our platform adds structure to unstructured work, creating clarity, transparency, and accountability to everyone within an organization—individuals, team leads, and executives—so they understand exactly who is doing what, by when."; in order to get the economic sectors.

## Used LLM's

> ...

## Word Embeding of the Enteirse 10-k Form Document -> Industry Classification

Extracting the economic sector from a 10-K form (or similar business description text) can definitely be framed as a **prediction** or **sequence tagging** problem in NLP. Here’s a more detailed breakdown of how this can work:

* **Input:**
  Raw text (e.g., business description section of a 10-K filing).

* **Goal:**
  Predict a label or tag corresponding to the economic sector of the company.

### Approach Using Word Embeddings + Prediction Model

1. **Preprocessing & Tokenization:**
   Break the text into tokens (words, subwords, or sentences).

2. **Word Embeddings / Feature Extraction:**
   Convert tokens into dense vector representations using techniques like:

   * Pretrained embeddings (e.g., GloVe, FastText)
   * Contextual embeddings (e.g., BERT, RoBERTa, GPT)
     These embeddings capture semantic information from the text.

3. **Model Architecture:**
   Several architectures are possible:

   * **Sequence classification model:**
     Feed the embedding sequence into a model (e.g., LSTM, Transformer) and generate a single label representing the sector.
   * **Sequence tagging / NER model:**
     Label each token or phrase with tags (like BIO tagging) to extract the relevant sector phrase.
   * **Text classification models:**
     Use the whole text embedding pooled (CLS token in BERT) to classify into a predefined sector category.

4. **Output:**

   * The predicted economic sector (e.g., "Technology", "Healthcare", "Financial Services", etc.)
   * Or extracted phrase(s) representing the sector

### Why This Works Well

* **Semantic understanding:**
  Word embeddings capture relationships and meanings beyond surface words.

* **Context-aware:**
  Modern contextual embeddings adjust meaning based on surrounding text.

* **Supervised learning:**
  If you have labeled examples (text + sector), the model can learn to associate patterns of language with economic sectors.

### Practical Notes

* You’ll need a labeled dataset of 10-K descriptions with sectors for training.
* Sectors can come from standard taxonomies like NAICS or GICS.
* The model can be fine-tuned on financial domain text for better performance.
* Ambiguity and overlap between sectors may require hierarchical or multi-label classification approaches.

## Topic Modelling

> ...

- Use topic modeling for exploratory analysis and feature extraction to discover thematic structures.

- Then feed topic distributions as features into supervised classifiers (e.g., neural nets, transformers) trained on labeled sector data.

- Alternatively, use topic modeling to cluster companies initially and then manually assign sector labels to clusters, creating a semi-supervised approach.

Pros:
- Unsupervised Approach: Topic models (like LDA) do not require labeled data, so you can discover latent topics representing sectors without pre-tagged examples.

- Insightful Themes: They can reveal underlying thematic clusters of words that correspond to industry sectors or sub-sectors.

- Interpretability:  Topics are distributions over words, which can be inspected to understand the semantic grouping.

- Scalable:  Easily applied to large corpora of filings.

Cons / Limitations:

- Granularity & Ambiguity: Topics may be too broad or too mixed to cleanly represent a specific economic sector, especially if company descriptions contain multiple lines of business.

- No Direct Labeling: Topics themselves are unlabeled clusters of words; mapping them to standard sector codes (NAICS, GICS) requires manual interpretation or additional classification.

- Sparse & Noisy Text: 10-K filings are long and verbose, with boilerplate language that can dilute sector-specific terms, reducing topic model effectiveness.

- Less Precision Than Supervised Models: If labeled data is available, supervised classifiers using embeddings (BERT/finBERT) usually outperform topic models in accuracy.

## **Evaluation & Interpretation**

* **Validate Clusters**:

  * Compare with known industry codes (NAICS, GICS) if available.
  * Check cluster cohesion (within-cluster similarity) and separation (between-cluster differences).
* **Label Clusters**:

  * Identify top keywords or representative companies per cluster.
  * Assign industry/sector labels where possible.

## Tools & Libraries

* **Python**: `requests`, `beautifulsoup4` (web scraping); `nltk`, `spacy` (NLP); `scikit-learn` (vectorization, clustering); `transformers` (BERT embeddings)

* **Visualization**: `matplotlib`, `seaborn`, `plotly`

* Embedding: https://sbert.net/

## Case Study

Industry:

* Project Management & Collaboration Software.

Firms:

* monday.com
* Asana Inc
* Airtable
* ClickUp
* Smartsheet Inc
* Notion Labs Inc

## Data Sources

- [SEC's Electronic Data Gathering, Analysis, and Retrieval (EDGAR) System](https://www.sec.gov/search-filings)

- [Welcome to the Hoberg-Phillips Data Library](https://hobergphillips.tuck.dartmouth.edu/tnic_basedata.html)

## Previous Works

- [Text-based Industry Classification](https://github.com/Sivalavida/Text-based-Industry-Classification)

- [Global text-based industry classification](https://papers.ssrn.com/sol3/papers.cfm?abstract_id=4985855)

- [Company classification using zero-shot learning](https://arxiv.org/abs/2305.01028)

- [Text-Based Industry Classification Based on Chinese A Share Annual Reports](https://ieeexplore.ieee.org/document/9696061)

- [Using text data instead of SIC codes to tag innovative firms and classify industrial activities](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0270041)

- [Emerging industry classification based on BERT model](https://www.sciencedirect.com/science/article/pii/S030643792400142X)

- [A Multimodal Embedding-Based Approach to Industry Classification in Financial Markets](https://arxiv.org/abs/2211.06378)

- [Text-based Industry Reclassification and Industry Momentum](https://mrkaiwu.github.io/ind_momentum.html)

## References

- [finBERT](https://github.com/ProsusAI/finBERT)
