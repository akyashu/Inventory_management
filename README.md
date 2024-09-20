# **Data Loading and Preprocessing**
This section loads the necessary datasets from Google Drive using `pandas` and processes the initial data exploration:

- **Sales Data**: `SalesFINAL12312016.csv` loaded with parsed dates.
- **Purchase Data**: `PurchasesFINAL12312016.csv`.
- **Invoice Data**: `InvoicePurchases12312016.csv`.
- **End Inventory**: `EndInvFINAL12312016.csv`.
- **Beginning Inventory**: `BegInvFINAL12312016.csv`.
- **Purchase Prices**: `2017PurchasePricesDec.csv`.

The data is explored, and unique counts for relevant columns such as `InventoryId`, `Brand`, and `onHand` are calculated to check for potential inconsistencies.

---





# **Data Cleaning**
Some inconsistencies are identified, especially in the combination of `Description` and `Size`. The `Brand` column is also used to group inventory information. The assumption is made that `InventoryId` = `store_city_brand`, and the brand can represent the combination of `Description + Size`. These aspects might need cleaning.

---

# **Sales Forecasting**
A time-series model is built using the `SARIMAX` model from the `statsmodels` library to forecast sales quantities:

- **Grouping by SalesDate**: Aggregated total sales quantity per day.
- **Model Training**: An ARIMA model is trained using `(5, 1, 1)` as the order for the SARIMAX model.
- **Prediction**: The sales forecast is made for March 2016.
- **MAPE (Mean Absolute Percentage Error)** and **R²** are calculated to evaluate the performance of the model.
- **Visualization**: Forecast and actual sales are plotted using Plotly.

---
![image](https://github.com/user-attachments/assets/97dc4d3e-2d81-4501-9ab7-d329e4580de4)
# **Fourier Transformed Linear Regression**
To analyze trends and seasonality, a Fourier-transformed Linear Regression model is applied:

- The `CalendarFourier` method is used to incorporate seasonal trends.
- **Linear Regression Model**: This model fits the sales data using time-related features.
- **Forecasts** are filtered to include only positive values, and **MAPE** and **R²** are recalculated for these positive forecasts.

---
![image](https://github.com/user-attachments/assets/96572690-f644-4a33-9ea6-0f236656f4db)
# **ABC Analysis**
An **ABC Classification** is performed based on total sales quantities for each brand:

- The dataset is divided into three bins: 
  - **Group A**: SalesQuantity > 1000.
  - **Group B**: 100 <= SalesQuantity <= 1000.
  - **Group C**: SalesQuantity < 100.
  
- **Visualization**: A histogram is created showing the distribution of brands in each ABC group.

---
  
![image](https://github.com/user-attachments/assets/6348da0f-bd1d-46a9-8bb1-8294e4dda58e)
![image](https://github.com/user-attachments/assets/cc4e07f7-33cb-4de2-aab0-dcd87e6ea3c0)
# **Economic Order Quantity (EOQ)**
The EOQ formula is applied to the sales data to compute the optimal order quantity based on demand and costs:

- **Cost Per Order (CPO)**: Calculated by merging purchase and invoice data, considering freight costs and volume.
- **Holding Cost**: Assumed to be 30% of the purchase price, calculated for each brand.
- **EOQ Calculation**: The EOQ is derived using the formula \( EOQ = \sqrt{\frac{2DS}{H}} \), where D is demand, S is setup cost (CPO), and H is holding cost.

The calculated EOQ values are grouped by ABC classification and summarized.

---

# **Sales Velocity and Lead Time**
- **Sales Velocity**: The average daily unit sales for each brand are calculated using the total sales quantity divided by the number of days in the sales data.
- **Lead Time**: The average lead time for each brand is computed as the difference between the `ReceivingDate` and `PODate` from the purchases data.

---


![image](https://github.com/user-attachments/assets/01d90026-ce8c-43c0-b948-58e8b4dffec5)
# **Safety Stock and Reorder Point Analysis**
- **Safety Stock**: This is calculated as the difference between the maximum daily sales and the average daily sales for each brand. Negative values are set to zero to avoid illogical outcomes.
- **Reorder Point (ROP)**: The reorder point is calculated as \( ROP = (Lead Time \times Sales Velocity) + Safety Stock \). 

A histogram is created to visualize the distribution of reorder points.

---
![image](https://github.com/user-attachments/assets/48320ba6-84a7-4b12-b1ca-9f0323523379)
# **Inventory Value Analysis**
- **End Inventory Value**: The total value of inventory on hand is calculated using the `onHand` and `Price` columns from the `EndInv` dataset.
- **Reorder Point Impact**: The potential reduction in total inventory value is calculated if the reorder point levels are applied.

**Visualization**: A comparison between current total inventory value and projected values if the reorder point strategy is followed is shown using histograms.

---
![image](https://github.com/user-attachments/assets/7940cbf4-ca4f-4887-8aa7-8beb9f184341)
# **Conclusion**
The code provides a comprehensive analysis of inventory management using sales forecasting, EOQ calculation, ABC analysis, and reorder point analysis. The visualizations provide a clear understanding of the performance and inventory levels, aiding in better decision-making for stock management.
