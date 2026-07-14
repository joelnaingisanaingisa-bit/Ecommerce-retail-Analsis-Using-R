# Ecommerce-retail-Analsis-Using-R
### E-commerce sales performance analysis
Business scenario: An online retail company wants to understand its sales performance using historical order data. Management needs insights into which product categories generate the highest revenue, which cities contribute most to sales, and how payment methods and order status affect customer satisfaction.
The dataset contains 50 e-commerce orders with details such as product category, quantity, unit price, total price, city, payment method, order status, and customer ratings.

## Problem statement
The company wants to answer the following questions:
Which product categories generate the highest sales revenue?
Which cities contribute the most to total sales?
What are the most commonly used payment methods?
How do order statuses such as Delivered, Processing, or Returned affect customer ratings?
What trends can be observed in sales over time?
 
## View The Dataset 
<img width="1182" height="692" alt="Screenshot 2026-07-14 144207" src="https://github.com/user-attachments/assets/26092ece-25c5-4cf8-a689-be177a26d0b0" />

### Project objectives
Clean and prepare the dataset for analysis.
<img width="728" height="461" alt="Screenshot 2026-07-14 144114" src="https://github.com/user-attachments/assets/23bd7f6a-7231-427c-a234-c778293e0727" />

 
# Check our clean data
summary(df_clean)

<img width="800" height="482" alt="Screenshot 2026-07-14 144622" src="https://github.com/user-attachments/assets/9b31a6c7-2735-4fa6-835b-64e873409014" />
# summary(df_clean)
<img width="750" height="358" alt="Screenshot 2026-07-14 144929" src="https://github.com/user-attachments/assets/82d42dce-5cdf-4ae1-b757-61f516071912" />
# overview
readr's column specification confirms 8 character columns and 4 numeric columns, matching OrderID, CustomerID, ProductCategory, ProductName, OrderDate, City, PaymentMethod, and OrderStatus as text, with Quantity, UnitPrice, TotalPrice, and CustomerRating as numeric. Note that OrderDate is imported as a character string in DD/MM/YYYY format, and CustomerRating contains missing values (NA) — both addressed in the next step.

# Two mutations are chained with the pipe operator (%>%):
<img width="811" height="481" alt="Screenshot 2026-07-14 145052" src="https://github.com/user-attachments/assets/ca97a329-30d6-495f-a6c2-8bb171dfd2b7" />


### Revenue Performance by City
 With the data clean, df_clean is grouped by City to compare total revenue and order volume across Kenyan cities. group_by() + summarize() collapses all rows for each city into a single summary row, and arrange(desc(TotalRevenue)) sorts cities from highest to lowest earner. 
<img width="716" height="407" alt="Screenshot 2026-07-14 145214" src="https://github.com/user-attachments/assets/7073c93e-f3d9-4f56-8fbd-853a3290cc07" />

Mombasa leads with the highest total revenue (KES 4,211.7) from just 6 orders, suggesting a higher average order value there. Eldoret and Nairobi generate solid revenue too but from more, lower-value orders (9 each), while Malindi appears only once in the sample and Thika trails with the second-lowest revenue. This kind of city-level rollup is a natural next step after cleaning — it turns a flat transaction log into an actionable regional performance view. 


### Visualizing Revenue by Product Category
The revenue-by-category table from the previous section is now visualized as a horizontal bar chart, sorted from lowest to highest so the strongest category reads at the top. reorder() sorts the bars by TotalSales, and coord_flip() turns the vertical bar chart into a horizontal one so the category labels stay readable. 

<img width="700" height="431" alt="Screenshot 2026-07-14 145347" src="https://github.com/user-attachments/assets/bdb607bc-6d36-4efb-8b48-0541b7a57ab1" />
# overview
Home & Kitchen and Toys are the top two revenue-generating categories, together contributing more than Beauty and Groceries combined. Beauty is the smallest category by a clear margin, at roughly a third of the top performer's revenue — a useful signal for where to focus promotions or restocking. 

### Daily Sales Trend 
Grouping by OrderDate and summing TotalPrice produces a day-by-day revenue series, plotted as a line chart to reveal volatility and any emerging pattern across the sample period. 
<img width="762" height="402" alt="Screenshot 2026-07-14 145558" src="https://github.com/user-attachments/assets/42dd4c4b-d785-4b4c-b14b-bb70eb6edadd" />

### Customer Payment Preferences
Grouping by PaymentMethod and counting orders (with total revenue as a secondary measure) shows which payment options customers actually use. 

<img width="700" height="341" alt="Screenshot 2026-07-14 145719" src="https://github.com/user-attachments/assets/ce0212d8-6afb-4729-bae6-17006e933b90" />
PayPal is the most-used payment method (15 of 50 orders), followed closely by Cash on Delivery (12). Together these two account for over half of all transactions, which is notable for a Kenyan e-commerce context where Mobile Money (M-Pesa-style payments) might be expected to dominate — here it ranks lowest by order count, though its per-order revenue is still meaningful. 

<img width="687" height="358" alt="Screenshot 2026-07-14 145752" src="https://github.com/user-attachments/assets/0bf64f4a-9fd2-4bae-b87b-55d63c03ea64" />

### Order Status vs. Customer Rating 

To examine whether how an order was fulfilled relates to how customers rated it, CustomerRating is summarized by OrderStatus (mean, standard deviation, and median), then visualized as a boxplot to show the full distribution rather than just averages. 

<img width="740" height="367" alt="Screenshot 2026-07-14 145942" src="https://github.com/user-attachments/assets/2612b957-100b-471b-9570-d572a5e40ac8" />
Average ratings are fairly close across all five statuses (roughly 2.9 to 3.5), and the boxplot shows heavy overlap in the interquartile ranges — so, at this sample size, order outcome doesn't show a strong relationship with how customers rated their experience. Processing and Delivered orders edge slightly higher on average, while Returned and Shipped trend a little lower, but the wide spread (SD around 0.6–1.7) means these differences aren't dramatic. A larger dataset would be needed to confirm whether this pattern holds. 
## <img width="690" height="425" alt="Screenshot 2026-07-14 150006" src="https://github.com/user-attachments/assets/0be54e01-01da-484c-83a8-ba9663c3b4d8" />

######  Key Takeaway
#  ● dmy() from lubridate correctly parses DD/MM/YYYY dates — a common format in Kenyan business data — into R's native Date class, enabling date arithmetic and time-series analysis.
 # ● Replacing missing CustomerRating values with the median (rather than dropping rows) preserves all 50 orders while avoiding the influence of outlier ratings. 
# ● The pipe operator (%>%) chains both mutate() calls into a single readable transformation pipeline, a core tidyverse convention used throughout DDMA Paper 10 coursework. 
# ● summary(df_clean) confirms the cleaning worked: 0 missing CustomerRating values remain, and OrderDate is now a genuine Date type rather than text. 
# ● group_by() + summarize() + arrange() turns the cleaned transaction-level data into a ranked city performance table in four lines — the same pattern generalizes to grouping by ProductCategory, PaymentMethod, or OrderStatus. 
# ● ggplot2's grammar (aes, geom_col, geom_line, geom_boxplot, theme_minimal) turns each grouped summary into a clear chart with minimal code — reorder() + coord_flip() is a reusable trick for readable horizontal bar charts. 
# ● Payment method and order-status-vs-rating analyses both show relatively even distributions in this 50-row sample — a reminder that patterns which look meaningful in a chart should be checked against sample size before drawing firm conclusions. 

 #### KEY INSIGHTS
The project can reveal the highest revenue-generating product categories.
It can identify the top-performing cities for sales.
It can show which payment methods customers prefer.
It can highlight whether returned or delayed orders are associated with lower customer ratings.
It can provide actionable recommendations for inventory planning, marketing focus, and customer service improvement.






