# Dividend-ML-Prediction
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Dividend Announcement – Stock Return Prediction (RF & LR)</title>
  
<body>
  <div class="container">
    <div class="card">
      <h1>📈 Dividend Announcement – Stock Return Prediction (Random Forest & Logistic Regression)</h1>
      <p class="small muted">Predict whether stock price will go <strong>up (1)</strong> or <strong>down (0)</strong> in the 3 trading days after a dividend announcement. Includes RF and LR notebooks and code that use extended October 2025 data.</p>

  <h2>✅ Project Overview</h2>
      <p>Dividend announcements influence stock prices. This project builds a reproducible ML pipeline that predicts the next <strong>3-day average return direction</strong> after a dividend announcement.</p>
      <div class="cols">
        <div class="col card" style="padding:14px;">
          <h3 style="margin:0 0 6px;">Models</h3>
          <ul>
            <li><strong>Random Forest</strong> — accuracy & feature importances</li>
            <li><strong>Logistic Regression</strong> — interpretable probabilities</li>
          </ul>
        </div>
        <div class="col card" style="padding:14px;">
          <h3 style="margin:0 0 6px;">Target</h3>
          <p class="small">Binary: <code>1</code> if 3-day average future return ≥ 0, else <code>0</code>.</p>
        </div>
      </div>

  <h2>📊 Data Sources</h2>
      <table>
        <thead>
          <tr><th>File</th><th>Description</th></tr>
        </thead>
        <tbody>
          <tr><td><code>dividend_data_per_company_7.zip</code></td><td>Per-company dividend announcement CSVs (Announcement Date, Dividend ₹)</td></tr>
          <tr><td><code>combined_stock_data.csv</code></td><td>Main historical daily price data (till Sep)</td></tr>
          <tr><td><code>adjusted_stock_data_stripped_appended_daily_cleaned.csv</code></td><td>Extended dataset (includes Oct 2025)</td></tr>
        </tbody>
      </table>

   <h2>🧠 Workflow (high level)</h2>
      <ol>
        <li><strong>Load & clean</strong> dividend CSVs (from ZIP) and combined stock CSV(s).</li>
        <li><strong>Merge</strong> dividend announcements onto price rows by <code>(ticker, date)</code>.</li>
        <li><strong>Feature engineering</strong> (lags, vol, momentum, dividend features).</li>
        <li>Select the <strong>first trading day after each announcement</strong> (one row per announcement).</li>
        <li><strong>Train/test split</strong>: training announcements before <code>TRAIN_END</code>, testing announcements in the Aug–Oct window.</li>
        <li><strong>TimeSeries CV</strong> (TimeSeriesSplit) → threshold tuning → evaluation.</li>
        <li><strong>Save</strong> predictions CSV.</li>
      </ol>

   <h2>⚙️ Features</h2>
      <table>
        <thead><tr><th>Feature</th><th>Meaning</th></tr></thead>
        <tbody>
          <tr><td><code>lag1 / lag2 / lag3</code></td><td>Prior daily returns</td></tr>
          <tr><td><code>vol20</code></td><td>20-day return volatility (std)</td></tr>
          <tr><td><code>momentum10</code></td><td>10-day momentum (sum of returns)</td></tr>
          <tr><td><code>log_dividend</code></td><td>Log(1 + dividend amount)</td></tr>
          <tr><td><code>dividend_ratio</code></td><td>Current dividend vs past mean</td></tr>
          <tr><td><code>price_above_ma</code></td><td>Close &gt; 5-day moving average (0/1)</td></tr>
          <tr><td><code>dividend_change</code></td><td>Change in dividend amount vs previous</td></tr>
        </tbody>
      </table>

   <h2>📈 Evaluation & Outputs</h2>
      <p class="small">Evaluation metrics include <strong>F1 score</strong> and full classification reports. The pipeline also computes strategy returns (ML strategy vs perfect oracle vs baseline).</p>

  <h2>🧾 Output Files</h2>
      <ul>
        <li><code>ml_first_post_announcement_AugOct2025_RF_FIXED.csv</code> — RF predictions (Aug–Oct test window)</li>
        <li><code>ml_first_post_announcement_AugOct2025_LR_FIXED.csv</code> — LR predictions</li>
      </ul>

  <h2>▶️ How to run (Colab / Jupyter)</h2>
      <p class="small">Steps to reproduce locally or on Colab:</p>
      <ol>
        <li>Clone the repo or download files.</li>
        <li>Ensure the following files are in the working folder:
          <ul>
            <li><code>dividend_data_per_company_7.zip</code></li>
            <li><code>combined_stock_data.csv</code></li>
            <li><code>adjusted_stock_data_stripped_appended_daily_cleaned.csv</code></li>
          </ul>
        </li>
        <li>Open the notebook (e.g. <code>notebooks/Random_Forest_Dividend.ipynb</code>) in Colab or Jupyter.</li>
        <li>Run all cells (install libraries if needed).</li>
      </ol>

   <h2>📦 Requirements</h2>
      <pre><code>pandas
numpy
scikit-learn
xgboost   # optional if used
</code></pre>

   <h2>📁 Suggested Repository Structure</h2>
      <pre><code>dividend-stock-prediction/
├─ notebooks/
│  ├─ Random_Forest_Dividend.ipynb
│  └─ Logistic_Regression_Dividend.ipynb
├─ src/
│  ├─ random_forest.py
│  └─ logistic_regression.py
├─ data/
│  ├─ dividend_data_per_company_7.zip
│  ├─ combined_stock_data.csv
│  └─ adjusted_stock_data_stripped_appended_daily_cleaned.csv
├─ output/
│  ├─ ml_first_post_announcement_AugOct2025_RF_FIXED.csv
│  └─ ml_first_post_announcement_AugOct2025_LR_FIXED.csv
└─ README.html
</code></pre>

  <h2>📌 Notes & Tips</h2>
      <ul>
        <li>Make sure <code>TRAIN_END</code> and test window dates are set correctly in the notebook (we use <code>2025-08-01</code> and test up to <code>2025-10-31</code>).</li>
        <li>If a test set is empty, the notebook safely trains on available data and skips test evaluation to avoid crashes.</li>
        <li>Ensure column names in CSVs match expectations: common names are <code>Date, Symbol, Company Name, Close, Adj Close</code>. The code tries to be resilient but double-check if you modified files.</li>
      </ul>

   <h2>🌟 Business Value</h2>
      <p>Automates detection of dividend events likely to be followed by positive returns — useful for research desks, algo idea generation, and intern projects.</p>

  <h2>🤝 Contact</h2>
      <p class="small">If you need assistance or want the PDF / PPT version, ping me — happy to help.</p>

  <footer>
        <div class="small muted">Generated: README HTML for the Dividend Announcement ML project — share this file in the repo root or docs folder.</div>
      </footer>
    </div>
  </div>
</body>
</html>

