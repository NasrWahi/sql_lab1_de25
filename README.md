# sql_lab1_de25

# Lab 1 - SQL (DVD Rental Analysis) 

This lab has SQL and Duckdb analysis, working with DVD rental data

# Files
- `duckdb_pandas.ipynb`: Jupyter notebook with DuckDB and Pandas analysis of DVD rental data
- `data/sakila.duckdb`
- `data/sqlite-sakila.db`
- `sql/load_sakila.sql`
- `README.md`

# What the analysis can do:
- Find movies longer than 3 hours (title and length)
- Find movies with only "love" in its title (title, rating, length, description)
- Calculate movie length statistics (shortest, average, median, longest)
- Find 10 most expensive movies to rent per day
- Find the actors who played in the most movies
- Create custom analysis (film categories, ratings, actor collaborations)
- Show top 5 customers by total spend (bar chart)
- Show revenue per film category (bar chart)

# Data Source
Original Sakila sample database from Kaggle: https://www.kaggle.com/datasets/atanaskanev/sqlite-sakila-sample-database

# Examples
```python
import duckdb

# Connect to database
duckdb_path = "data/sakila.duckdb"

# Example: Task 1a | Movies longer than 3 hours
task1a = duckdb.sql(
    ---
    SELECT title, length
    FROM film
    WHERE length > 180
    ---
).df()

print("=== Task 1a: Movies longer than 3 hours ===")
print(f"Found {len(task1a)} movies")
task1a.head(10)

# Example: Task 2a | Top 5 customers by total_spend
task2a = duckdb.sql(
    ---
    SELECT 
        c.customer_id,
        c.first_name || ' ' || c.last_name as customer_name,
        SUM(p.amount) as total_spend
    FROM customer c
    JOIN payment p ON c.customer_id = p.customer_id
    GROUP BY c.customer_id, c.first_name, c.last_name
    ORDER BY total_spend DESC
    LIMIT 5
    ---
).df()

task2a