# 🏦 Bank Customer Segmentation

## 📌 Project Overview

In this project, I performed **unsupervised customer segmentation** for a bank using transactional and demographic data. The objective was to discover natural customer groups based on their behavioral patterns and value to the business. These insights will guide **personalized marketing**, **retention strategies**, **loyalty programs**, and **product development**.

---
## 🧭 Business Objective

Banks serve a broad and diverse customer base. However, treating all customers the same is inefficient and costly. By grouping customers based on actual behaviors, such as **how often** they transact, **how recently**, and **how much** they spend, can help the bank:

- Prioritize high-value customers for loyalty initiatives  
- Reactivate dormant customers through targeted engagement  
- Design and recommend tailored financial products (e.g., savings plans, credit cards)  
- Improve campaign ROI through **data-driven personalization**

---

## 🧾 Code and Resources Used

**Python Version**: 3.8

**Libraries**:
- `pandas`, `numpy`, `matplotlib`, `seaborn`
- `scikit-learn`
- `scipy`
- Segmentation Approach: https://www.researchgate.net/publication/328533235_A_customer_segmentation_approach_in_commercial_banks

---

## 📂 Dataset Description

The dataset consists of **1 million+ transactions** from over **800,000 customers** of a bank in India. It includes rich customer-level information such as:

- **Customer date of birth**
- **Gender**
- **Location**
- **Account balance**
- **Customer ID**
- **Transaction timestamp**
- **Transaction amount**
- **Transaction ID**

---

## 🧹 Data Cleaning & Feature Engineering

- Filtered out rows with missing or invalid values in key fields such as `TransactionAmount`, `TransactionTime`, and `CustomerID`
- Removed outliers above the 99th percentile in `TransactionAmount` and `AccountBalance`
- Combined the `TransactionDate` and `TransactionTime` to create the `Transaction Timestamp` column
- Applied **log transformation** to `TransactionAmount` and `AccountBalance` to reduce skewness and stabilize variance
- Created customer-level **RFM features**:
  - `Recency`: Days since the customer's most recent transaction
  - `Frequency`: Count of transactions per customer
  - `AvgMonetary`: Average spend per transaction
  - `TotalMonetary`: Total transaction amount per customer
- Computed `Age` from customer date of birth
- Handled wrongly imputed values in `CustDOB` using mode imputation

---

## 📊 Exploratory Data Analysis (EDA)

The EDA phase helped uncover important patterns in customer behavior and laid the foundation for segmentation. Key steps included:

- **Descriptive Statistics**: Examined distributions of Age, Recency, Frequency, and Monetary features to understand data spread and central tendency.
- **Outlier Detection**: Identified and visualized outliers in age.
- **Correlation Analysis**: Used heatmaps to inspect relationships between numerical features. Frequency and total spend showed moderate to strong correlations.
- **RFM Feature Insights**:
  - Customers with high frequency often had higher total spending
  - Recency was inversely related to customer engagement
- **Visualizations**: 
  - Bar plots, histograms, and pair plots helped summarize customer distributions and detect patterns.
 
<img width="2167" height="1590" alt="correlation_heatmap" src="https://github.com/user-attachments/assets/d7399bf1-804d-49bf-a1a6-202d2f7720c3" />

<img width="3549" height="2359" alt="distribution_of_numeric_features (1)" src="https://github.com/user-attachments/assets/73cb625f-334d-40dc-a3fc-3f398589479e" />

<img width="1803" height="1375" alt="customer_gender_distribution" src="https://github.com/user-attachments/assets/d4c4ba1d-5aec-47a1-821f-fc34ed6777f6" />

<img width="1874" height="1394" alt="top_10_locations_by_customer_count" src="https://github.com/user-attachments/assets/10a7a458-2b46-42f5-8720-f2d629ef2d9e" />

<img width="1701" height="1375" alt="total_monetary_spend_by_gender" src="https://github.com/user-attachments/assets/98ce6245-988f-4960-bd4c-ddf714aa423c" />


📌 *These insights guided feature selection and confirmed the viability of clustering as a segmentation technique.*

---

### 🎯 Customer Segmentation KMeans (k = 4)

This section presents insights from clustering analysis performed using the K-Means algorithm on standardized customer features.
I selected **KMeans** for its:

- Simplicity and speed  
- Compatibility with Euclidean-based clustering assumptions  
- Clear output (cluster labels and centroids)  
- Compatibility with PCA for visualization

<img width="579" height="467" alt="image" src="https://github.com/user-attachments/assets/48e5d484-8fb8-4fbc-866d-20eafb113fb2" />


---

### 📊 Clustering Summary

I applied K-Means clustering with `k=4`(identified in the elbow plot above) on scaled features from the customer profile dataset. The segmentation was visualized using two feature pairs:

- **AvgMonetary vs Recency**
- **TotalMonetary vs Frequency**

These visualizations allowed me to identify key customer groups and assign business-friendly labels to each cluster.

---

### 🔍 1. AvgMonetary vs Recency (Cluster Interpretation)

<img width="605" height="465" alt="image" src="https://github.com/user-attachments/assets/4bc8555a-2d09-49ba-a2c4-0dbe1eab30d5" />


| Cluster Label            | Characteristics                                  | Business Strategy                          |
|--------------------------|--------------------------------------------------|--------------------------------------------|
| 🟢**Champions**            | Recent transactions, high avg spend              | Priority for loyalty & premium services    |
| 🟡**Potential Loyalists**  | Moderate recency and spending                    | Target with engagement campaigns           |
| 🔵**At-Risk High Spenders**| Low recency, but high past spending              | Retention strategy, reactivation offers    |
| 🔴**Low-Value Dormant**    | Long time since activity, low avg spend          | Low priority or churn segment              |

This segmentation aligns closely with RFM-based logic and business-focused strategies from the reference segmentation paper.

---

### 🔍 2. TotalMonetary vs Frequency (Cluster Interpretation)

<img width="604" height="466" alt="image" src="https://github.com/user-attachments/assets/86d684db-1c05-4b6b-952b-dfef1b46609d" />


| Cluster Label            | Characteristics                                  | Business Strategy                          |
|--------------------------|--------------------------------------------------|--------------------------------------------|
| 🟢**High-Value Loyalists** | Frequent transactions, high total spend          | VIP treatment, reward programs             |
| 🟡**Casual Spenders**      | Low frequency, moderate spend                    | Cross-sell or upsell campaigns             |
| 🔵**Super Users**          | Very frequent and very high spend                | Strategic partners, personalized outreach  |
| 🔴**One-Time Users**       | Very low frequency and low total spend           | Acquisition focus, low ROI group           |

Although this plot showed less clear visual separation, it still supported the customer tiers derived from the first plot.

---

### 💡 Recommendation

The first clustering view (`AvgMonetary vs Recency`) provided clearer and more business-actionable clusters. These will guide personalized marketing strategies, retention initiatives, and cross-sell opportunities.

---

### 🧠 Principal Component Analysis (PCA) Interpretation

#### 📊 Explained Variance

Using PCA, I reduced the five scaled numeric features — `Age`, `Recency`, `Frequency`, `AvgMonetary`, and `TotalMonetary` — to two principal components for visualization. Here's how much variance is captured:

- **PC1** explains **41.2%**
- **PC2** explains **24.2%**
- **Total Explained Variance** = **65.4%**

<img width="2955" height="2056" alt="pca_clusters" src="https://github.com/user-attachments/assets/5500340e-4bbc-4b23-abb4-36fe7eb9faba" />


This means these two components preserve a good amount of the structure in our data and are suitable for 2D visualization of customer segments.


#### 🔎 Component Loadings

| Feature         | PC1     | PC2     |
|-----------------|---------|---------|
| Age             | 0.1432  | -0.5838 |
| Recency         | -0.2087 | -0.4024 |
| Frequency       | 0.5812  | 0.3719  |
| AvgMonetary     | 0.3609  | -0.5991 |
| TotalMonetary   | 0.6841  | -0.0005 |

- **PC1** is dominated by `TotalMonetary`, `Frequency`, and `AvgMonetary`, representing **Customer Value**.
- **PC2** contrasts `AvgMonetary` and `Age` (negative loadings) against `Frequency`, indicating **Spending Behavior vs. Frequency**.

#### 📌 Business Interpretation of PCA Clusters

Using KMeans clustering on the scaled features and projecting them in PCA space, I identified **4 customer segments**. Here's what each segment likely represents based on the PCA structure and loadings:

---

### 🧩 Cluster Profiles

| Cluster | Description                  | Characteristics                                                                 |
|---------|------------------------------|----------------------------------------------------------------------------------|
| **0 - Loyal Big Spenders**      | High Value & Moderate Age         | High `TotalMonetary` and `Frequency`, moderate `Age`, slightly low `Recency`. These customers buy often and spend big. |
| **1 - High-Potential Newcomers**| Recent, Young & Big Spenders      | Younger customers with recent purchases and high `AvgMonetary`. Could be early loyalists. |
| **2 - At-Risk or Inactive**     | Low Value, Aging                  | Older customers with low frequency and spending. Long recency implies possible churn. |
| **3 - Mass of Low-Value Customers** | Infrequent, Recent Buyers      | Largest segment. Low frequency, low spending, and moderate recency. Potential to upsell or engage further. |

#### 💡 Strategic Opportunities

- **Target Cluster 1** with loyalty programs and upselling campaigns.
- **Re-engage Cluster 2** with win-back offers or feedback surveys.
- **Nurture Cluster 3** using promotions to convert them into loyal spenders.
- **Reward Cluster 0** with VIP benefits or referrals to maintain engagement.

---

### ✅ Statistical Validation with ANOVA

To validate that my clusters were statistically distinct across key features, I conducted **ANOVA (Analysis of Variance)** on each numeric variable. All variables showed statistically significant differences (p-value < 0.05), confirming the relevance of the clustering.

<img width="271" height="199" alt="image" src="https://github.com/user-attachments/assets/f8b089be-0709-42cf-a0dc-c131a2f378ee" />

---

### 📦 Feature Distribution Analysis by Cluster
I used box plots to visually inspect the distribution of each key feature across clusters
### 📊 Cluster Interpretations Based on Feature Distributions

#### 1. **Age by Cluster**

<img width="2353" height="1454" alt="Age_by_cluster" src="https://github.com/user-attachments/assets/f1aee6be-31cd-467d-a8c4-b038071f2405" />

* **Cluster 0 & 3**: Customers here have the **highest median age**, suggesting these might be older or more mature customer segments.  
* **Cluster 1**: Slightly younger than Clusters 0 and 3.  
* **Cluster 2**: Contains the **youngest customers** overall. This could include students or early-career individuals.

> **Insight**: Age could correlate with purchasing power or transaction behavior. Younger clusters may exhibit more volatile or developing financial patterns.

---

#### 2. **AvgMonetary by Cluster**

<img width="2353" height="1454" alt="AvgMonetary_by_cluster" src="https://github.com/user-attachments/assets/a806ddc2-a487-405f-aefe-97f16df57524" />

* **Cluster 0**: Has the **highest average monetary value**, indicating **high-value customers** who spend more per transaction.  
* **Cluster 3**: Close to Cluster 0, but slightly lower in average transaction value.  
* **Cluster 1**: Moderate average spenders.  
* **Cluster 2**: **Lowest average monetary value**, possibly **budget-conscious or low-value** customers.

> **Insight**: Clusters 0 and 3 are the premium spenders. The bank might prioritize them for **exclusive deals or loyalty programs**.

---

#### 3. **Frequency by Cluster**

<img width="2353" height="1454" alt="Frequency_by_cluster" src="https://github.com/user-attachments/assets/0df4fd37-c6c9-4880-b1b4-e774eda21468" />

* **Cluster 3**: Has a notably higher **frequency of transactions**, even with a few customers reaching up to 6.  
* **Clusters 0, 1, 2**: Predominantly have a frequency of 1, with **minimal repeat purchases**.

> **Insight**: Cluster 3 contains the **most loyal or engaged customers**. These are prime candidates for **retention strategies or upsell opportunities**.

---

#### 4. **Recency by Cluster**

<img width="2353" height="1454" alt="Recency_by_cluster" src="https://github.com/user-attachments/assets/cf9da950-c73e-423a-8cdb-a5f2c3d34d17" />

* **Cluster 1**: Has the **highest recency values**, meaning these customers haven’t transacted in a long time. They are likely **inactive or at-risk** of churn.  
* **Clusters 0, 2, and 3**: Have **lower recency**, indicating **more recent engagement** with the bank.

> **Insight**: Customers in Cluster 1 require **re-engagement campaigns**, while the others can be nurtured through **continued engagement or personalized offers**.

---

#### 5. **TotalMonetary by Cluster**

<img width="2353" height="1454" alt="TotalMonetary_by_cluster" src="https://github.com/user-attachments/assets/1fdb5a4d-3ea8-42b2-8478-04572e0bdbce" />

* **Cluster 3**: Clearly stands out with **significantly higher total monetary values** — these are the **top revenue-generating customers**.  
* **Clusters 0 and 1**: Show moderate total monetary values.  
* **Cluster 2**: Has the **lowest total value**, possibly infrequent, low-spending customers.

> **Insight**: Cluster 3 deserves **premium loyalty benefits**, **VIP programs**, or **cross-sell opportunities**. Cluster 2 could be targeted with **entry-level or incentive-based offers** to increase engagement.

---

Thanks for checking out this project!  
Feel free to reach out if you’d like to collaborate or ask questions about the methodology or insights.


