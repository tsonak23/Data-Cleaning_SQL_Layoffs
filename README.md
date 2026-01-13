<!-- ===================== BLUE INSTRUCTION BLOCK ===================== -->
<instructions>
  <div style="color:#1f6feb; font-weight:600; font-size:15px; line-height:1.6;">
    
    🔹 <b>Instructions</b><br/>
    1. Load the dataset into Power BI Desktop<br/>
    2. Open Power Query Editor<br/>
    3. Clean and transform the data<br/>
    4. Create calculated columns and DAX measures<br/>
    5. Build visuals and publish the report<br/>
    
  </div>
</instructions>

<!-- ===================== ORANGE CODE BLOCK ===================== -->
<codeblock>
  <div style="background-color:#0d1117; color:#f0883e; padding:14px; border-radius:10px; font-family:Consolas, monospace; font-size:14px;">
    
    SELECT product_category,<br/>
           SUM(sales_amount) AS total_sales<br/>
    FROM sales_data<br/>
    GROUP BY product_category<br/>
    ORDER BY total_sales DESC;<br/>
    
  </div>
</codeblock>
