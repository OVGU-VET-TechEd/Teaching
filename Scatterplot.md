The following interactive visualization generates a **scatter plot** to explain **variance**, **standard deviation**, and other key metrics related to the spread of data. You can manipulate settings related to **variance** to observe their impact.

#### Parameters:

$mu =$ <script modify="false" input="range" step="0.1" min="-10" max="10" value="0" output="mu">@input</script> (Mean),  
$sigma =$ <script modify="false" input="range" step="0.1" min="0.1" max="5" value="1" output="sigma">@input</script> (Standard Deviation),  
$n =$ <script modify="false" input="range" step="1" min="10" max="1000" value="100" output="n">@input</script> (Sample Size),  
**Add Outliers:** <script modify="false" input="checkbox" output="outliers">false</script> (Toggle to add extreme outliers)

<script modify="false" run-once style="display: inline-block; width: 100%">
"LIASCRIPT: ### Generating Random Data with mean μ=@input(`mu`) and standard deviation σ=@input(`sigma`)"
</script>

<script run-once style="display: inline-block; width: 100%">
function generateData() {
  const mu = @input(`mu`);
  const sigma = @input(`sigma`);
  const n = @input(`n`);
  let data = [];
  
  // Generate n random data points based on the mean and standard deviation
  for (let i = 0; i < n; i++) {
    data.push(mu + sigma * Math.random());
  }

  // Optionally add outliers to see their effect on variance and standard deviation
  if (@input(`outliers`)) {
    data.push(mu + sigma * 5);  // Add large outlier
    data.push(mu - sigma * 5);  // Add small outlier
  }
  
  return data;
}

let data = generateData();
let sortedData = data.slice().sort((a, b) => a - b);

function calculateVariance(data, mean) {
  const variance = data.reduce((acc, x) => acc + Math.pow(x - mean, 2), 0) / data.length;
  return variance;
}

function calculateStandardDeviation(variance) {
  return Math.sqrt(variance);
}

let mean = @input(`mu`);
let variance = calculateVariance(data, mean);
let standardDeviation = calculateStandardDeviation(variance);

// Scatter plot visualization
let scatterOption = {
  title: {
    text: 'Scatter Plot: Visualizing Variance and Standard Deviation',
  },
  tooltip: {
    trigger: 'item',
    formatter: function(params) {
      return 'Value: ' + params.data[1];
    }
  },
  xAxis: {
    name: 'Index',
    splitLine: { show: true }
  },
  yAxis: {
    name: 'Value',
    splitLine: { show: true }
  },
  series: [
    {
      type: 'scatter',
      data: data.map((value, index) => [index, value]),
      symbolSize: 8,
    }
  ]
};

"HTML: <lia-chart option='" + JSON.stringify(scatterOption) + "'></lia-chart>"

"HTML: <p><b>Mean (μ):</b> " + mean + "</p>"
"HTML: <p><b>Variance (σ²):</b> " + variance.toFixed(2) + "</p>"
"HTML: <p><b>Standard Deviation (σ):</b> " + standardDeviation.toFixed(2) + "</p>"
"HTML: <p><b>Sample Size (n):</b> " + @input(`n`) + "</p>"
"HTML: <p><b>Outliers Added:</b> " + (@input(`outliers`) ? 'Yes' : 'No') + "</p>"
</script>
