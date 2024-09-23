# Normal Distribution and p-Value (1)

## Introduction to Normal Distribution

The normal distribution, also known as the Gaussian distribution, is a probability distribution that is symmetric around the mean. In this type of distribution, most of the observations cluster around the central peak and the probabilities for values taper off equally on both sides.

![Normal Distribution](https://upload.wikimedia.org/wikipedia/commons/7/74/Normal_Distribution_PDF.svg)

---

### Key Properties of a Normal Distribution:

1. **Symmetry**: The distribution is symmetric around the mean.
2. **Mean, Median, Mode**: In a perfectly normal distribution, the mean, median, and mode are identical.
3. **Bell-shaped curve**: The distribution takes on a bell-shaped curve.
4. **68-95-99.7 Rule**: About 68% of the data falls within one standard deviation of the mean, 95% within two, and 99.7% within three.

---

## p-Value and Its Connection to the Normal Distribution

The **p-value** is a measure used in hypothesis testing to quantify the evidence against a null hypothesis. In the context of the normal distribution, p-values help us determine how extreme a given data point is within a distribution.

For a given test statistic, the p-value tells us the probability of observing a test statistic as extreme as, or more extreme than, the one observed under the assumption that the null hypothesis is true.

---

### The Role of the p-Value:
- **Low p-value (< 0.05)**: Suggests that the observed data is unlikely under the null hypothesis, leading to its rejection.
- **High p-value (> 0.05)**: Suggests that the observed data is consistent with the null hypothesis.

---

## Explore Different Distributions

@slider("Mean", 0, 100, 50)
var mean = 50

@slider("Standard Deviation", 1, 30, 10)
var stddev = 10

@slider("Skewness (left or right)", -3, 3, 0)
var skew = 0

### Visualizing the Normal Distribution
@js
@input(mean, -10, 10, 0.1, 0)
@input(stddev, 0.1, 10, 0.1, 1)
@input(skew, -5, 5, 0.1, 0)

function gaussian(mean, stddev, skew, count = 100) {
  let values = [];
  let start = mean - 4 * stddev;
  let end = mean + 4 * stddev;
  let step = (end - start) / count;

  for (let i = 0; i < count; i++) {
    let x = start + step * i;
    let y = (1 / (stddev * Math.sqrt(2 * Math.PI))) * Math.exp(-0.5 * Math.pow((x - mean) / stddev, 2));
    
    // Apply skewness to the distribution
    if (skew < 0) {
      y *= Math.exp(skew * (x - mean));
    } else {
      y *= Math.exp(-skew * (x - mean));
    }

    values.push({x: x, y: y});
  }
  return values;
}

let values = gaussian(mean, stddev, skew);

plotly.plot("gaussian-plot", [{
  x: values.map(v => v.x),
  y: values.map(v => v.y),
  type: 'scatter',
  mode: 'lines'
}], {
  title: `Normal Distribution with Mean=${mean}, Stddev=${stddev}, Skew=${skew}`,
  xaxis: {title: "Value"},
  yaxis: {title: "Probability Density"}
});

---

## Hypothesis Testing Example

Let’s take a simple hypothesis testing scenario:

- **Null Hypothesis (H0)**: The population mean is 50.
- **Alternative Hypothesis (H1)**: The population mean is different from 50.

We will calculate the p-value for a sample mean of **55** with a sample size of **30** and a population standard deviation of **10**.

@slider("Sample Mean", 45, 60, 55)
var sample_mean = 55

@slider("Sample Size", 10, 100, 30)
var sample_size = 30

@slider("Population Std Dev", 5, 15, 10)
var population_std = 10

### Calculating the Z-Score and p-Value

@js
function zScoreAndPValue(sample_mean, population_mean, population_std, sample_size) {
    let z_score = (sample_mean - population_mean) / (population_std / Math.sqrt(sample_size));
    let p_value = 2 * (1 - jStat.normal.cdf(Math.abs(z_score), 0, 1));
    return { z_score, p_value };
}

let results = zScoreAndPValue(sample_mean, 50, population_std, sample_size);

`Z-score: ${results.z_score} | P-value: ${results.p_value}`;

---

### Conclusion

When interpreting the p-value:
- A **small p-value** (typically ≤ 0.05) indicates strong evidence against the null hypothesis, so you reject the null hypothesis.
- A **large p-value** (> 0.05) suggests weak evidence against the null hypothesis, so you fail to reject the null hypothesis.
