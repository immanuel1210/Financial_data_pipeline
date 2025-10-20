# 💱 Financial Data Pipeline — Exchange Rate ETL Project

This project is part of my Data Engineering portfolio.  
It demonstrates how to build an **end-to-end ETL pipeline** that extracts, transforms, and loads financial data (foreign exchange rates) from a public API into a relational database for further analysis and visualization.

---

## 🚀 Project Overview
The goal of this project is to **automate the process of collecting currency exchange rate data**, clean and structure it properly, then store it into a database for analytics or reporting.

The project uses:
- 🧩 **Python** — for data extraction and transformation  
- 🌐 **ExchangeRate Host API** — as the data source  
- 🧮 **Pandas** — for data wrangling  
- 🗄️ **MySQL** — as the data warehouse  

---

## 🧠 ETL Workflow

### 1️⃣ **Extract**
Fetch live or historical exchange rate data from the ExchangeRate Host API.

```python
def extract_rate(base="IDR"):
    url = f"https://api.exchangerate.host/live?source={base}"
    response = requests.get(url)
    data = response.json()
    return data
```

### 2️⃣ **Transform**
Convert the raw JSON into a clean and structured pandas DataFrame.
- Reorder columns
- Convert data types
- Add usefull metadata (Created_at, "Base_Currency)
    
```python
def transform_rate(data):
    rates = data["quotes"]
    df = pd.DataFrame(rates.items(), columns=["Target_Currency", "Exchange_Rate"])
    df["Base_Currency"] = data["source"]
    df["Date"] = datetime.utcfromtimestamp(data["timestamp"]).strftime("%Y-%m-%d %H:%M:%S")
    df["Created_At"] = datetime.now()
    return df
```

### 3️⃣ Load
Store the transformed dataset into a MySQL database for long-term storage and analysis.

```python
def load_to_mysql(df):
    engine = create_engine("mysql+pymysql://root:root@localhost:3306/currency")
    df.to_sql("exchange_rate", con=engine, if_exists="append", index=False)
    print("✅ Data successfully loaded into MySQL!")
```

## ⚙️ Tools

| Category                | Tools                 |
| ----------------------- | --------------------- |
| Programming             | Python 3, Pandas      |
| Data Source             | ExchangeRate Host API |
| Database                | MySQL                 |
| Version Control         | Git & GitHub          |


## 🔄 Future Improvements

 - Add daily scheduler using Apache Airflow
 - Integrate visualization with Power BI or Tableau
 - Store data in cloud database (AWS RDS / Google Cloud SQL)
 - Automate API error handling and logging


## 🧑‍💻 Author
Immanuel L.P. Simarsoit
**💡 Data Enthusiast | Data Analyst | Aspiring Data Engineer**  
Passionate about transforming raw data into meaningful insights.
