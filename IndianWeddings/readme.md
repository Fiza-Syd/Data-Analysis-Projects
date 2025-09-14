A comprehensive database project for analyzing wedding costs, expenses, and vendor data to provide insights into wedding planning and budgeting.

## 📋 Project Overview

This project analyzes wedding expenses data to understand:
- Wedding cost patterns across different regions, cities, and wedding types
- Budget vs actual cost analysis
- Expense category breakdowns
- Vendor cost analysis
- Cost efficiency metrics (cost per guest)
- Regional and demographic cost variations

## 🗂️ Project Structure

```
├── _tables_creation.sql    # Database schema creation
├── data_loading.sql        # Data import scripts
├── wedding_data.sql        # Sample wedding data
├── vendor_data.sql         # Sample vendor data
├── Wed.sql                # Main analysis queries
├── expenses.csv           # Expense data (7,000+ records)
└── README.md              # This documentation
```

## 🗄️ Database Schema

### Tables

1. **weddings** - Main wedding information
   - `wedding_id` (INT, PRIMARY KEY)
   - `budget` (DECIMAL)
   - `city` (VARCHAR)
   - `city_tier` (VARCHAR) - TIER1, TIER2, TIER3
   - `guest_count` (INT)
   - `region` (VARCHAR) - North, South, East, West, Central
   - `wedding_type` (VARCHAR) - Traditional, Modern, Destination, Luxury, etc.
   - `year` (INT)

2. **expenses** - Wedding expense details
   - `expense_id` (INT, PRIMARY KEY)
   - `wedding_id` (INT, FOREIGN KEY)
   - `amount` (DECIMAL)
   - `category` (VARCHAR) - Venue, Catering, Jewelry, Clothing, etc.

3. **vendors** - Vendor service costs
   - `vendor_id` (INT, PRIMARY KEY)
   - `wedding_id` (INT, FOREIGN KEY)
   - `price` (DECIMAL)
   - `vendor_type` (VARCHAR) - PHOTOGRAPHER, FLORIST, MUSICIAN, etc.

## 🚀 Setup Instructions

### Prerequisites
- MySQL, PostgreSQL, or SQLite database
- Database management tool (MySQL Workbench, pgAdmin, etc.)

### Installation Steps

1. **Create Database Schema**
   ```sql
   -- Run the table creation script
   SOURCE _tables_creation.sql;
   ```

2. **Load Sample Data**
   ```sql
   -- Load wedding data
   SOURCE wedding_data.sql;
   
   -- Load vendor data
   SOURCE vendor_data.sql;
   ```

3. **Import Expense Data**
   ```sql
   -- For MySQL
   SOURCE data_loading.sql;
   
   -- For PostgreSQL, use COPY command
   -- For SQLite, use .import command
   ```

4. **Run Analysis**
   ```sql
   -- Execute the main analysis queries
   SOURCE Wed.sql;
   ```

## 📊 Key Analysis Features

### 1. Data Quality & Validation
- Null value detection
- Duplicate record identification
- Data cleaning and standardization
- Negative value correction

### 2. Cost Analysis
- **Total Cost Calculation**: Budget + Expenses + Vendor Costs
- **Cost per Guest**: Efficiency metric
- **Budget Variance Analysis**: Actual vs planned costs
- **Regional Cost Comparison**: North, South, East, West, Central

### 3. Categorical Analysis
- **Wedding Types**: Traditional, Modern, Destination, Luxury, Royal, Simple
- **City Tiers**: TIER1 (Metro), TIER2 (Major), TIER3 (Smaller cities)
- **Guest Categories**: Small (≤200), Medium (201-500), Large (>500)

### 4. Expense Breakdown
- **Top Expense Categories**: Venue, Catering, Jewelry, Photography, etc.
- **Vendor Analysis**: Photographer, Florist, Musician, Caterer, Decorator
- **Cost Distribution**: Percentage breakdown by category

### 5. Advanced Insights
- **Top 10 Most Expensive Weddings**
- **Cost Efficiency by Region and Type**
- **Budget Performance Analysis**
- **Year-over-Year Trends**
- **City-wise Cost Analysis**

## 📈 Sample Queries

### Most Expensive Weddings
```sql
SELECT 
    wedding_id, city, region, wedding_type, guest_count,
    budget, total_expenses, total_vendor_cost, total_cost,
    ROUND((total_cost / guest_count), 2) as cost_per_guest
FROM wedding_summary 
ORDER BY total_cost DESC 
LIMIT 10;
```

### Regional Cost Analysis
```sql
SELECT 
    region,
    COUNT(*) as wedding_count,
    ROUND(AVG(total_cost), 2) as avg_total_cost,
    ROUND(AVG(total_cost / guest_count), 2) as avg_cost_per_guest
FROM wedding_summary 
GROUP BY region 
ORDER BY avg_total_cost DESC;
```

### Budget Performance
```sql
SELECT 
    CASE 
        WHEN total_cost <= budget THEN 'Under Budget'
        WHEN total_cost <= budget * 1.1 THEN 'Within 10% of Budget'
        WHEN total_cost <= budget * 1.2 THEN 'Within 20% of Budget'
        ELSE 'Over Budget (>20%)'
    END as budget_status,
    COUNT(*) as wedding_count,
    ROUND(AVG((total_cost - budget) / budget * 100), 2) as avg_budget_variance_percent
FROM wedding_summary 
GROUP BY budget_status;
```

## 🎯 Key Insights

Based on the analysis, you can discover:

1. **Cost Patterns**: Which regions and wedding types are most expensive
2. **Budget Management**: How often weddings stay within budget
3. **Efficiency Metrics**: Cost per guest across different wedding characteristics
4. **Expense Distribution**: Which categories consume the most budget
5. **Vendor Impact**: How vendor costs compare to direct expenses
6. **Geographic Trends**: Cost variations across different city tiers

## 🔧 Customization

### Adding New Data
- Modify `wedding_data.sql` to add more wedding records
- Update `vendor_data.sql` for additional vendor information
- Import new CSV files using `data_loading.sql` as a template

### Extending Analysis
- Add new queries to `Wed.sql` for specific insights
- Create additional views for frequently used calculations
- Implement stored procedures for complex analysis

### Database Compatibility
- **MySQL**: Use `LOAD DATA INFILE` in `data_loading.sql`
- **PostgreSQL**: Use `COPY` command
- **SQLite**: Use `.import` command

## 📝 Data Notes

- **Sample Size**: 50 weddings with 7,000+ expense records
- **Geographic Coverage**: All major Indian regions and city tiers
- **Time Period**: 2023 data
- **Currency**: Indian Rupees (₹)
- **Data Quality**: Cleaned and validated for analysis

## 🤝 Contributing

To extend this project:
1. Add more wedding data for better statistical significance
2. Include additional expense categories
3. Add vendor performance metrics
4. Implement cost prediction models
5. Create visualization dashboards

## 📞 Support

For questions or issues:
- Check the SQL comments in each file
- Verify database compatibility
- Ensure proper data loading sequence
- Review query syntax for your specific database system

---

**Happy Analyzing! 🎉**

*This project provides a solid foundation for wedding cost analysis and can be extended for business intelligence, market research, or personal wedding planning insights.*

## DAY 1: https://medium.com/@syedfiza96/how-indian-wedding-expenses-evolved-2000-2025-6f089e34c9a2
## DAY 2: https://medium.com/@syedfiza96/how-indian-wedding-expenses-evolved-2000-2025-3c936c625ab6
