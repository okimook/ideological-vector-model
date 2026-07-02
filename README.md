# Ideological Vector Model (IVM) — Dataset of 42 Political Actors

Data accompanying **"Ideological Vector Model (IVM): A Semantic-Based Approach to Quantitative Political Analysis"** (Kimook, 2026). 
The model represents political figures as vectors in a 9-dimensional ideological space and measures proximity between them with a normalized Euclidean distance — the Semantic Distance Score (SDS).

This repository holds the two source files behind every number in the paper: the raw ideological coordinates, and the pairwise distance matrix computed from them.

## Files

| File | Shape | Content |
|---|---|---|
| `Dataset_42_political_actors_-_vectors.csv` | 42 rows × 9 columns | Each actor's raw score on each of the 9 IVM axes |
| `Dataset_42_political_actors_-_gaps.csv` | 42 × 42 | Pairwise SDS (ideological dissimilarity, 0–100%) between every actor |

Both files use `;` as the delimiter. `gaps.csv` uses a comma as the decimal separator and stores values as percentage strings (e.g. `13,7%`). If you're loading it with pandas:

```python
import pandas as pd

vectors = pd.read_csv("Dataset_42_political_actors_-_vectors.csv", sep=";")
gaps = pd.read_csv("Dataset_42_political_actors_-_gaps.csv", sep=";", index_col=0)
gaps = gaps.drop(columns="Actors").apply(
    lambda col: col.str.replace("%", "").str.replace(",", ".").astype(float)
)
```

Row order is identical across both files — row *i* in `vectors.csv` corresponds to row *i* and column *i* in `gaps.csv`.

## Distance metric

`gaps.csv` is generated from `vectors.csv` with the normalized Euclidean distance defined in Section 3.1 of the paper:

```
SDS_ij = sqrt( Σ_{k=1..9} (X_ik − X_jk)² ) / sqrt(9 · 200²) × 100%
```

0% means two actors' vectors coincide; 100% means they're diametrically opposed on every axis. This has been verified computationally: recalculating the full 42×42 matrix directly from `vectors.csv` reproduces every value in `gaps.csv` to within ±0.05 percentage points (rounding-level agreement) across all 861 unique pairs.


## Actors

Adolf Hitler, Alexei Navalny, Angela Merkel, Barack Obama, Benito Mussolini, Benjamin Netanyahu, Bernie Sanders, Boris Johnson, Boris Yeltsin, Charles de Gaulle, Claudia Sheinbaum, Deng Xiaoping, Donald Trump, Emmanuel Macron, Franklin D. Roosevelt, Friedrich Merz, George W. Bush, Giorgia Meloni, Jair Bolsonaro, Joe Biden, John F. Kennedy, Joseph Stalin, Justin Trudeau, Keir Starmer, Lula da Silva, Mahatma Gandhi, Margaret Thatcher, Mark Carney, Martin Luther King Jr., Narendra Modi, Nelson Mandela, Prabowo Subianto, Recep Tayyip Erdogan, Richard Nixon, Ronald Reagan, Shehbaz Sharif, Shigeru Ishiba, Viktor Orban, Vladimir Putin, Volodymyr Zelenskyy, Winston Churchill, Xi Jinping.

Axis scores were assigned through manual expert evaluation (see Section 2.3 of the paper) and should be read as a proof-of-concept dataset, not a validated measurement instrument.

## Citation

If you use this dataset, please cite:

> Kimook, O. (2026). *Ideological Vector Model (IVM): A Semantic-Based Approach to Quantitative Political Analysis.*

### License

Released under [CC BY-NC-ND 4.0](LICENSE): you are free to read, save, and share the article datasets unchanged and non-commercially, with attribution.

### On authorship

"Olesya Kimook" is a pen name. The author has deliberately chosen anonymity for personal reasons and does not disclose their real identity.
All communication regarding the IVM article — press, questions, proposals — goes through a single channel:

**olesya.kimook@pm.me**
