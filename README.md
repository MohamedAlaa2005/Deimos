# Deimos — Earth Analog Discovery for the Moon and Mars

**Deimos** is a data-driven planetary analog discovery system that identifies locations on Earth with environmental characteristics similar to selected locations on the **Moon or Mars**.

The project combines scientific datasets from Earth, Moon, and Mars, processes them into a unified spatial representation, and uses a similarity model to return the **Top 5 Earth locations** that most closely resemble a selected extraterrestrial location.

**Live Demo:** https://deimos-nasa.vercel.app/  

---

## 🚀 Project Overview

### The Problem

Researchers studying the Moon and Mars often need Earth locations that can serve as environmental analogs for extraterrestrial environments.

However, planetary datasets are collected from different missions and instruments. They often differ in:

- Data format
- Spatial resolution
- Coordinate systems
- Coverage
- Feature definitions
- Missing values

This makes direct comparison between Earth, Moon, and Mars data difficult.

### The Idea

Deimos creates a common representation of planetary environments by processing heterogeneous scientific datasets into a standardized spatial grid.

Given a location on the **Moon or Mars**, the system compares its environmental features against locations on Earth and retrieves the **Top 5 most similar Earth locations**.

### The Goal

Build a data pipeline and AI system capable of answering:

> **"Which places on Earth most closely resemble this location on the Moon or Mars?"**

---

# 🏗️ System Architecture

```text
                    Scientific Data Sources
                             │
          ┌──────────────────┼──────────────────┐
          │                  │                  │
        Earth              Moon               Mars
          │                  │                  │
          └──────────────────┼──────────────────┘
                             │
                             ▼
                  Data Processing Pipeline
                             │
              ┌──────────────┴──────────────┐
              │                             │
        Data Cleaning                Feature Processing
              │                             │
              └──────────────┬──────────────┘
                             ▼
                  Spatial Standardization
                       1° × 1° Grid
                             │
                             ▼
                 Structured Planetary Data
                    ┌────────┼────────┐
                    │        │        │
                  Earth     Moon     Mars
                    │        │        │
                    └────────┼────────┘
                             ▼
                    Similarity Model
                             │
                             ▼
                  Top 5 Earth Locations
                             │
                             ▼
                      FastAPI Backend
                             │
                             ▼
                         Web App
```

---

# 🧮 Data Engineering Pipeline

One of the main challenges of Deimos was transforming heterogeneous scientific datasets into a representation that could be compared consistently.

## 1. Data Collection

Data was collected from multiple scientific sources covering:

- Earth
- Moon
- Mars

The datasets contain environmental and surface characteristics such as temperature, elevation, reflectance, and other planetary measurements.

### Mars

The Mars data includes:

- MGS TES TSDR
- MGS MOLA MEGDR
- MOLA-Derived
- TES Global Albedo

### Moon

The Moon data includes:

- LRO Diviner Global Cumulative Products (GCP)
- LRO LOLA GDR
- LOLA LDRM_16
- LOLA LDAM_10_FLOAT

### Earth

The Earth data includes:

- MODIS MOD11C3 v061
- MODIS MOD09CMG v061
- SRTMGL30 v002
- EMITL3ASA v002
- MERRA-2

> The project combines multiple scientific sources into a unified planetary representation. The exact number of source products may change as datasets are updated or consolidated.

---

# 🌍 Spatial Standardization

A major challenge was that the datasets did not use the same spatial resolution.

For example, one dataset might contain measurements at a much finer resolution than another.

To make the features comparable, Deimos standardizes the data onto a common:

**1° × 1° spatial grid**

Each observation is mapped to its corresponding latitude/longitude grid cell.

This creates a consistent representation where different environmental features can be associated with the same spatial location.

```text
Original Dataset A       Original Dataset B
High Resolution          Different Resolution
       │                        │
       ▼                        ▼
       └──────────┬─────────────┘
                  │
                  ▼
            Spatial Mapping
                  │
                  ▼
              1° Grid
                  │
                  ▼
        Unified Feature Space
```

This step is important because the similarity model requires comparable feature vectors across locations.

---

# 🗃️ Data Organization

After processing, the data is organized into three main planetary datasets:

```text
Processed Data
│
├── Earth
│   └── Environmental features
│
├── Moon
│   └── Environmental features
│
└── Mars
    └── Environmental features
```

Each spatial cell represents a location and contains the corresponding processed environmental features.

The resulting structure allows the same feature representation to be used when comparing:

```text
Moon Location
      │
      ▼
Feature Vector
      │
      ▼
Similarity Model
      │
      ▼
Earth Feature Vectors
      │
      ▼
Similarity Ranking
      │
      ▼
Top 5 Earth Locations
```

---

# 🤖 Similarity Model

For a selected Moon or Mars location, Deimos extracts its processed feature representation and compares it against Earth locations.

The model produces a similarity score for candidate Earth locations.

The locations are then ranked according to their similarity.

```text
Selected Moon/Mars Location
            │
            ▼
      Extract Features
            │
            ▼
      Create Feature Vector
            │
            ▼
   Compare with Earth Cells
            │
            ▼
      Calculate Similarity
            │
            ▼
       Rank Candidates
            │
            ▼
       ┌─────────────┐
       │ Top 5 Earth │
       │  Locations  │
       └─────────────┘
```

The result allows users to explore Earth locations that have environmental characteristics similar to the selected extraterrestrial location.

---

# 🗄️ Database

The processed data and application information are integrated with a **PostgreSQL database hosted on Neon**.

The database provides persistent storage for the application's structured planetary data and supports backend queries.

---

# ⚡ Backend

The similarity system is exposed through a **FastAPI** backend.

The backend is responsible for:

- Receiving user requests
- Retrieving planetary data
- Processing location information
- Running the similarity model
- Ranking candidate Earth locations
- Returning the Top 5 results

A simplified request flow looks like:

```text
Frontend
   │
   │ API Request
   ▼
FastAPI
   │
   ├── Retrieve Data
   │
   ├── Process Location
   │
   ├── Run Similarity Model
   │
   └── Rank Results
   │
   ▼
Top 5 Earth Analog Locations
```

---

# 🖥️ Web Application

The web interface allows users to:

1. Select a planetary body.
2. Select a location on the Moon or Mars.
3. Submit the location for analysis.
4. Run the similarity search.
5. View the Top 5 Earth analog locations.

**Live Demo:**  
https://deimos-nasa.vercel.app/

---

# 🛠️ Technology Stack

### Data Engineering

- Python
- NumPy
- Pandas
- Geospatial data processing
- Data cleaning
- Feature engineering
- Spatial normalization
- 1° spatial grid standardization

### Machine Learning / AI

- Python
- Similarity-based retrieval
- Feature vectors
- Ranking

### Backend

- FastAPI
- REST API
- PostgreSQL
- Neon

### Frontend / Deployment

- Web application
- Vercel
- FastAPI backend deployment

### Development

- Git
- GitHub
- Linux/Bash

---

# 📂 Project Structure

```text
Deimos/
│
├── data/
│   ├── earth/
│   ├── moon/
│   └── mars/
│
├── notebooks/
│
├── preprocessing/
│   ├── data_cleaning/
│   ├── spatial_processing/
│   └── feature_engineering/
│
├── model/
│   └── similarity/
│
├── backend/
│   └── FastAPI application
│
├── frontend/
│
├── requirements.txt
│
└── README.md
```

> The exact directory structure may differ from the current repository implementation. Update this section to match the final repository structure.

---

# 🔄 End-to-End Pipeline

The complete Deimos workflow is:

```text
1. Collect Scientific Datasets
             ↓
2. Clean and Validate Data
             ↓
3. Process Individual Sources
             ↓
4. Map Data to Geographic Coordinates
             ↓
5. Standardize Spatial Resolution
             ↓
6. Create 1° × 1° Grid
             ↓
7. Build Planetary Feature Tables
             ↓
8. Store Processed Data
             ↓
9. Generate Feature Representations
             ↓
10. Calculate Earth–Moon/Mars Similarity
             ↓
11. Rank Earth Locations
             ↓
12. Return Top 5 Analog Locations
             ↓
13. Display Results in Web Application
```

---

# 🎯 Example Use Case

Suppose a researcher selects a location on Mars.

Deimos processes the selected Martian location into its corresponding environmental feature representation.

The system then searches the Earth dataset for locations with similar characteristics.

The final result might conceptually look like:

```text
Selected Mars Location
        │
        ▼
Environmental Features
        │
        ▼
Similarity Search
        │
        ├── Earth Location #1
        ├── Earth Location #2
        ├── Earth Location #3
        ├── Earth Location #4
        └── Earth Location #5
```

These locations can then be used as potential Earth analog environments for further exploration and analysis.

---

# 🔬 Why Data Engineering Matters

The main challenge of this project was not simply building a similarity model.

The datasets originated from different scientific missions and were not immediately compatible.

The data engineering pipeline was therefore responsible for:

- Integrating heterogeneous scientific sources
- Cleaning and transforming raw measurements
- Handling different spatial resolutions
- Aligning data geographically
- Creating a common spatial representation
- Building structured planetary datasets
- Preparing the final feature space for similarity analysis

This preprocessing layer makes the AI component possible.

