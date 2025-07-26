# Data_Analysis_Project

I developed a personal finance tracker and spending pattern analyzer using SQL, Excel, and Power BI. Includes 3 years of simulated data with visual insights on income, expenses, savings, and trends for better financial understanding and decision-making.


## Dataset Generation
Data is generated using SQL Server from 01-Jan-2022 to 31-Dec-2024.
Monthly income, fixed & variable expenses, and savings are recorded.
Categories include Rent, Groceries, EMI, SIP, Medical, Outings, etc.
Salary increases by 20% every year (to simulate realistic income growth).



## Tech Stack
SQL Server – Dataset creation & data management
Microsoft Excel – Data cleaning & pre-processing
Power BI – Data analysis & dashboard visualization



## CLIENT’S REQUIREMENTS: FINAL 
VISUAL DASHBOARD :-
Overall Financial Snapshot
 • I want a clear top summary section with KPI Cards showing:
 • Total Income (3 years & yearly)
 • Total Expenses (3 years & yearly)
 • Total Savings (Income – Expenses)
 • Average Monthly Savings
 
Yearly & Monthly Trends
 • I want line charts for:
 • Monthly Income vs Expenses vs Savings (3-year timeline)
 • Year-over-Year Income & Expense Comparison (bar chart)
️
Category Breakdown
I want to know where my money goes:
 • Pie or Donut Chart: % share of each Category (Rent, Groceries, Outing, 
    Travel, EMI, SIP, Medical, etc.)
 • Stacked Bar Chart: Payment Mode by Year
 ️
Drill Down & Details
I want to:
 • Click on a category → see detailed transactions for that category.
 • Have a table with all transactions: Date, Amount, Type, Category, 
    Subcategory, Remarks, Payment Mode.
 ️
Filters
I need simple filters/slicers:
 • Year Selector
 • Month Selector
 • Category Selector
 ️
Visual Style
Make it:
 • Clean, simple, modern
 • Use Blue for income, Royal Blue for expenses, Orange for savings
 • Add clear titles and tooltips

## MY EXECUTION PLAN

## Cleaning, Analyzing and Visualizing data using SQL Query

select sum(Amount) as Total_Income from PersonalFinance
where Type = 'Income';
 

select sum(Amount) as Total_Expenses from PersonalFinance
where Type = 'Expense';
 

select sum(Amount) as Total_Savings from PersonalFinance
where Type = 'Saving';
 
select (SUM(Amount)/ (Count(distinct(month))*count(distinct(year)))) as Avg_Monthly_Savings from PersonalFinance
where Type = 'Saving';
 

SELECT 
    YEAR(Date) AS Year,
    MONTH(Date) AS Month,
    SUM(CASE WHEN Type = 'Income' THEN Amount ELSE 0 END) AS Monthly_Income,
    SUM(CASE WHEN Type = 'Expense' THEN Amount ELSE 0 END) AS Monthly_Expenses,
    SUM(CASE WHEN Type = 'Saving' THEN Amount ELSE 0 END) AS Monthly_Savings
FROM PersonalFinance
GROUP BY YEAR(Date), MONTH(Date)
ORDER BY Year, Month;
 

SELECT
    YEAR(Date) AS Year,
    SUM(CASE WHEN type = 'Income' THEN Amount ELSE 0 END) AS Yearly_Income,
    SUM(CASE WHEN type = 'Expense' THEN Amount ELSE 0 END) AS Yearly_Expense
FROM PersonalFinance
GROUP BY Year(Date)
ORDER BY Year;
 

SELECT
    Category as category,
    sum(case when type != 'Income' THEN Amount END) as Total_Expenses from PersonalFinance
GROUP BY Category
ORDER BY Total_Expenses
 

SELECT
    Year(Date) as Year,
    Category as category,
    sum(case when type != 'Income' THEN Amount END) as Total_Expenses from PersonalFinance
GROUP BY Category,Year(Date)
ORDER BY Year,Total_Expenses
 
select 
    	Year,
   	Month,
    	sum(case when category in('Rent', 'emi', 'SIP') then Amount else 0 End) as Fixed_Expenses,
sum(case when category in('Utilities','Groceries','Outing','Medical' ) then Amount else 0 End) as  Variable_Expenses
from PersonalFinance
WHERE type = 'Expense'
Group by Year,Month
order by Year,Month
 

SELECT 
    COUNT(CASE WHEN RecurringFlag = 0 THEN TransactionID END) AS OneTimeExpense,
    COUNT(CASE WHEN RecurringFlag = 1 THEN TransactionID END) AS RecurringExpense
FROM PersonalFinance
WHERE Type = 'Expense';
 
SELECT
    Top 1
    category as category,
    sum(Amount) as Biggest_Expense_category
    from PersonalFinance
    where Type = 'Expense'
group by Category
order by Biggest_Expense_category DESC;
 

select
    top 1
    Year,
    Month,
    sum(Amount) as Month_with_Highest_Spending
    from PersonalFinance
    where Type = 'Expense'
Group by Year, Month
Order by Month_with_Highest_Spending;
 

SELECT 
  Year,
  Month,
  SUM(CASE WHEN Type = 'Income' THEN Amount ELSE 0 END) AS Total_Income,
  SUM(CASE WHEN Type = 'Expense' THEN Amount ELSE 0 END) AS Total_Expenses
FROM PersonalFinance
GROUP BY Year, Month
HAVING SUM(CASE WHEN Type = 'Expense' THEN Amount ELSE 0 END)  >  SUM(CASE WHEN Type = 'Income' THEN Amount ELSE 0 END)
ORDER BY Year, Month;
 
select
    DATENAME(month, date) as Month_name,
    count(distinct TransactionID) as Transaction_per_month
    from PersonalFinance
group by DATENAME(month, date)
 

select
    Category as Category,
    sum(Amount) as Total_Expenses,
    sum(Amount)*100/(select sum(Amount) from PersonalFinance  where month(date) = 1) as Amount_percentage
    from personalfinance
    where month(date) = 1                                     (Only For JANUARY)
Group by category
order by Total_Expenses
 

select
    paymentmode as mode_of_payment,
    sum(Amount)*100/(select sum(Amount) from PersonalFinance) as Percentage_of_Payment_mode
    from PersonalFinance
group by PaymentMode;
 


 ## Building the Dashboard
 
• Visuals:
 • KPI Cards: Total Income, Total Expenses, Net Savings, Avg Monthly Savings.
 • Table: All transactions, fully filterable.
 • Bar Chart: Year-over-Year comparison.
 • Line Chart: Yearly Income vs Expenses vs Savings.
 • Stacked Bar Chart: Payment Mode by Year
 • Clustered Bar: Number of Transaction by Category
 • Pie/Donut: % of Amount by Category.
 • Pie/Donut: Transaction by Year
 
Filters:
 • Year
 • Month
 • Category
 
Drill-Through:
 • Click category → see detailed transactions.
 ️
Design & Interactivity
 • Consistent colors: Blue (Income), Royal Blue (Expenses), Orange (Savings).
 • Clear labels, tooltips, legends.
 • Tested for easy filtering.









