# Blinkit Sales Analytics Dashboard

A Power BI project where I dig into Blinkit grocery sales data to see what sells, where it sells, and which kinds of outlets bring in the most revenue. The dashboard covers 8,523 items across 10 outlets and about $1.2M in total sales.

![Blinkit Dashboard](images/dashboard.png)

## What I wanted to find out

I started with a handful of simple questions a business team would actually ask:

- How much are we selling, and what does an average item bring in?
- Do Low Fat or Regular products do better?
- Which product categories carry the business?
- Does outlet size, location tier or outlet type make a real difference?
- Does putting an item in a more visible spot actually lead to more sales?

## Tools used

- Power BI Desktop for the dashboard
- Power Query for cleaning the data
- DAX for the measures
- The source data came in as a flat file (CSV/Excel)

## The dataset

Everything sits in one table called `BlinkIT Grocery Data`. Each row is one item sold at one outlet.

| Column | What it holds |
|--------|---------------|
| Item Identifier | Unique product ID |
| Item Type | Product category (Dairy, Snack Foods, Household, etc.) |
| Item Fat Content | Low Fat or Regular |
| Item Weight | Weight of the product |
| Item Visibility | Share of display space given to the item in the outlet |
| Rating | Customer rating |
| Sales | Sales amount |
| Outlet Identifier | Unique outlet ID |
| Outlet Establishment Year | Year the outlet opened |
| Outlet Size | Small, Medium or High |
| Outlet Location Type | Tier 1, Tier 2 or Tier 3 |
| Outlet Type | Grocery Store or Supermarket Type1, Type2, Type3 |

There are only 10 outlets in the data, which turned out to matter a lot when reading the results (more on that below):

| Outlet | Opened | Tier | Type |
|--------|--------|------|------|
| OUT010 | 2011 | Tier 3 | Grocery Store |
| OUT049 | 2012 | Tier 1 | Supermarket Type1 |
| OUT013 | 2014 | Tier 3 | Supermarket Type1 |
| OUT045 | 2015 | Tier 2 | Supermarket Type1 |
| OUT046 | 2016 | Tier 1 | Supermarket Type1 |
| OUT035 | 2017 | Tier 2 | Supermarket Type1 |
| OUT019 | 2018 | Tier 1 | Grocery Store |
| OUT027 | 2018 | Tier 3 | Supermarket Type3 |
| OUT017 | 2020 | Tier 2 | Supermarket Type1 |
| OUT018 | 2022 | Tier 3 | Supermarket Type2 |

One small thing to know: Item Weight is empty for the two outlets that opened in 2018. I don't use weight in any KPI, so I left it alone.

## Cleaning the data

All of this was done in Power Query. The applied steps, in order:

1. Source
2. Navigation
3. Promoted Headers
4. Changed Type
5. Replaced Value: `LF` became `Low Fat`
6. Replaced Value1: `low fat` became `Low Fat`
7. Replaced Value2: `reg` became `Regular`
8. Filtered Rows
9. Removed Duplicates
10. Removed Blank Rows
11. Removed Errors

The three replace steps were the important ones. The fat content column had the same value written in different ways, and without fixing that the charts would have shown extra categories. After cleaning, it only has two values: Low Fat and Regular.

## Data model and measures

Besides the main table, there is a small `Metrics` table that works as a field parameter. It powers the buttons at the top of the left panel (Total Sales, Average, No of Items, Average Rating), so one chart can switch between metrics.

The four measures are basic:

```DAX
Total Sales    = SUM('BlinkIT Grocery Data'[Sales])
Average        = AVERAGE('BlinkIT Grocery Data'[Sales])
No of Items    = COUNT('BlinkIT Grocery Data'[Item Identifier])
Average Rating = AVERAGE('BlinkIT Grocery Data'[Rating])
```

## Headline numbers

| KPI | Value |
|-----|-------|
| Total Sales | $1.2M |
| Average Sales per Item | $140.99 |
| Number of Items | 8,523 |
| Average Rating | 3.92 |

## Exploring the data

### Fat content

| Fat Content | Sales | Share |
|-------------|-------|-------|
| Low Fat | $776.3K | 64.6% |
| Regular | $425.4K | 35.4% |

Low Fat leads in every location tier:

| Tier | Low Fat | Regular |
|------|---------|---------|
| Tier 3 | $0.31M | $0.17M |
| Tier 2 | $0.25M | $0.14M |
| Tier 1 | $0.22M | $0.12M |

### Item type

| Item Type | Sales |
|-----------|-------|
| Fruits and Vegetables | $0.18M |
| Snack Foods | $0.18M |
| Household | $0.14M |
| Frozen Foods | $0.12M |
| Dairy | $0.10M |
| Canned | $0.09M |
| Baking Goods | $0.08M |
| Health and Hygiene | $0.07M |
| Meat | $0.06M |
| Soft Drinks | $0.06M |
| Breads | $0.04M |
| Hard Drinks | $0.03M |
| Others | $0.02M |
| Starchy Foods | $0.02M |
| Breakfast | $0.02M |
| Seafood | $0.01M |

(These come from the bar chart, so they are rounded to the nearest $0.01M.)

### Outlet size

| Size | Sales | Share |
|------|-------|-------|
| Medium | $507.9K | 42.27% |
| Small | $444.8K | 37.01% |
| High | $249.0K | 20.72% |

### Location tier

| Tier | Sales |
|------|-------|
| Tier 3 | $472.1K |
| Tier 2 | $393.2K |
| Tier 1 | $336.4K |

### Outlet type

| Outlet Type | Sales | Items | Avg Sales | Avg Rating | Avg Visibility |
|-------------|-------|-------|-----------|------------|----------------|
| Supermarket Type1 | $787,549.9 | 5,577 | $141.21 | 3.92 | 0.06 |
| Grocery Store | $151,939.1 | 1,083 | $140.29 | 3.93 | 0.10 |
| Supermarket Type2 | $131,477.8 | 928 | $141.68 | 3.93 | 0.06 |
| Supermarket Type3 | $130,714.7 | 935 | $139.80 | 3.91 | 0.06 |

### Establishment year

| Year | Sales |
|------|-------|
| 2011 | $78K |
| 2012 | $130K |
| 2014 | $132K |
| 2015 | $131K |
| 2016 | $132K |
| 2017 | $133K |
| 2018 | $205K |
| 2020 | $129K |
| 2022 | $131K |

## Questions and answers

**How is the business doing overall?**
About $1.2M in sales from 8,523 items, so roughly $141 per item, with an average rating of 3.92.

**Do Low Fat or Regular products sell more?**
Low Fat, and by a clear margin: $776.3K against $425.4K, or about 65% of sales. It also wins in all three location tiers, so this isn't a one-region effect.

**Which categories bring in the most money?**
Fruits and Vegetables and Snack Foods, at about $0.18M each. Together they make up around 30% of sales. Add Household, Frozen Foods and Dairy and the top five cover roughly 60%.

**Which categories are weakest?**
Seafood is last at about $0.01M. Breakfast, Starchy Foods and Others sit at around $0.02M each.

**Does outlet size matter?**
Medium outlets lead with $507.9K (42%), then Small with $444.8K (37%), and High with $249.0K (21%). I'd be careful with this one though. In the raw data some outlets are listed under more than one size (OUT010, for example, shows up as Small, Medium and High), so the size split isn't fully reliable.

**Does location tier matter?**
On total sales, Tier 3 is ahead ($472.1K), then Tier 2 ($393.2K) and Tier 1 ($336.4K). But this mostly comes down to how many outlets each tier has: Tier 3 has four, the others have three. Per outlet, Tier 2 does best at roughly $131K, compared with about $118K for Tier 3 and $112K for Tier 1. Tier 1 and Tier 3 each have a Grocery Store dragging their average down.

**Why does Supermarket Type1 account for so much of the revenue?**
Type1 brings in $787.5K, about 65% of everything. That looks impressive until you notice that 6 of the 10 outlets are Type1. Per outlet, Type1, Type2 and Type3 all earn around $131K. The two Grocery Stores are the real outlier at about $76K each.

**Do items earn more at certain outlet types?**
No. Average sales per item stay between $139.80 and $141.68 across every outlet type. The differences in total revenue come from how many items are sold, not from what each item earns.

**Does higher item visibility lead to higher sales?**
Not really. Grocery Stores give items the most visibility (0.10 on average, against 0.06 everywhere else), yet their sales per item ($140.29) are no better than the others.

**Do ratings differ by outlet type?**
Barely. They range from 3.91 to 3.93, which suggests customers are about equally satisfied across formats.

**Why is 2018 so high on the establishment-year chart?**
This one fooled me at first. It looks like outlets opened in 2018 perform better, but two outlets opened that year: OUT027 (Supermarket Type3, about $131K) and OUT019 (a Grocery Store, about $74K). Every other year has just one outlet, which is why they all sit near $130K. 2011 is low because its only outlet is a Grocery Store. So the year isn't really the driver; outlet type and outlet count are.

## What stood out

- Low Fat products make up almost two thirds of sales.
- Fruits and Vegetables and Snack Foods are the top earners.
- Outlet count explains most of the gaps between tiers and outlet types. Once you look per outlet, the supermarket types are almost identical.
- Grocery Stores earn roughly 40% less per outlet than supermarkets.
- More shelf visibility doesn't translate to more sales per item.
- Ratings and average sales per item are very consistent everywhere, so growth is about reach and volume.

## What I'd suggest

- Lean on supermarket formats for expansion, since each earns about $131K per outlet against about $76K for a Grocery Store.
- Look closely at the two Grocery Stores (OUT010 and OUT019). Either improve what they stock and how they price, or consider moving them to a supermarket format.
- Tier 2 looks like the best place for a new outlet, based on sales per outlet.
- Keep Low Fat well stocked, and test promotions on Regular items to see if that gap can close.
- Keep Fruits and Vegetables, Snack Foods, Household, Frozen Foods and Dairy as the priority for stock and promotions. Review Seafood, Breakfast and Starchy Foods to see if they deserve their shelf space.
- Rethink how shelf space is used in Grocery Stores, since high visibility isn't paying off.
- Clean up the outlet size data before using it for any decision.

## Dashboard features

- Four KPI cards: Total Sales, Average, No of Items, Average Rating
- Slicers for Outlet Location Type, Outlet Size and Item Type
- Toggle buttons to switch the fat content and item type charts between the four metrics
- Donut charts, bar charts, a funnel, an area chart and a color-coded matrix, all cross-filtering each other

## Running it yourself

1. Clone the repo.
2. Open the `.pbix` file in Power BI Desktop.
3. If it asks, point the data source to your local copy of the file (Transform data, then Data source settings).
4. Hit Refresh and use the slicers to explore.

## Limitations and next steps

- Item type values are rounded because they were read from the chart.
- There's no date or customer column, so I couldn't look at trends over time or customer behavior.
- With only 10 outlets, tier, type and year are tangled together. Each establishment year is basically one outlet, so I wouldn't read too much into year-based comparisons.
- Outlet Size is inconsistent within some outlets, as mentioned above.

If I take this further, I'd add profit or margin data, transaction dates for trend and seasonality analysis, a simple sales forecast, and publish it to Power BI Service with scheduled refresh.

---

Author :-
Atharva Jagtap
