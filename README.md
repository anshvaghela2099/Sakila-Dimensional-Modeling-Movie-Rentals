# Sakila Dimensional Modeling - Movie Rentals  

## Project Overview  
This project involves transforming the **Sakila OLTP** database model into a **Dimensional Model** for analytical purposes. The **dimensional model** is designed using **ER Studio or Navicat**, and follows the principles covered in **Week 4** of the course.  

---

## Objectives  
- Convert the **Sakila OLTP database** into a **Dimensional Model**.  
- Implement **Slowly Changing Dimension Type 2 (SCD Type 2)** for **Customer Dimension**.  
- Optionally, introduce the concept of **Durability Key**.  
- Submit the **ER Diagram (.DM1 file)** and a **PDF version** of the model.  

---

## Files in this Repository  
| File Name | Description |
|-----------|------------|
| `Sakila_Dimensional_Model_Ansh_Vaghela.pdf` | PDF version of the Dimensional Model |
| `Sakila_Dimensional_Model.DM1` | ER Studio / Navicat Dimensional Model File |

---

## **Dimensional Model Overview**  

### **Fact Table: Fact_Rental**
The **Fact_Rental** table contains transactional data about movie rentals, with foreign keys linking to dimension tables.

| Column Name | Type | Description |
|-------------|------|-------------|
| `Rental_ID` | PK | Unique identifier for the rental transaction |
| `Customer_SK` | FK | Surrogate Key referencing **Dim_Customer** |
| `Date_SK` | FK | Surrogate Key referencing **Dim_Date** |
| `Film_SK` | FK | Surrogate Key referencing **Dim_Film** |
| `Store_SK` | FK | Surrogate Key referencing **Dim_Store** |
| `Amount` | Numeric | Amount paid for the rental |
| `Rental_Date` | Date | Date of rental |
| `Return_Date` | Date | Date of return |

---

### **Dimension Tables**  

#### **Dim_Date** (Fixed Dimension)  
Stores date-based attributes for easy time-series analysis.

| Column Name | Type | Description |
|-------------|------|-------------|
| `Date_SK` | PK | Surrogate Key for the date |
| `Year` | Int | Year value |
| `Quarter` | Int | Quarter (1-4) |
| `Month` | Int | Month (1-12) |
| `Week` | Int | Week number |
| `Day` | Int | Day of the month |
| `Date` | Date | Actual date value |

---

#### **Dim_Customer** (Type 2 Slowly Changing Dimension)  
Stores customer details and tracks changes over time.

| Column Name | Type | Description |
|-------------|------|-------------|
| `Customer_SK` | PK | Surrogate Key for the customer |
| `Customer_ID` | Int | Business Key from OLTP system |
| `First_Name` | String | Customer's first name |
| `Last_Name` | String | Customer's last name |
| `Create_Date` | Date | Date when the customer was created |
| `Is_Active` | Boolean | Whether the record is currently active |
| `Effective_Start_Date` | Date | Start date of this record version |
| `Effective_End_Date` | Date | End date of this record version |

**Type 2 Implementation:** Whenever a customer’s details change, a new row is inserted with an updated **Effective_Start_Date and Effective_End_Date**, ensuring historical tracking.

---

#### **Dim_Film** (Fixed Dimension)  
Stores film-related attributes.

| Column Name | Type | Description |
|-------------|------|-------------|
| `Film_SK` | PK | Surrogate Key for the film |
| `Film_ID` | Int | Business Key from OLTP system |
| `Last_Update` | Timestamp | Last update timestamp |
| `Rating` | String | Film rating (G, PG, R, etc.) |
| `Rental_Duration` | Int | Rental duration in days |
| `Rental_Rate` | Numeric | Rental rate of the film |

---

#### **Dim_Store** (Fixed Dimension)  
Stores store-related details.

| Column Name | Type | Description |
|-------------|------|-------------|
| `Store_SK` | PK | Surrogate Key for the store |
| `Store_ID` | Int | Business Key from OLTP system |

---

## **Dimensional Modeling Approach**  

**OLTP to OLAP Conversion**  
- The **Sakila OLTP model** consists of **normalized tables**, optimized for transactions.  
- The **Dimensional Model** (Star Schema) is **denormalized**, improving analytical performance.  

**Surrogate Keys Usage**  
- Each dimension has a **Surrogate Key (SK)** instead of using natural keys from OLTP.  
- This ensures consistency and improves query performance.  

**SCD Type 2 Implementation**  
- The **Customer Dimension** maintains **historical changes** using `Effective_Start_Date` and `Effective_End_Date`.  

**Fact-Dimension Relationships**  
- Fact table uses **foreign keys** (`Customer_SK`, `Film_SK`, `Date_SK`, `Store_SK`) to join with dimensions.  

---

## **Tools Used**
- **ER Studio / Navicat** (for Dimensional Model Design)
- **Sakila Database (MySQL)** (for OLTP Reference)
- **SQL** (for Querying and Data Transformation)

---

---

## **Key Learnings**
**Dimensional modeling optimizes analytical queries**.  
**SCD Type 2 helps maintain historical data**.  
**Surrogate keys improve performance over natural keys**.  
**Denormalization increases query efficiency in OLAP systems**.  

---

## **Authors**
**Ansh Vaghela**  
[LinkedIn](https://www.linkedin.com/in/anshv1)  
