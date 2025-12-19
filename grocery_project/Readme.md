Grocery ERP System
Grocery ERP System is a streamlined web-based solution for managing grocery store networks. The system allows owners and managers to track sales, manage product catalogs by categories, monitor operational expenses, and analyze financial performance (Revenue and Net Profit) in real-time.

Developed to provide a clean, efficient, and user-friendly interface for small to medium-sized retail businesses.

Tech Stack
Backend: Python 3.10+, Flask (Web Framework)

Database: PostgreSQL (DBMS), Psycopg2 (Database Adapter)

Frontend: HTML5, CSS3, Bootstrap 5, FontAwesome Icons

Architecture: SQL Views for advanced real-time financial analytics and KPI calculation.

Key Features
Multi-Branch Management: Seamlessly switch between different store locations via session-based state. All transactions and expenses are automatically linked to the active branch.

Financial KPI Dashboard: Automatic calculation of Revenue, Cost of Goods Sold (COGS), Other Expenses, and Net Profit specifically for the current month using PostgreSQL DATE_TRUNC logic.

Catalog Management (Admin Panel):

Create and manage product categories.

Register products with SKU, purchase price, and retail price.

Real-time inventory tracking (Stock-in/Stock-out).

Point of Sale (POS) Interface: Interactive shopping cart with stock availability checks and automatic inventory deduction upon checkout.

Installation & Setup
1. Clone the Repository
Bash

git clone https://github.com/AZADEVV/database/tree/main/grocery_project
cd grocery-erp
2. Configure Environment
Ensure you have PostgreSQL installed and running. Create a virtual environment and install the required packages:

Bash

# Create virtual environment
python -m venv venv
source venv/bin/activate  # On Windows use: venv\Scripts\activate

# Install dependencies
pip install flask psycopg2-binary
3. Database Initialization (pgAdmin 4)
It is recommended to initialize the database manually:

Open pgAdmin 4 and create a new database named grocery_db.

Open the Query Tool for grocery_db.

Execute the schema.sql file to create tables for stores, products, categories, orders, and expenses.

Important: Create the Financial KPI View to enable the dashboard functionality:

SQL

CREATE OR REPLACE VIEW kpi AS
SELECT 
    s.store_id, s.store_name,
    ROUND(COALESCE(SUM(CASE WHEN DATE_TRUNC('month', o.order_timestamp) = DATE_TRUNC('month', CURRENT_DATE) THEN oi.quantity * oi.unit_price ELSE 0 END), 0)::numeric, 2) as total_revenue,
    -- (Add other calculations for cost and expenses here)
FROM stores s
LEFT JOIN orders o ON s.store_id = o.store_id
...
GROUP BY s.store_id, s.store_name;
Running the Project
Run the Flask application from your terminal:

Bash

python app.py
The website will be available at: http://127.0.0.1:5000

User Flow
Select Branch: Use the navigation bar to pick a store location. The system will "remember" this branch for all sales and expenses.

Populate Catalog: Go to the Management section to create categories and add products with their respective prices and initial stock.

Process Sales: On the Showcase page, add items to the cart and click "Checkout". The system will update the database instantly.

Review Analytics: Visit the Dashboard to see the financial health of the selected branch for the current month.

Database Structure
stores – Management of physical locations.

products – Product data (Name, SKU, Prices, Stock).

categories – Product grouping.

orders & order_items – Sales transaction records.

expenses – Operational costs (Rent, Utilities, Salaries).

Built for efficient retail management and data-driven decision making.