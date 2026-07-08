# Quantium Retail Analytics: Category Review & Trial Store Uplift Testing

This project started as a presentation I completed through Quantium's Data Analytics Job Simulation on Forage, where I analyzed chip category performance and evaluated a store layout trial using control store matching and statistical testing. I later rebuilt the entire analysis in Python using the same source data, both to make it fully reproducible and to independently verify the original findings.

## The Questions I Set Out to Answer

1. What does the chip category look like overall: sales trends, customer segments, and brand performance?
2. Which stores make the best control comparisons for each trial store?
3. Did the trial store layout actually produce a statistically significant increase in sales and customers?

## What I Found

**Category overview:** Kettle is the clear leading brand by revenue (21.7%, closely matching my original analysis), with Doritos and Smiths much closer to each other in market share than I originally reported. Reproducing the segment analysis from scratch also led me to a genuine correction: Budget Older Families, not Mainstream Retirees, are actually the highest-spending segment in the data. I also found that the "~30% December spike" I originally reported doesn't hold up under recalculation; December sales were only about 4% above the monthly average, with no other month showing a dramatic seasonal swing either.

**Control store selection:** I built a similarity-scoring function from scratch, combining Pearson correlation and magnitude distance across both sales and customer-count patterns, to identify the best-matched control store for each trial store. This successfully and independently reproduced all three original control store selections: Store 233 for Trial Store 77, Store 155 for Trial Store 86, and Store 237 for Trial Store 88.

**Uplift testing:** Using a t-test framework built around each store's pre-trial variability, I confirmed the original analysis's core conclusions. Trial Stores 77 and 88 both showed statistically significant sales and customer growth in most trial months. Trial Store 86 showed a more mixed pattern, with significant customer growth that wasn't matched by significant sales growth, supporting the original recommendation to investigate pricing at that location before a full rollout.

## A Data Quality Issue Worth Mentioning

While reproducing the control store selection, my first attempt for Trial Store 88 didn't match the original report at all; my algorithm selected a different store entirely. Looking back at the original methodology, I realized I had only been scoring similarity based on sales patterns, when the original approach combined both sales and customer-count patterns into one score. Once I corrected this and recalculated using both metrics, Store 237 came out as the correct top match, exactly as in the original analysis. This turned out to be a good example of how skipping even one piece of a stated methodology can lead to a meaningfully different result.

I also found a single loyalty card with two transactions of 200 units each, more than 100 times the size of a typical purchase, which I excluded as a likely bulk or commercial buyer rather than a representative household shopper.

## Tools & Skills Demonstrated

- **Python:** pandas for data loading, cleaning, and merging; matplotlib for visualization
- **Statistical analysis:** correlation-based similarity scoring, t-tests for measuring statistical significance
- **Custom algorithm design:** building a control-store matching function from a written methodology description, without a reference implementation
- **Data quality investigation:** identifying inconsistent brand naming, excluding an outlier bulk purchase, and diagnosing a gap in my own analytical methodology
- **Independent verification:** cross-checking an existing analysis against freshly reproduced results, and documenting discrepancies transparently rather than forcing a match

## Project Structure

```
quantium-retail-analytics/
├── data/               # QVI transaction and customer datasets
├── notebooks/          # Jupyter notebook with the full analysis
├── images/             # Saved chart visualizations
└── README.md
```

## Data Source

Transaction and customer behavior data provided through Quantium's Data Analytics Job Simulation on [Forage](https://www.theforage.com/), covering July 2018 to June 2019.

## What I'd Do Next

I'd like to extend the uplift testing to include a post-rollout monitoring period, using the same control-store methodology, to confirm the sales uplift is sustained over time rather than a short-term novelty effect.
