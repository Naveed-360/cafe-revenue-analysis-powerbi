<img width="100%" height="5%" alt="logo" src="https://github.com/user-attachments/assets/f38453ef-3880-4d4b-a615-c0679c429bf0" />

<div align="center">
  <!-- Replace this src with your own hosted logo image (Cloudinary, Imgur, or a file in your repo's /assets folder) -->
  <img width="320px" src="assets/amber-oak-logo.png" />
</div>

<h1 align="center">Amber &amp; Oak Café — Revenue Performance Report</h1>

<table align="center">
  <tr>
    <td width="1440">
      <h2 align="center">Client Background</h2>
      <body>
        <strong>Amber &amp; Oak Café</strong> is a quick-service café operating two order channels, In-store and Takeaway, serving a menu of eight core items: Salad, Sandwich, Smoothie, Juice, Cake, Coffee, Tea, and Cookie. This analysis covers <strong>2023</strong> transaction data and was built to give café management a clear view of which items, channels, and periods drive revenue, and where the underlying sales data itself needs better capture at the point of sale. <br>
        <br>
        <strong>Dataset:</strong> <a href="https://www.kaggle.com/datasets/ahmedmohamed2003/cafe-sales-dirty-data-for-cleaning-training">Dirty Cafe Sales Dataset</a> (Kaggle), 10,000 synthetic transaction records used as a practice dataset, deliberately containing missing values and invalid placeholder entries (<code>ERROR</code>, <code>UNKNOWN</code>) to simulate real-world data quality issues.
      </body>
    </td>
  </tr>
</table>

<table align="center">
  <tr>
    <td width="1440">
      <h2 align="center">Executive Summary</h2>
      <div align="center">
        <img width="700px" src="screenshots/revenue-by-item-and-location.png" />
        <img width="700px" src="screenshots/revenue-by-quarter-and-location.png" />
      </div>
      <h4>
        <ul>
          <li><strong>Total revenue: $88,952</strong> across the full 2023 transaction history.</li>
          <li>Revenue was <strong>stable across all four quarters</strong> (roughly $6.2K–$6.6K per quarter per channel), no strong seasonal spike.</li>
          <li><strong>In-store and Takeaway performed nearly identically</strong> overall ($27,127 vs $26,488 among transactions with a recorded channel).</li>
          <li><strong>Salad is the top overall revenue item</strong>, and also the top item specifically in Q4 ($2,715 combined).</li>
          <li><strong>Cake declined 8.8% from Q3 to Q4</strong> ($1,533 → $1,398), the clearest confirmed downward trend found in this analysis.</li>
          <li>A meaningful share of transactions, roughly a third, have no recorded Location or Payment Method, a checkout data-capture gap rather than a sales issue.</li>
        </ul>
      </h4>
    </td>
  </tr>
</table>

<table align="center">
  <tr>
    <td width="1440">
      <h2 align="center">Data Structure &amp; ERD</h2>
      <body>
        The model uses a simple two-table star schema: <br><br>
        <strong>Fact table — Cafe Sales:</strong> one row per transaction. Columns: Transaction ID, Item, Quantity, Price Per Unit, Total Spent, Payment Method, Location, Transaction Date. <br><br>
        <strong>Dimension table — Calendar:</strong> one row per calendar day (Date, Day, Month, Month Name), joined one-to-many on Date → Transaction Date, set as the model's official date table. Revenue, AOV, and top-item figures throughout this report are DAX measures calculated on top of the fact table, not stored columns within it. <br><br>
        <strong>Data preparation:</strong> the raw export contained <code>ERROR</code> and <code>UNKNOWN</code> placeholder text mixed into numeric and categorical fields, inconsistent types, and missing values. Where exactly one of Quantity, Price Per Unit, or Total Spent was missing, it was mathematically recovered from the other two (Total Spent = Quantity × Price Per Unit). Missing Payment Method and Location values could not be reliably inferred from any other column, so they were left as true nulls rather than guessed or filled.
      </body>
    </td>
  </tr>
</table>

<table align="center">
  <tr>
    <td width="1440">
      <h2 align="center">Insight Deep-Dive</h2>
      <h3>Sales Trend</h3>
      <div align="center">
        <img width="700px" src="screenshots/revenue-by-quarter-and-location.png" />
      </div>
      <h4>
        <ul>
          <li>Quarterly revenue stayed in a narrow $6.2K–$6.6K band per channel, roughly a 6% swing, not a sharp seasonal curve.</li>
          <li>In-store and Takeaway moved together through the year rather than trading places.</li>
        </ul>
      </h4>
      <h3>Product Performance</h3>
      <div align="center">
        <img width="700px" src="screenshots/cake-quarterly-trend.png" />
        <img width="700px" src="screenshots/q4-revenue-by-item.png" />
      </div>
      <table align="center">
        <tr><th>Rank</th><th>Item</th><th>In-store</th><th>Takeaway</th></tr>
        <tr><td>1 (highest)</td><td>Salad</td><td>$5.6K</td><td>$5.1K</td></tr>
        <tr><td>8 (lowest)</td><td>Cookie</td><td>~$1.0K</td><td>~$0.9K</td></tr>
      </table>
      <br>
      <body>
        <strong>Cake's Q3 → Q4 decline, verified:</strong>
        <ul>
          <li>In-store: $780 → $705 (−9.6%)</li>
          <li>Takeaway: $753 → $693 (−8.0%)</li>
          <li>Combined: $1,533 → $1,398 (<strong>−8.8%</strong>)</li>
        </ul>
        <strong>Best and worst by quantity sold (full year):</strong> Juice (3,373 units) narrowly leads Coffee (3,368 units), a five-unit margin close enough that it should be re-verified against the final cleaned dataset before being treated as settled. Cookie is lowest on both volume and revenue. <br><br>
        <strong>Volume vs. revenue mismatch:</strong> Coffee ranks near the top on order volume but near the bottom on revenue, pointing to a low per-unit price on a high-frequency item.
      </body>
      <h3>Average Order Value</h3>
      <body>
        Based on total revenue against total transaction count, average spend per order is approximately <strong>$8.90</strong>, a modest ticket size typical of a quick-service counter format.
      </body>
    </td>
  </tr>
</table>

<table align="center">
  <tr>
    <td width="1440">
      <h2 align="center">Recommendations</h2>
      <h3>Product</h3>
      <h4>
        <ul>
          <li>Feature and protect Salad's position as the top revenue driver, confirmed both as the overall leader and the Q4 leader specifically.</li>
          <li>Investigate Cake's Q3→Q4 decline (−8.8%, confirmed). A Q4 promotion or seasonal variant could offset the drop before it compounds into next year.</li>
          <li>Investigate Coffee's pricing: high order volume, low revenue contribution, a candidate for a modest price adjustment or an add-on bundle.</li>
          <li>Reassess Cookie's place on the menu, the lowest performer on both volume and revenue all year.</li>
        </ul>
      </h4>
      <h3>Channels</h3>
      <h4>
        <ul>
          <li>In-store and Takeaway perform almost identically overall; no current evidence justifies shifting investment toward one channel over the other.</li>
        </ul>
      </h4>
      <h3>Data Quality</h3>
      <h4>
        <ul>
          <li>Prioritize fixing Payment Method and Location capture at checkout. Roughly a third of transactions are missing Location, and roughly a quarter are missing Payment Method, limiting confidence in any channel- or payment-based conclusion.</li>
          <li>Extend the Item × Quarter breakdown built for Cake in this analysis to the full menu. Sandwich in particular showed a notably lower Q4 total ($1,792) worth checking against its own Q3 figure before drawing a conclusion.</li>
        </ul>
      </h4>
    </td>
  </tr>
</table>

<table align="center">
  <tr>
    <td width="1440">
      <h2 align="center">Limitations</h2>
      <h4>
        <ul>
          <li>Dataset is synthetic practice data, not live point-of-sale data, used here to demonstrate the analysis workflow.</li>
          <li>~33% of transactions have no recorded Location; ~26% have no recorded Payment Method (Digital Wallet 22.9%, Credit Card 22.7%, Cash 22.6% among the transactions that do have it recorded). These were left as nulls rather than imputed.</li>
          <li>The Juice vs. Coffee "top item by quantity" result is a near-tie and should be re-confirmed before being presented as a firm conclusion.</li>
          <li>Item-level quarterly trends were only fully verified for Cake in this pass. "Salad leads every quarter" is confirmed for Q4 and for the full year, not individually confirmed for Q1–Q3. Sandwich's Q4 total is known but its quarter-over-quarter trend is not yet verified.</li>
        </ul>
      </h4>
    </td>
  </tr>
</table>

<!--
SCREENSHOT CHECKLIST — export these from Power BI into a /screenshots folder,
and your own logo into /assets, filenames must match exactly:

1. assets/amber-oak-logo.png        — your café logo
2. screenshots/revenue-by-item-and-location.png   — full year, no filters
3. screenshots/revenue-by-quarter-and-location.png — full year, no filters
4. screenshots/cake-quarterly-trend.png            — quarter/location line chart, filtered to Cake
5. screenshots/q4-revenue-by-item.png              — item/location bar chart, filtered to Q4
-->
