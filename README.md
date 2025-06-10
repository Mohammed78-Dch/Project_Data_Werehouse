# Data Warehouse Project: Analyzing Customer Reviews of Bank Agencies in Morocco

## 📌 Title
**Analyzing Customer Reviews of Bank Agencies in Morocco using a Modern Data Stack**

## 👨‍💻 Author
**Mohammed Dechraoui**  
Master 2 – Systèmes d'Information et Systèmes Intelligents (M2SI)  
Institut National de Statistique et d'Économie Appliquée (INSEA)  
📧 mdechraoui@insea.ac.ma 
📅 Academic Year: 2024–2025  

---

## 📝 Introduction

This project focuses on building a comprehensive Data Warehouse to analyze customer reviews of bank agencies in Morocco. It leverages a modern data stack to extract, transform, model, and visualize customer feedback, providing valuable insights for the banking sector. This `README.md` provides an overview of the project's architecture, implementation details, and key features.

---

## ✅ Project Contributions

This project encompasses a comprehensive data pipeline, with all components meticulously implemented, tested, and documented. The following sections outline the key phases and contributions:

### 🔹 Phase 1 – Data Collection

- Implemented a Python-based web scraper using **Scrapy** to extract: bank name, branch location, review text, rating, and date of review.
- Used **Google Maps search patterns** to locate bank branches in Morocco.
- Exported data to structured **JSON** and imported it into PostgreSQL.
- Designed and scheduled an **Apache Airflow DAG** to automate weekly extraction.

### 🔹 Phase 2 – Data Cleaning & Transformation

- Used **DBT (Data Build Tool)** to:
  - Clean raw review data (remove duplicates, missing values).
  - Normalize and preprocess text (lowercasing, stop words removal).
- Developed Python scripts to:
  - Detect the language of reviews.
  - Apply **sentiment analysis** using TextBlob.
  - Extract common themes using **LDA topic modeling**.
- Logged transformations and documented DBT models.

### 🔹 Phase 3 – Data Modeling (Star Schema)

- Designed and created the following tables in **PostgreSQL**:
  - `fact_reviews`
  - `dim_bank`
  - `dim_branch`
  - `dim_location`
  - `dim_sentiment`
- Used DBT to create SQL models and load data incrementally.
- Ensured referential integrity and optimized queries for BI tools.

### 🔹 Phase 4 – BI & Analytics

- Built a **Looker Studio (Data Studio)** dashboard:
  - Trends of customer sentiment per bank and branch.
  - Top positive/negative themes extracted from reviews.
  - Ranking of bank branches by average sentiment.
  - Interactive filters for location, bank, and date range.

### 🔹 Phase 5 – Deployment & Automation

- Integrated all steps in a single Apache Airflow workflow:
  - Daily DAG: fetch new reviews, update DBT models, refresh schema.
  - Included email alerts for DAG failures.

---

## 📁 Repository Structure

```
📁 project-data-warehouse/
├── airflow/
│   ├── dags/
│   │   └── dag_load_reviews.py
│   └── scripts/
│       ├── connect_to_db.py
│       ├── scraper_banks.py
│       ├── scraper.py
│       ├── insert_data_to_json.py
│       ├── insert_data_to_postgresql.py
│       ├── convertir_data_relative.py
│       ├── detect_language.py
│       ├── topic_modeling.py
│       ├── sentiment_analysis.py
│       ├── banks_maroc.json 
│       └── avis.json
│
├── .dbt/
│   └── dbt_projects/
│        ├── bank_reviews_decisionnal/
│        └── bank_reviews_transactional/
│
├── requirements.txt
│
└── README.md
```

---

## 📷 Project Visuals

### 📌 Architecture Diagram

![Architecture Diagram](./assets/architecture_diagram.png)

> This diagram illustrates the complete pipeline from scraping to dashboarding, including Airflow, PostgreSQL, DBT, and Looker Studio.

### 📊 Dashboard (Looker Studio)

![Dashboard Screenshot](./assets/dashboard_looker_studio.png)

> Interactive dashboard presenting sentiment trends, top reviewed branches, frequent topics, and customer satisfaction distribution.

---

## 🛠️ Technologies & Dependencies

The project leverages a diverse set of technologies and libraries across different stages of the data pipeline:

### 🔍 Web Scraping
- `selenium`: For browser automation and web interaction.
- `webdriver-manager`: To manage WebDriver binaries for Selenium.
- `beautifulsoup4`: For parsing HTML and XML documents.

### 🗂️ JSON & Utilities (built-in modules)
- `json`: For working with JSON data.
- `os`: For interacting with the operating system.
- `time`: For time-related functions.
- `random`: For generating random numbers.
- `re`: For regular expressions.
- `urllib.parse`: For parsing URLs.
- `logging`: For logging events and debugging.

### 🧠 NLP & Text Mining
- `langdetect`: For language detection of text.
- `transformers`: For advanced natural language processing tasks, potentially for sentiment analysis.
- `spacy`: For advanced NLP, including tokenization, named entity recognition, and part-of-speech tagging.
- `nltk`: The Natural Language Toolkit, used for various NLP tasks, including stopwords removal.
- `gensim`: For topic modeling, specifically LDA (Latent Dirichlet Allocation).
- `pandas`: For data manipulation and analysis.

### 📦 Database
- `psycopg2-binary`: A PostgreSQL adapter for Python, enabling interaction with the PostgreSQL database.

### ⚙️ Workflow Orchestration
- **Apache Airflow**: A platform to programmatically author, schedule, and monitor workflows (DAGs).

### 🧱 Data Modeling
- **DBT (Data Build Tool)**: Used for transforming data in the warehouse, with separate projects for transactional and decisional data.

### 📊 Visualization
- **Looker Studio (Google Data Studio)**: A free, web-based tool for creating interactive data dashboards and reports, connected to PostgreSQL for data visualization.

---

## 📥 Required Downloads (before execution)

```bash
# spaCy language models
python -m spacy download en_core_web_sm
python -m spacy download fr_core_news_sm
python -m spacy download es_core_news_sm
python -m spacy download de_core_news_sm

# NLTK stopwords
python -c "import nltk; nltk.download(\'stopwords\')"

```

---

## 🔄 Data Pipeline Flow

The data pipeline is designed as a continuous flow, ensuring data is processed from raw collection to insightful visualization:

### 🏦 [Scraping]
Customer reviews and bank information are collected using `scraper_banks.py` and `scraper.py` scripts, which are part of the web scraping module.

### 💾 [Storage]
Raw data, initially in JSON format, is then loaded into the PostgreSQL database for persistent storage and further processing.

### 🧹 [Preprocessing]
This stage involves several critical steps:
- **Relative Date Conversion**: Dates in the reviews are converted to a standardized format.
- **Language Detection**: The language of each review is automatically detected.
- **Sentiment Analysis**: Advanced Transformer models are applied to perform sentiment analysis on the reviews, classifying them as positive, negative, or neutral.
- **Topic Modeling**: Latent Dirichlet Allocation (LDA) is used to extract dominant themes and topics from the customer reviews.

### 🤖 [Automation]
All the aforementioned steps are orchestrated and automated through the Apache Airflow DAG: `dag_load_reviews.py`, ensuring a seamless and scheduled execution of the entire pipeline.

### 🧱 [Modeling]
Transformed and preprocessed data is then modeled using DBT, creating well-defined fact and dimension tables in the data warehouse:
- `fact_reviews`
- `dim_bank`
- `dim_branch`
- `dim_location`
- `dim_sentiment`

This star schema design optimizes data retrieval for analytical queries.

### 📊 [Visualization]
The final stage involves connecting the PostgreSQL database to **Looker Studio** to create interactive dashboards, providing a visual representation of the analyzed customer review data.

---

## 🔍 Example Use Cases

The insights derived from this data warehouse project can be applied to various business scenarios within the banking sector:

- **Detection of Underperforming Agencies**: Identify bank branches that consistently receive negative customer feedback, allowing for targeted interventions and improvements.
- **Identification of Recurring Complaints**: Pinpoint common issues reported by customers (e.g., long waiting times, ATM malfunctions), enabling banks to address systemic problems.
- **Understanding Regional Differences in Reviews**: Analyze customer sentiment and feedback patterns across different geographical regions to tailor services and strategies.
- **Benchmarking Performance Across Banks**: Compare the performance and customer satisfaction levels of different banks based on their customer reviews.

---

## 🧠 Key Learning Outcomes

This project provided valuable learning experiences and reinforced key skills in data engineering and analytics:

- **Robust Scraping Systems**: Gained expertise in building resilient and modular web scraping systems, including error handling mechanisms.
- **NLP Application**: Applied Natural Language Processing techniques to real-world customer data, extracting meaningful insights from unstructured text.
- **Star Schema and Dimensional Modeling**: Developed a strong understanding of designing and implementing star schemas and dimensional models for efficient data warehousing.
- **Airflow and DBT for Production Pipelines**: Acquired practical experience in using Apache Airflow for workflow orchestration and DBT for data transformation in production environments.
- **Effective Communication through Dashboards**: Learned to communicate complex analytical results clearly and interactively through well-designed dashboards.

---

## 📌 Remarks

This project serves as a practical simulation of a real-world data engineering and analytics challenge within the banking sector. It demonstrates how customer voice data can be leveraged to drive actionable insights and improve business outcomes. All scripts, data models, dashboards, and datasets developed for this project are included in the submission and are available in the associated GitHub repository.

## 👨‍🏫 Supervision

Professor: Dr. BENELALLAM Imade
Course: Data Warehousing and Business Intelligence
Institution: INSEA

