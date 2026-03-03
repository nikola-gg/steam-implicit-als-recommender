# Steam Recommender System (Implicit ALS)

This project builds a collaborative filtering recommender system 
based on implicit user–item interactions from the Steam dataset.

The model is trained using **Alternating Least Squares (ALS)** 
and evaluated against a **global popularity baseline** using Precision@K and Recall@K.

---

## Dataset

https://www.kaggle.com/datasets/tamber/steam-video-games

The dataset contains 200,000 user–game interaction records 
with purchase and playtime events.

Each row represents:
- `user_id`
- `game`
- `behavior` (purchase / play)
- `value` (playtime in hours or 1.0 for purchase)

This is an **implicit feedback dataset** - user preference is inferred 
from behavior (playtime, purchases), not explicit ratings.

---

## Methodology

### 1. Data preprocessing
- Converted raw events into a unified implicit preference score
- Combined purchase signal and log-scaled playtime
- Aggregated to one row per (user, game)

### 2. Filtering
- Removed sparse users and games
- Kept users with ≥ 3 games
- Kept games with ≥ 5 users

### 3. Train/Test split
- User-based split (80/20)
- Ensured no data leakage
- Each user kept at least one training interaction

### 4. Baseline model
- Global popularity ranking
- Used as reference and cold-start fallback

### 5. ALS Model
- Trained an Implicit Alternating Least Squares (ALS) model
- Treated stronger interactions (e.g., long playtime) as stronger signals
- Learned hidden representations of users and games
- Generated personalized recommendations based on similarity in learned patterns

### 6. Evaluation
- Precision@10
- Recall@10
- Case-study analysis for individual users
- Comparison with popularity baseline

---

## Results

ALS significantly outperforms the popularity baseline:

- Precision@10 improved from **0.0377 → 0.0897**
- Recall@10 improved from **0.1704 → 0.4269**
- Average hits in Top-10 more than doubled

This demonstrates that collaborative filtering captures 
personalized preferences better than global popularity ranking.

---

## How to Run

1. Create and activate a virtual environment:

```bash
python -m venv .venv
```

Windows:
```bash
.\.venv\Scripts\activate
```

macOS / Linux:
```bash
source .venv/bin/activate
```

2. Install dependencies:

```bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

3. Download the dataset from Kaggle and place it in:

```
data/steam-200k.csv
```

4. Run the notebook:

```bash
jupyter notebook notebooks/recommender_system_als.ipynb
```

Then select:

```
Kernel → Restart & Run All
```

to reproduce the full analysis.