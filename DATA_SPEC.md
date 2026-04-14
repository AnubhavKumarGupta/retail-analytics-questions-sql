# 📐 Data Specification — Retail Analytics Text-to-SQL Dataset

This document defines the structure, fields, and quality standards for all contributions to this dataset.

---

## 📦 Submission Format

Each contribution is a single JSON file with the following structure:

```json
{
  "question": "",
  "context": "",
  "business_logic": "",
  "tables_used": [],
  "sql": ""
}
```

All five fields are **required**.

---

## 🔍 Field Definitions

---

### `question`
**Type:** `string`  
**Required:** Yes

The natural language question that a business user might ask. It should read like something a real analyst, manager, or executive would ask — not a technical query description.

**Good examples:**
- *"Which stores had the highest return rate last quarter?"*
- *"What are the top 10 products by revenue in the Electronics category this year?"*
- *"How many customers made more than 5 purchases in the last 90 days?"*

**Avoid:**
- Technical phrasing like *"Join fact_sales to dim_product and group by category"*
- Vague questions like *"Show me sales data"*

---

### `context`
**Type:** `string`  
**Required:** Yes

Background information about the business scenario. This helps readers understand *why* someone would ask this question and what decisions the answer informs.

**What to include:**
- Who is asking the question (e.g., store manager, CFO, merchandising team)
- What decision or action the answer will support
- Any relevant business situation (e.g., end of quarter, product launch, seasonal planning)

**Example:**
> "The regional operations team is preparing for a quarterly review and wants to identify stores where return rates spiked above the company average. High return rates may indicate product quality issues, poor staff training, or mismatched customer expectations."

---

### `business_logic`
**Type:** `string`  
**Required:** Yes

A plain-language explanation of *how* to answer the question — the reasoning behind the SQL, written for a business audience.

This is **not** a technical walkthrough of the SQL syntax. It explains the business rules, thresholds, and logic used to arrive at the answer.

**What to include:**
- How the key metric is calculated (e.g., "return rate = number of returns / total units sold")
- Any filters, thresholds, or business rules applied (e.g., "only stores open for the full quarter")
- How results are ranked or grouped
- Any edge cases handled

**Example:**
> "Return rate is calculated as the number of returned transactions divided by total sales transactions, expressed as a percentage. Only stores that were operational for the entire quarter are included to avoid skewing results. Stores are ranked in descending order of return rate, and only those exceeding the company-wide average are surfaced."

**Avoid:**
- Explaining SQL syntax (e.g., "we use a LEFT JOIN here because...")
- Copying the SQL query in text form

---

### `tables_used`
**Type:** `array` of `string`  
**Required:** Yes

List the names of all tables referenced in the SQL query. Use the standard schema table names (see below).

**Example:**
```json
"tables_used": ["fact_sales", "dim_product", "dim_store", "dim_date"]
```

---

### `sql`
**Type:** `string`  
**Required:** Yes (strongly encouraged — may be omitted only if contributor is non-technical)

The SQL query that answers the business question. It should be correct, readable, and follow standard SQL conventions.

**Guidelines:**
- Use the standard schema defined below
- Alias tables for readability
- Format with consistent casing (SQL keywords in UPPERCASE)
- Include comments if the query is complex
- Avoid database-specific syntax unless clearly noted

---

## 🗃️ Standard Schema

All examples in this dataset use the following table schema. Contributors should map their use cases to these tables.

### `fact_sales`
| Column | Type | Description |
|---|---|---|
| `sale_id` | INT | Unique transaction identifier |
| `date_id` | INT | FK to dim_date |
| `product_id` | INT | FK to dim_product |
| `store_id` | INT | FK to dim_store |
| `customer_id` | INT | FK to dim_customer |
| `quantity` | INT | Units sold |
| `unit_price` | DECIMAL | Price per unit |
| `net_sales` | DECIMAL | Revenue after discounts |
| `discount_amount` | DECIMAL | Total discount applied |
| `is_return` | BOOLEAN | Whether the transaction is a return |

### `dim_product`
| Column | Type | Description |
|---|---|---|
| `product_id` | INT | Unique product identifier |
| `product_name` | VARCHAR | Product display name |
| `category` | VARCHAR | Top-level category |
| `sub_category` | VARCHAR | Sub-category |
| `brand` | VARCHAR | Brand name |
| `unit_cost` | DECIMAL | Cost of goods |

### `dim_store`
| Column | Type | Description |
|---|---|---|
| `store_id` | INT | Unique store identifier |
| `store_name` | VARCHAR | Store display name |
| `city` | VARCHAR | City |
| `region` | VARCHAR | Geographic region |
| `store_type` | VARCHAR | e.g., flagship, outlet, express |
| `opening_date` | DATE | Date store opened |

### `dim_date`
| Column | Type | Description |
|---|---|---|
| `date_id` | INT | Unique date key |
| `full_date` | DATE | Calendar date |
| `year` | INT | Year |
| `quarter` | INT | Quarter (1–4) |
| `month` | INT | Month (1–12) |
| `week` | INT | ISO week number |
| `day_of_week` | VARCHAR | e.g., Monday |
| `is_weekend` | BOOLEAN | Whether the date is a weekend |
| `is_holiday` | BOOLEAN | Whether the date is a public holiday |

### `dim_customer`
| Column | Type | Description |
|---|---|---|
| `customer_id` | INT | Unique customer identifier |
| `customer_segment` | VARCHAR | e.g., Premium, Regular, New |
| `acquisition_channel` | VARCHAR | e.g., Online, In-Store, Referral |
| `city` | VARCHAR | Customer city |
| `region` | VARCHAR | Customer region |
| `first_purchase_date` | DATE | Date of first transaction |

---

## ✅ Quality Checklist

Before submitting, verify:

- [ ] `question` reads naturally, as a business user would ask it
- [ ] `context` explains who is asking and why
- [ ] `business_logic` explains the reasoning without referencing SQL syntax
- [ ] `tables_used` lists all tables in the query
- [ ] `sql` is valid, formatted, and uses the standard schema
- [ ] No sensitive, proprietary, or personally identifiable information is included
- [ ] The JSON file is valid (use a JSON validator if unsure)

---

## ❌ What Not to Include

- Real customer names, emails, or personal data
- Internal company names, system names, or proprietary terminology
- Queries with hardcoded credentials or environment-specific references
- Poorly formed questions with no clear business intent

---

*Questions about the spec? Open an Issue or start a Discussion.*
