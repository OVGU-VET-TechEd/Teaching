##### The following code generates an interactive visualization for explaining the **standard deviation**, **variance**, and related parameters for a normally distributed dataset.

#### Parameters:

$mu =$ <script modify="false" input="range" step="0.1" min="-10" max="10" value="0" output="mu">@input</script>,  
$sigma =$ <script modify="false" input="range" step="0.1" min="0.1" max="5" value="1" output="sigma">@input</script>,  
$n =$ <script modify="false" input="range" step="1" min="10" max="1000" value="100" output="n">@input</script>

<script modify="false" run-once style="display: inline-block; width: 100%">
"LIASCRIPT: ### $$f(x) = \frac{1}{\sigma\sqrt{2\pi}} e^{-\frac{(x-\mu)^2}{2\sigma^2}}$$"
</script>

<script run-once style="display: inline-block; width: 100%">
function normalDistribution(x, mu, sigma) {
  return (1 / (sigma * Math.sqrt(2 * Math.PI))) * Math.exp(-Math.pow(x - mu, 2) / (2 * Math.pow(sigma, 2)));
}

function generateData() {
  let data = [];
  const mu = @input(`mu`);
  const sigma = @input(`sigma`);
  const n = @input(`n`);
  for (let i = -15; i <= 15; i += 0.1) {
    data.push([i, normalDistribution(i, mu, sigma)]);
  }
  return data;
}

function calculateVariance(data, mean) {
  const variance = data.reduce((acc, x) => acc + Math.pow(x - mean, 2), 0) / data.length;
  return variance;
}

function calculateStandardDeviation(variance) {
  return Math.sqrt(variance);
}

let option = {
  grid: { top: 40, left: 50, right: 40, bottom: 50 },
  xAxis: {
    name: 'x',
    minorTick: { show: true },
    splitLine: { lineStyle: { color: '#999' } },
    minorSplitLine: { show: true, lineStyle: { color: '#ddd' } }
  },
  yAxis: {
    name: 'Probability Density', min: 0, max: 0.5,
    minorTick: { show: true },
    splitLine: { lineStyle: { color: '#999' } },
    minorSplitLine: { show: true, lineStyle: { color: '#ddd' } }
  },
  series: [
    {
      type: 'line',
      showSymbol: false,
      data: generateData()
    }
  ]
};

let data = generateData().map(d => d[1]);
let mean = @input(`mu`);
let variance = calculateVariance(data, mean);
let standardDeviation = calculateStandardDeviation(variance);

"HTML: <p><b>Variance (σ²):</b> " + variance.toFixed(2) + "</p>"
"HTML: <p><b>Standard Deviation (σ):</b> " + standardDeviation.toFixed(2) + "</p>"
"HTML: <p><b>Sample Size (n):</b> " + @input(`n`) + "</p>"

"HTML: <lia-chart option='" + JSON.stringify(option) + "'></lia-chart>"
</script>
