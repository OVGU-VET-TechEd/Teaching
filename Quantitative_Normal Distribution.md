# Normal Distribution and p-Value

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

```javascript
// Function to create a normal distribution with skewness adjustment
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

        values.push([x, y]);
    }
    return values;
}

// Create chart
let chart = {
    type: 'line',
    data: {
        labels: [],
        datasets: [{
            label: 'Probability Density',
            data: [],
            fill: true,
            borderColor: 'rgba(75, 192, 192, 1)',
            tension: 0.1
        }]
    },
    options: {
        scales: {
            x: {
                type: 'linear',
                position: 'bottom',
                title: {
                    display: true,
                    text: 'Value'
                }
            },
            y: {
                title: {
                    display: true,
                    text: 'Probability Density'
                }
            }
        }
    }
};

// Populate chart data
let values = gaussian(mean, stddev, skew);
chart.data.labels = values.map(v => v[0]);
chart.data.datasets[0].data = values.map(v => v[1]);

showChart(chart);
