# Fashion_Sales_Retail_Project

**Dataset:**
The dataset is the retail sales of fashion items consisting columns such as Customer Reference ID,Item Purchased,Purchase Amount (USD),Date Purchase,Review Rating,Payment Method. I will be answering four questions which are: 
- Which months have the highest and lowest sales overall?
- Is there a recurring pattern in sales over the years during the same months (e.g., is December always high)?
- How do monthly sales trends differ by product category (e.g., tops, pants, accessories)?
- Which fashion items perform best during specific seasons?

**Tools Used:**
- PostgreSQL
- SQL
- Visual Studio Code 
- Git & Github
### 1. Which months have the highest and lowest sales overall?
It was mentiioned above that the dataset provided seems to be incomplete as some of the months in both 2022 and 2023 are left blank or have not been recorded. I first need to check where the dataset begins and ends 

```sql
SELECT MIN(date_purchase), MAX(date_purchase)
FROM customer_purchases;
```
The data started on October 2, 2022 and ended on October 1, 2023. That means that the data from January 1, 2022 to the end of September 2022 is missing, as well as the majority of October 2023 to the end of December 2023. 

```sql
SELECT 
    EXTRACT(YEAR FROM date_purchase) AS year,
    EXTRACT(MONTH from date_purchase) AS month,
    SUM(purchase_amount_usd) AS total_sales

FROM customer_purchases
GROUP BY EXTRACT(MONTH from date_purchase), EXTRACT(YEAR FROM date_purchase) 
HAVING EXTRACT(YEAR FROM date_purchase) = '2022'
ORDER BY SUM(purchase_amount_usd) DESC;

--in the year 2022 Highest: December with 46851 Lowest: October with 30812

SELECT 
    EXTRACT(YEAR FROM date_purchase) AS year,
    EXTRACT(MONTH from date_purchase) AS month,
    SUM(purchase_amount_usd) AS total_sales

FROM customer_purchases
GROUP BY EXTRACT(MONTH from date_purchase), EXTRACT(YEAR FROM date_purchase) 
HAVING EXTRACT(YEAR FROM date_purchase) = '2023'
ORDER BY SUM(purchase_amount_usd) DESC;
--in the year 2023 Highest: May with 45352 Lowest: October with 423

```
### Findings:
In the year 2022, three out of 12 months are accounted for, making December having the highest sales (46851) and October having the lowest sales (30812). In the year 2023, ten out of 12 months are accounted for, making May having the highest sales (45352), and October having the lowest sales (423). 

### Is there a recurring pattern in sales over the years during the same months (e.g., is December always high)?

```sql
SELECT 
  TO_CHAR(date_purchase, 'Month') AS month,
  SUM(CASE WHEN EXTRACT(YEAR FROM date_purchase) = 2022 THEN purchase_amount_usd ELSE 0 END) AS total_sales2022,
  SUM(CASE WHEN EXTRACT(YEAR FROM date_purchase) = 2023 THEN purchase_amount_usd ELSE 0 END) AS total_sales_2023

FROM customer_purchases
GROUP BY TO_CHAR(date_purchase, 'Month'), EXTRACT(MONTH FROM date_purchase)
ORDER BY EXTRACT(MONTH FROM date_purchase);

```
### Findings:
It is not possible to create a recurring pattern in sales due to the fact that first, the data is incomplete. Second, the data is limited to two years which makes it not viable for the report. For instance, while December 2022 shows strong performance, there’s no corresponding data for December 2023 to compare. Another example would be that on October 2022, a total sales of 30812 is recorded. On the other hand, on October 2023, the total sales were only 423. We can not say that it is a justifiable sale value because the majority of October 2023 has no data. It can only be viable for comparison if the whole data of October 2023 were present. Hence, there is a need for a more complete, multi-year data to identify recurring sales trends across the same months.

(Insert pic)

### 3. How do monthly sales trends differ by product category (e.g., tops, pants, accessories)?

Each items were not assigned to any product category. Therefore, i decided to categorized these purchased items.
- **Outwear** (Jacket, Trench Coat, Coat, Raincoat, Blazer, Poncho, Kimono)
- **Tops** (T-Shirt, Tank Top, Blouse, Flannel Shirt, Polo Shirt, Tunic, Camisole, Sweater, Hoodie, Cardigan, Vest)
- **Bottoms** (Jeans, Pants, Trousers, Shorts, Leggings, Skirt)
- **One-Piece Outfits** (Dress, Jumpsuit, Romper, Overalls, Pajamas, Onesie)
- **Footwear** (Sneakers, Boots, Sandals, Flip-Flops, Loafers, Slippers)
- **Accessories** (Hat, Sun Hat, Gloves, Scarf, Tie, Bowtie, Sunglasses, Belt, Umbrella)
- **Bags and Wallets** (Handbag, Backpack, Wallet)
- **Swimsuit** (Swimwear)

See full SQL [here](/Fashion_project/Product_Category_Trends)

(insert graph)

### Findings:

### 4. Which fashion items perform best during specific seasons?

The items were also not categorized by seasons, therefore i utilized CTE's to represent each season. We will then analyze fashion retail trends using timestamped transaction data.
- **SpringSummer Season** (January to June)
- **WinterFall Season** (July to December)
- **ResortCruise Season** (November to January)
- **Pre-Fall Season** (May to July)

See full SQL [here](/Fashion_project/Fashion_Items_Performance)

### Findings:

**Top Performing Items per Season**
- **SpringSummer Season** - Jeans, Slippers, and Trench Coat
- **WinterFall Season** (Tunic, Shorts, and Flip-Flops)
- **ResortCruise Season** (Sweater, Tunic, Bowtie)
- **Pre-Fall Season** (Jeans, Loafers, and Belt)

### SpringSummer Season
Jeans and slippers can reflect the stapleness in transitioning to warmer weathers. However, due to the lack of data in location and climatic conditions, it is difficult to make sense as to why trenchcoat is trending despite the hot season. It might be possible that the data is taken from a tropical region where it is generally warm all year long.

### WinterFall Season 
If the data is taken from western regions where winter is present, then tunic, shorts, and flip-flops are an odd combinations when approaching to colder weathers. However, if the data was taken from eastern and middle-eastern tropical regions, then tunic, shorts, and flip-flops does make it seem understandable. The highest sale is Tunic, which is commonly worn by the people in Indian subcontinents like India, Pakistan, and Bangladesh, therefore, reinforcing the idea that the data may be taken possibly from these regions. Furthermore, flip-flops are a common footwear worn by these people. 







