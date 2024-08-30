# Additional Task: SQL and Big Data Adaptation

Consider the following complex SQL query that involves relationships between companies, their suppliers, and customers:

```sql
WITH supplier_customer_counts AS (
    SELECT 
        company_id,
        SUM(CASE WHEN relationship_type = 'supplier' THEN 1 ELSE 0 END) AS supplier_count,
        SUM(CASE WHEN relationship_type = 'customer' THEN 1 ELSE 0 END) AS customer_count
    FROM company_relationships
    GROUP BY company_id
),
top_relations AS (
    SELECT 
        cr.company_id,
        cr.related_company_id,
        cr.relationship_type,
        c.name AS related_company_name,
        ROW_NUMBER() OVER (
            PARTITION BY cr.company_id, cr.relationship_type 
            ORDER BY t.total_value DESC
        ) AS rank
    FROM company_relationships cr
    JOIN companies c ON cr.related_company_id = c.id
    JOIN transactions t ON (
        (cr.company_id = t.from_company_id AND cr.related_company_id = t.to_company_id) OR
        (cr.company_id = t.to_company_id AND cr.related_company_id = t.from_company_id)
    )
    WHERE t.transaction_date >= DATE_SUB(CURDATE(), INTERVAL 1 YEAR)
    GROUP BY cr.company_id, cr.related_company_id, cr.relationship_type, c.name
)
SELECT 
    c.id AS company_id,
    c.name AS company_name,
    scc.supplier_count,
    scc.customer_count,
    sup.related_company_name AS top_supplier,
    cus.related_company_name AS top_customer,
    COALESCE(sup_trans.total_value, 0) AS top_supplier_transaction_value,
    COALESCE(cus_trans.total_value, 0) AS top_customer_transaction_value
FROM companies c
LEFT JOIN supplier_customer_counts scc ON c.id = scc.company_id
LEFT JOIN top_relations sup ON c.id = sup.company_id AND sup.relationship_type = 'supplier' AND sup.rank = 1
LEFT JOIN top_relations cus ON c.id = cus.company_id AND cus.relationship_type = 'customer' AND cus.rank = 1
LEFT JOIN (
    SELECT from_company_id, to_company_id, SUM(value) AS total_value
    FROM transactions
    WHERE transaction_date >= DATE_SUB(CURDATE(), INTERVAL 1 YEAR)
    GROUP BY from_company_id, to_company_id
) sup_trans ON c.id = sup_trans.to_company_id AND sup.related_company_id = sup_trans.from_company_id
LEFT JOIN (
    SELECT from_company_id, to_company_id, SUM(value) AS total_value
    FROM transactions
    WHERE transaction_date >= DATE_SUB(CURDATE(), INTERVAL 1 YEAR)
    GROUP BY from_company_id, to_company_id
) cus_trans ON c.id = cus_trans.from_company_id AND cus.related_company_id = cus_trans.to_company_id
WHERE scc.supplier_count > 0 OR scc.customer_count > 0
ORDER BY (scc.supplier_count + scc.customer_count) DESC, c.name;
```

This query combines data from multiple tables (companies, company_relationships, transactions) to provide insights into each company's suppliers and customers.

### Questions:

1. Explain what this SQL query does and what kind of insights it provides about the companies and their relationships with suppliers and customers.

2. How would you adapt this query and its underlying data model to work in a big data environment using technologies like Hadoop, Spark, or a NoSQL database? Consider factors such as:
   - Data volume (assume billions of records across all tables)
   - Query performance
   - Scalability
   - Real-time vs. batch processing requirements

3. What challenges might you encounter when implementing this in a distributed big data system, and how would you address them?

4. How would you integrate this kind of complex relational data analysis into your entity resolution system? What additional insights or capabilities might this provide for identifying and merging similar entities?

5. How might you modify this query or data model to handle cases where the same company might be listed under slightly different names or addresses in the supplier and customer relationships?

Be prepared to discuss your approach and any assumptions you make about the data or system requirements.

## Evaluation Criteria

- Understanding of complex SQL queries and relational data models
- Ability to translate relational database concepts to big data paradigms
- Awareness of big data processing challenges and solutions
- Creative thinking in integrating complex data analysis with entity resolution
- Consideration of data quality and entity matching in the context of business relationships
