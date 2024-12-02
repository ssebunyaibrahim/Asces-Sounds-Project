# `TABLE OF CONTENTS`

- [`PROJECT BACKGROUND`](#project-background)
  - [*`Objectives`*.](#objectives)
- [`METRICS AND DIMENSIONS`](#metrics-and-dimensions)
  - [`Metrics (Quantitative Measures):`](#metrics-quantitative-measures)
  - [`Dimensions (Categorical Attributes for Analysis):`](#dimensions-categorical-attributes-for-analysis)
  - [](#)
- [`DATA STRUCTURE & INNITIAL CHECKS`](#data-structure--innitial-checks)
  - [`Technical Analysis Process`](#technical-analysis-process)
  - [](#-1)
- [`EXECUTIVE SUMMARY`](#executive-summary)
  - [`Full performance Report`](#full-performance-report)
- [`INSIGHTS DEEP DIVE`](#insights-deep-dive)
  - [`Overall Growth Trends.`](#overall-growth-trends)
  - [`Product Analysis`](#product-analysis)
  - [`Analysis on Discounts`](#analysis-on-discounts)
- [`RECOMMENDATIONS`](#recommendations)
  - [`Sales Strategy Team: Leverage Seasonal Trends for Strategic Focus`](#sales-strategy-team-leverage-seasonal-trends-for-strategic-focus)
  - [`Marketing Team: Optimize Discount Band Utilization`](#marketing-team-optimize-discount-band-utilization)
  - [`Product Development Team: Address Product Profitability and Market Positioning`](#product-development-team-address-product-profitability-and-market-positioning)
- [`CAVEATS AND ASSUMPTIONS`](#caveats-and-assumptions)



# `PROJECT BACKGROUND`

Asces Sound, established in 2022, is a new leading innovator in audio technology, offering high-quality solutions for professionals and enthusiasts. Specializing in cutting-edge audio interfaces, microphones, and accessories, the company combines exceptional sound clarity, advanced engineering, and durable designs to meet modern audio production demands.

As a Data Analyst in the Data Analytics Team at Asces Sound, I am collaborating closely with the Product Development, Marketing and Sales Strategy Teams to generate actionable insights that will shape strategic decision-making. This project aims to provide a comprehensive view of various performance metrics, empowering stakeholders with the data needed to enhance sales strategies, optimize marketing efforts, and prioritize product innovation.

In response to a directive from the leadership team, I am also tasked with designing a Performance Analytics Dashboard. This dashboard will serve as a centralized tool for tracking and evaluating key performance indicators (KPIs) such as revenue trends, profit margins, and regional sales performance. It will also shed light on the impact of discounting strategies and provide a year-over-year (YoY) comparison of sales and profit growth.

## *`Objectives`*.
Through this initiative, I aim to:
1. Support the Sales Strategy Team: By identifying top-performing regions, customer segments, and products to focus resources effectively.

2. Guide the Marketing Team: By offering insights into revenue distributions across discount bands, enabling the optimization of promotional strategies.

3. Empower the Product Development Team: By analyzing product-specific data to identify high-performing categories and areas for innovation.

# `METRICS AND DIMENSIONS`
<details>
  <summary>Here are the definitions for the key metrics and dimensions used in this analysis:</summary>

  ## `Metrics (Quantitative Measures):`
  -	**Sales Revenue**  
  Definition: The total income from sales before deducting expenses.   
  Importance: Measures overall sales performance and business growth.  
  Calculation: Revenue = Units Sold × Sale Price

  - **Profit**  
  Definition: The financial gain after subtracting the cost of goods sold.  
  Importance: Evaluates how efficiently a company is generating profit.  
  Calculation: Profit = Revenue - (Units Sold × Cost Price)  

  -	**Units Sold**  
  Definition: The total quantity of products sold in each period.  
  Importance: Indicates product demand and sales volume trends.

  - **YoY Growth Rate**  
  Definition: The year-over-year percentage change in key metrics like revenue, profit, and unit sales.  
  Importance: Reveals trends in business performance over time.  
  Calculation: YoY Growth = (Current Year Value - Previous Year Value) / Previous Year Value × 100

  - **Profit Margin**  
  Definition: Percentage of revenue that is profit after costs.  
  Importance: Measures financial efficiency.  
  Calculation: Profit Margin = (Profit / Revenue) × 100

  - **AOV (Average Order Value)**  
  Definition: Percentage of revenue that is profit after costs.  
  Importance: Average value per order.  
  Calculation: AOV = Total Revenue / Number of Orders  

  ## `Dimensions (Categorical Attributes for Analysis):`

  - **Country**  
  Use: Analyze sales and profitability based on geographic location.  
  Importance: Identifies regional performance and market opportunities.

  - **Product**  
  Use: Break down performance data by specific products.  
  Importance: Helps guide product strategy, development, and marketing.  

  - **Customer Type**  
  Use: Segment data based on customer groups.  
  Importance: Understand customer behavior and target marketing efforts effectively.

  - **Date (Year, Month, Day)**  
  Use: Analyze sales and performance trends over time.  
  Importance: Helps identify seasonality and plan campaigns or inventory for peak periods.  

  - **Discount Band**  
  Use: Analyze sales performance by different discount ranges  
  Importance: Identifies how varying discount levels affect revenue and profitability.    

</details>
---  

# `DATA STRUCTURE & INNITIAL CHECKS`

This Entity-Relationship (ER) Diagram provides a visual representation of the data structure used in this analysis, highlighting key entities such as products, customers, sales, and their relationships. 

<img src="Images/Ascess ER Diagram-2024-12-01-120342.svg"
     style="width: 600px; height: 600px; display: block; margin: 40px auto;"/>

## `Technical Analysis Process`

<details>
  <summary>Technical details: Technical details about the analysis process, including data cleaning, transformations, and modeling approaches, can be found here.</summary>


  The analysis journey began with preparing the data tables provided as CSV files. These files were imported into MySQL, where I structured a relational database and utilized SQL queries to join tables based on key relationships. During this step, I generated essential metrics such as **Profit**, **Total Cost**, **Total Revenue**, and **Discounted Revenue**.

  <img src="Images/SQL QUERY.png"
      style="width: 500px; height: 500px; display: block; margin: 40px auto;"/>

  **`Transition to Power BI`**

  The joined table was exported as a CSV file and imported into Power BI for further processing. In Power Query, transformations were made to ensure data consistency and proper formatting. Upon discovering discrepancies in product image URLs, I created a new csv file, with accurate image links and Product ID as the key. This table was later joined to the existing model. 

  <img src="Images/Power query.png"
      style=" height: 400px; display: block; margin: 40px auto;"/>

  A **Date Table** was Also generated using **DAX**, enabling detailed time-series analysis.   
  Below is an image of the full model as setup in Power Bi.

  <img src="Images/Data Model.png"
      style=" height: 400px; display: block; margin: 40px auto;"/>

  Additionally, multiple **DAX measures** were created to compute **key performance indicators (KPIs)** and **metrics** required for visualization. The code for the DAX Expressions used in the Analysis are also presented Below.

  ``` YEAR = FORMAT('Date Table'[Date], "YYYY")

  Month_name = FORMAT('Date Table'[Date], "MMMM")

  Cost = "$" & Product_sales[Cost Price] 

  Price = "$" &Product_sales[Sale Price]

  Profit Growth(YoY) = DIVIDE(
      SUM('Product_sales'[Profit])-[Profit last Year],[Profit last Year])

  Profit last Year = CALCULATE(
      SUM('Product_sales'[Profit]),
      SAMEPERIODLASTYEAR('Date Table'[Date]))

  AOV = DIVIDE(SUM('Product_sales'[Revenue]), SUM(Product_sales[Units Sold]))

  Profit Margin = DIVIDE(
      SUM(Product_sales[Profit]),
      SUM(Product_sales[revenue]),0)

  Revenue Growth (YoY) = 
      var last_year =CALCULATE(SUM(Product_sales[revenue]),SAMEPERIODLASTYEAR('Date Table'[Date]))
      RETURN
      (SUM(Product_sales[revenue])-last_year)/last_year

  Total Units Sold = SUM(Product_sales[Units Sold])

  Total Units Sold (last year) = CALCULATE(
      [Total Units Sold],
      SAMEPERIODLASTYEAR('Date Table'[Date]))

  Units Sold Growth (YoY) = DIVIDE(
      [Total Units Sold]-[Total Units Sold (last year)],
      [Total Units Sold (last year)])
  ```
  **`Visualizations and Analysis`**

  With this, I proceeded to design the report pages

  <img src="Images\report pages.png"
      style=" height: 400px; display: block; margin: 40px auto;"/>  


</details>  
---  

# `EXECUTIVE SUMMARY`
Between **2022** and **2023**, **total sales** amounted to approximately **$2.95 million**, reflecting an **8.02% growth**, while **profits** increased by **6.68%** over the same period. The average **annual sales** were **$2,827**, with the **Average Order Value (AOV)** remaining largely consistent at **$177** in 2022 and **$178** in 2023.

The **MV7 microphone** led in growth with a **20.23%** increase, while the **AudioBox USB 96 Studio** dominated profitability at **44.96%**. Notably, the **Medium Discount Band** generated over **$400,000** in total revenue, with **government** customers contributing **43.06%**. The team recommends Leveraging Q4’s momentum, optimizing the Medium Discount Band for maximum impact, and addressing production inefficiencies in the MV7 to boost profitability.

## `Full performance Report`
This is a screenshot of the interactive full performance report page that was designed for the analysis as requested by the team leader.

<img src="Images\Full performance dashboard.png"
      style=" height: 500px; display: block; margin: 40px auto;"/>  


# `INSIGHTS DEEP DIVE`

## `Overall Growth Trends.`
<img src="Images\Over all growth trends.png"
      style=" height: 450px; display: block; margin: 40px auto;"/>  

From 2022 to 2023 the total number of sales grew by 8.02% with an 8.87% growth in sales and a 6.68% growth in profits, the average number of sales per year was 15, with average yearly sales revenue of $2827 and average order value of $178.

2023 saw the highest average number of sales at 16 per month, with the year also leading in both total sales and average sales metrics. Despite this, the Average Order Values (AOV) were nearly identical across both years (2022: $177, 2023: $178), suggesting consistent customer purchasing behavior. In 2022, sales outperformed 2023 only during February and June, while 2023 consistently led in sales for the remaining months of the year.

In 2023, total sales peaked in October with $194,108, followed by December at $182,708 and November at $101,384, highlighting that the fourth quarter was the most profitable period. This trend aligns with potential seasonal factors, such as holiday promotions or increased consumer demand during the festive season. Among the products, the NT1-A microphone led in sales during that season.

## `Product Analysis`
<img src="Images\Product analysis.png"
      style=" height: 450px; display: block; margin: 40px auto;"/>  

The MV7 microphone emerged as the fastest-growing product, with a 20.23% growth rate over the two years, likely fueled by the increasing number of content creators in 2023 seeking tools optimized for recording and streaming. Following closely, the NT1-A microphone achieved a notable 16.54% growth rate, while the QuadCast S microphone experienced a decline with a -2.67% growth rate, signaling a need to reassess its market strategy or positioning.

In terms of total sales, the Scarlet 2i2 audio interface dominated, with an average annual revenue of $422,246, nearly doubling the performance of the runner-up, the NT1-A microphone, which averaged $251,900 per year. On the lower end, the QuadCast S microphone and the Arctis 7P+ headset were the least profitable products, with average annual revenues of $179,658 and $173,361, respectively. Notably, the highest sales for the Scarlet 2i2 were recorded in Canada, while Germany reported the lowest sales figures.

From a profitability perspective, the AudioBox USB 96 Studio showcased a remarkable profit margin of 44.96%, significantly outperforming all other products, which averaged less than half of this margin. This highlights the AudioBox USB 96 as the company’s most profitable product post-cost, presenting an opportunity to explore scaling or promotional efforts for this line. Conversely, despite its high growth, the MV7 microphone reported the lowest profit margin, indicating potential inefficiencies in production or pricing strategies.

Customer demographics reveal that government contracts constitute 43.06% of the company’s customer base, three times higher than other segments, which average around 13% each. The Scarlet 2i2 audio interface leads in customer demand, attracting 28.71% of the total customer base, justifying its sales dominance. Meanwhile, the MV7 microphone and NT1-A microphone had the smallest customer shares, each accounting for 13.30% of the total, suggesting opportunities to expand their appeal.


## `Analysis on Discounts`
<img src="Images\Product analysis.png"
      style=" height: 450px; display: block; margin: 40px auto;"/>  

The High Discount Band contributed the largest share of revenue at 34.93%, followed closely by the Medium Band at 34.52%, with the Low Band and Non-Discount Band trailing at 23.93% and 6.61%, respectively.

Despite its smaller revenue share, the Non-Discount Band delivered the highest profit margin at 32.39%, significantly outperforming the Low Band (23.79%), Medium Band (22.03%), and High Band (16.58%).

Customer segmentation revealed that Government buyers drove the majority of revenue, with the Medium and High Discount Bands each generating over $400,000, explaining their dominance in revenue share.

This discount analysis suggests that the Medium Discount Band drives substantial revenue while maintaining profitability, making it an ideal focus for strategic initiatives.

---
# `RECOMMENDATIONS`
## `Sales Strategy Team: Leverage Seasonal Trends for Strategic Focus`
- **Q4 Focus:** Q4 saw the highest total sales in 2023, with October generating $194,108, followed by December ($182,708). The Sales Strategy Team should capitalize on these peaks by aligning future campaigns to target the fourth quarter, especially focusing on high-performing products like the NT1-A microphone.

## `Marketing Team: Optimize Discount Band Utilization`
- **Medium Discount Band Optimization:** The Medium Discount Band contributed 34.52% of total revenue and yielded a 22.03% profit margin. The Marketing Team should prioritize this band due to its strong performance in both revenue and profitability.
  
- **Segmented Promotions:** Government buyers, contributing 43.06% of total revenue, drove the most substantial sales within the Medium and High Discount Bands (both generating over $400,000). Tailor campaigns to this segment, focusing on maximizing sales and profitability from these discount bands.
•	Broaden Product Appeal: The MV7 and NT1-A microphones represent just 13.30% of the customer base each. The Marketing Team should explore ways to increase their appeal across new markets or customer segments, potentially boosting overall

## `Product Development Team: Address Product Profitability and Market Positioning`
- **Focus on the AudioBox USB 96 Studio:** With a 44.96% profit margin, the AudioBox USB 96 Studio was the most profitable product. The Product Development Team should scale this line, introducing new features or variations to maximize revenue.
  
- **Address Low-Margin Products:** The MV7 microphone, while growing at 20.23%, reported the lowest profit margin at 16.58%. This product requires a review of production processes and pricing to increase profitability while maintaining its growth trajectory.
  
# `CAVEATS AND ASSUMPTIONS`
- **Product Data Table Adjustments:** The image URLs in the product data table were found to be incorrect at the time of analysis in Power BI. A new file with corrected URLs was created in excel and added to the existing model in Power BI
  
- **Sales Data Accuracy:** The analysis presumes that the sales, discount, and revenue data provided were complete and accurate. Any missing or erroneous records could affect results and insights.
  
- **Discount Effectiveness:** It was assumed that the revenue attributed to each discount band directly reflects customer responsiveness to those discounts without external factors (e.g., economic conditions or seasonal promotions) heavily influencing outcomes.
  
- **Fiscal Year Alignment:** The analysis assumes that the company’s fiscal year aligns with the standard calendar year (January to December), meaning trends are interpreted accordingly.


