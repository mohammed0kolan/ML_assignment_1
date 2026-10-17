# CS 461 Machine Learning ? Assignment 1: EDA

## Dataset overview

The original CSV has 2,000 rows and 21 columns: 20 input features and the `price_range` target. All columns are stored as numbers. Some numbers represent categories: `blue`, `dual_sim`, `four_g`, `three_g`, `touch_screen`, and `wifi` are binary features, while `price_range` has four ordered classes (0?3). After cleaning, the dataset has 1,996 rows.

## Descriptive statistics

I used `describe()` to calculate the count, mean, standard deviation, minimum, quartiles, and maximum. These selected statistics are from the final cleaned data:

| Feature | Mean | Median | Std. dev. | Min | Max |
|---|---:|---:|---:|---:|---:|
| `battery_power` | 1,239.48 | 1,229.5 | 439.26 | 501 | 1,998 |
| `ram` | 2,122.88 | 2,140.5 | 1,085.31 | 256 | 3,998 |
| `px_height` | 644.27 | 562.5 | 444.21 | 0 | 1,960 |
| `px_width` | 1,250.98 | 1,247 | 431.97 | 500 | 1,998 |
| `screen_area` | 80.28 | 55 | 76.87 | 0 | 342 |

Binary columns are coded as 0 and 1, so their means show the fraction of phones with value 1. `price_range` is a class label, so its average is not a physical measurement. The notebook includes the full `describe()` output.

## Outlier check and cleaning

I used the IQR rule: a value is flagged when it is below Q1 ? 1.5 ? IQR or above Q3 + 1.5 ? IQR. This rule finds unusual values, but it does not prove that they are errors. For example, it flagged 477 rows with `three_g = 0` because this is a binary category, so I kept those rows.

I removed two rows with impossible negative battery power or screen width, reducing the data from 2,000 to 1,998 rows. I then removed two extreme RAM values (12,631 and 26,032), which were above the IQR upper limit of 5,859. The final dataset has 1,996 rows. I kept the high `fc` values (17?19) and `px_height` values (1,949 and 1,960) because they could be valid measurements; being flagged by IQR alone was not enough reason to remove them.

## Missing values

There were no missing values in the original or final dataset, so no filling or removal for missing data was needed.

## Engineered feature: screen area

I created `screen_area` by multiplying `sc_w` by `sc_h`. Its correlation with `price_range` is 0.041. For comparison, `sc_w` has a correlation of 0.039 and `sc_h` has 0.022. Screen area is slightly higher, but all three correlations are very weak.

## Min?Max scaling and data leakage

Min?Max scaling uses `(value ? minimum) / (maximum ? minimum)`. I scaled the input features and left `price_range` unchanged as the target. The scaled values fall in the range [0, 1], including the endpoints. The notebook shows sample values before and after scaling and checks the feature ranges.

For this assignment, I demonstrated scaling on the full cleaned data. In a real machine-learning workflow, I would split the data first, fit the scaler on the training data, and use it to transform the test data. Fitting it on all the data lets test-set information affect preprocessing. This is data leakage and can make evaluation less reliable.

## Price range counts and balance

| Price range | Number of phones |
|---:|---:|
| 0 | 500 |
| 1 | 499 |
| 2 | 498 |
| 3 | 499 |

The target is balanced because the class counts are almost equal. If one class had many more examples, a classifier might favor that class and perform worse on the less common classes. That issue is unlikely here.

## Binary features by price range

The table gives the percentage with feature value 1 in each price range:

| Price range | Dual SIM | Bluetooth | Touch screen | Wi-Fi |
|---:|---:|---:|---:|---:|
| 0 | 50.0% | 48.6% | 52.4% | 49.6% |
| 1 | 51.1% | 49.1% | 52.3% | 50.3% |
| 2 | 49.6% | 48.4% | 46.8% | 50.6% |
| 3 | 52.9% | 51.9% | 49.5% | 52.5% |

These percentages are close to 50% across price ranges, so the four features do not show a large difference between classes in this dataset.

## Correlations

The strongest positive feature pairs include `sc_w` and `screen_area` (r = 0.960), which is expected because screen width is part of the area calculation; `fc` and `pc` (r = 0.644); `four_g` and `three_g` (r = 0.584); and `px_height` and `px_width` (r = 0.510). RAM has the strongest correlation with `price_range` (r = 0.917), followed by battery power (r = 0.202). The notebook includes the full correlation results and a bar chart. Correlation shows association, not cause.

## Negative correlation

I did not find a strong negative correlation. The most negative pair is `four_g` and `talk_time`, at about r = -0.046, which is very weak. One possible real-world explanation could be that 4G uses extra power and reduces call time, but these data do not show a meaningful relationship. Therefore, the assignment's request for a strong negative pair is not supported by this dataset.

## Generative AI use

please note that Generative AI was used for brainstorming, code debugging, and report FORMATTING.the rest is my own honest Work.

## Conclusion

The final cleaned data has 1,996 rows and no missing values. Its price classes are balanced. RAM has the clearest relationship with price range, while screen area has only a very small correlation. No strong negative feature relationship appears in the data.