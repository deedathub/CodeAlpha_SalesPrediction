# Sales Prediction using Python

CodeAlpha Data Science Internship, Task 4.

A regression model predicting sales from advertising spend across three channels,
with a check on how much each channel actually contributes.

## Data

`Advertising.csv`, supplied by CodeAlpha. 200 rows, 5 columns. Each row records
spend on television, radio and newspaper advertising against the resulting sales.
The file states no units and no currency, and contains no date column.

## What the notebook does

1. Loads the raw file without modification
2. Inspects it and records what needs fixing
3. Drops a leftover row counter, the only cleaning required
4. Explores how sales relates to spend in each channel, and tests two apparent
   relationships rather than reporting them at face value
5. Trains a linear regression and a random forest, cross-validates both, and
   measures what each channel contributes
6. States the limitations

## Findings

**Television shows diminishing returns.** Splitting spend into four equal bands
of 50 rows, mean sales run 8.39, 12.69, 16.55 and 18.47. The increase between
successive bands falls from 4.30 to 3.86 to 1.92, so the highest band of spend
buys less than half what the lowest one did.

**Newspaper spend does not predict sales.** It correlates with sales at 0.228,
which looks weak but not absent. It also correlates with radio spend at 0.35, so
campaigns buying newspaper tend to buy radio as well. Splitting the data at the
median radio spend and recomputing within each half drops the newspaper
correlation to 0.056 and 0.111. The model reaches the same conclusion
independently: feature importance puts newspaper at 0.01, and removing the column
entirely leaves the score unchanged at 0.983 against 0.980. A feature carrying
real signal cannot improve a model by being removed.

**The random forest beats linear regression, for a reason the data explains.**
Cross-validated across five shuffled folds, the forest scores a mean R squared of
0.980 with a spread of 0.005. Linear regression scores 0.883 with a spread of
0.040. Linear regression fits one constant rate of return across the whole range
of television spend, so it overstates the return at high spend where the effect
has already flattened. The forest follows the shape instead of imposing one.

**Channel contribution.** Television 0.62, radio 0.36, newspaper 0.01.

## Limitations

The relationship in this data is unusually clean. An R squared of 0.980 with a
spread of 0.005 is far tighter than real advertising data produces, where
seasonality, pricing, competitor activity and brand effects all move sales
independently of spend. This is a long-standing teaching dataset and the result
should not be read as evidence that sales are this predictable in practice.

No units, currency, date or period are given. Without a time dimension, every row
is treated as independent, so any carry-over from one period into the next is
invisible. The task offered time series models as an option and this file cannot
support one. It also mentions target segment and platform, neither of which
exists in the data.

Nothing here establishes cause. The model shows spend and sales moving together,
not that spending more would produce more sales. That applies most to the
diminishing returns finding, which describes the range of spend present in the
data and says nothing about what happens outside it.

The newspaper result is a finding about this dataset, not about newspaper
advertising generally.

## Running it

Requires Python 3 with pandas, numpy, matplotlib, seaborn and scikit-learn. Open
`CodeAlpha_SalesPrediction.ipynb` in Jupyter and run all cells in order.

## Files

- `CodeAlpha_SalesPrediction.ipynb` - the analysis
- `Data/Advertising.csv` - the dataset
- `README.md` - this file

## Author

Ahmed Yunus Albarka
