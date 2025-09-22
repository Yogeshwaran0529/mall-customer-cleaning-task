Dataset Info

This project involves cleaning and preprocessing the Mall Customer Segmentation Data. The original dataset contains information about customers, including their gender, age, annual income, and spending score.

Columns: 5

Rows: 200

Main Features: CustomerID, Gender, Age, Annual Income (k$), Spending Score (1-100)

✅ Cleaning Steps
1. 🧭 Initial Exploration

Used .shape and .info() to inspect the dataset.

Confirmed there were no missing values.

Checked data types and column names.

2. 🗑 Removed Duplicates
df.duplicated().sum()


Found 0 duplicate rows, so no removal was needed.

3. 🧼 Column Name Cleaning

Converted all column names to lowercase:

df.columns = df.columns.str.lower()


Renamed columns for consistency:

df.rename(columns={
    "customerid": "customer_id",
    "annual income (k$)": "annual_income($k)",
    "spending score (1-100)": "score"
}, inplace=True)

4. 🧹 Cleaned Text Values

Cleaned the only object column gender:

df["gender"] = df["gender"].str.strip()


Verified gender values with .value_counts() — only two unique, clean categories: Male and Female.

5. 💲 Added New Feature

Created a new column for Annual Income in $:

df["annual_income"] = df["annual_income($k)"] * 1000

📊 Exploratory Data Analysis (EDA)
1. 🔢 Basic Aggregations
df["age"].agg(["min", "max"])

2. 📈 Gender-Based Aggregation
df.groupby("gender").agg({
    "annual_income($k)": ["count", "mean", "sum"],
    "score": ["mean", "max"]
})

3. 🎯 Age Group Segmentation

Created an age_group feature using a custom function:

def age_group(age):
    if age <= 30:
        return "18-30"
    elif age <= 50:
        return "31-50"
    elif age <= 70:
        return "51-70"
    else:
        return "+70"


Applied it:

df["age_group"] = df["age"].apply(age_group)

4. 📊 Pivot Tables

Explored age group vs. gender using pivot tables:

df.pivot_table(values="annual_income($k)", index="age_group", columns="gender", aggfunc="sum")
df.pivot_table(values="annual_income($k)", index="age_group", columns="gender", aggfunc=["min", "max",
