# 🌐 Knowledge Graph Analysis: Brand, Product, Industry, and Country Network

## 📌 Overview

This project analyzes the relationships between **brands, products, industries, and countries of origin** using a **Knowledge Graph approach**.

The dataset is built by integrating two Linked Open Data sources:

- Wikidata
- DBpedia

These relationships form a complex global business network that is suitable for graph-based analysis.

---

## 🎯 Objective

The main goal of this project is to:

- Build a unified dataset from multiple knowledge graph sources
- Model relationships between business entities
- Enable network analysis using graph-based techniques (e.g., clustering, centrality)

---

## 📊 Why This Dataset?

This dataset is chosen because:

- It represents real-world business relationships (brand–product–industry–country)
- It has explicit and structured relationships
- It is heterogeneous (multiple entity types in one graph)
- It is meaningful for both technical graph analysis and business interpretation

---

## 🔗 Data Sources

### Wikidata (SPARQL)

```sparql
SELECT DISTINCT
  ?brand ?brandLabel
  ?product ?productLabel
  ?industry ?industryLabel
  ?country ?countryLabel
WHERE {
  ?brand wdt:P31 wd:Q4830453 ;
         wdt:P1056 ?product ;
         wdt:P452 ?industry ;
         wdt:P17 ?country .

  SERVICE wikibase:label {
    bd:serviceParam wikibase:language "en" .
  }
}

### DBpedia (SPARQL)
SELECT DISTINCT
  ?brand ?brandLabel
  ?product ?productLabel
  ?industry ?industryLabel
  ?country ?countryLabel
WHERE {
  ?brand a dbo:Company ;
         rdfs:label ?brandLabel ;
         dbo:product ?product ;
         dbo:locationCountry ?country .

  OPTIONAL {
    ?brand dbo:industry ?industry .
    ?industry rdfs:label ?industryLabel .
    FILTER (lang(?industryLabel) = 'en')
  }

  ?product rdfs:label ?productLabel .
  ?country rdfs:label ?countryLabel .

  FILTER (lang(?brandLabel) = 'en')
  FILTER (lang(?productLabel) = 'en')
  FILTER (lang(?countryLabel) = 'en')
}

## ⚙️ Data Integration Process

The data integration process consists of the following steps:

### 1. Load Dataset
Wikidata and DBpedia CSV files are loaded and only relevant columns are selected.

### 2. Text Normalization
All text values are standardized by removing extra spaces and converting them to lowercase to ensure consistent matching.

### 3. Dataset Merging
Both datasets are combined using an outer join based on `brandLabel` and `productLabel` to ensure no data is lost from either source.

### 4. Fallback Strategy
Missing values in `industryLabel` and `countryLabel` are filled by prioritizing Wikidata, and using DBpedia as a backup source.

### 5. Data Cleaning & Export
Rows with missing key fields are removed, duplicates are dropped, and the final dataset is saved as a CSV file.
```
