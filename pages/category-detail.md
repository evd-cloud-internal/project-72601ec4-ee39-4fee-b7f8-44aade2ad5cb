---
name: Category Detail
assetId: 0063e563-130b-4461-ae74-e89ce4303e7e
type: page
---

# Category Detail

```sql mock_data
SELECT
  arrayElement(['Electronics', 'Clothing', 'Home & Garden', 'Sports', 'Books'], (number % 5) + 1) AS category,
  arrayElement(['Laptop', 'T-Shirt', 'Lamp', 'Basketball', 'Novel', 'Phone', 'Jeans', 'Rug', 'Tennis Racket', 'Cookbook', 'Tablet', 'Jacket', 'Vase', 'Yoga Mat', 'Biography'], (number % 15) + 1) AS product,
  toDate('2024-01-01') + toIntervalDay(number * 3) AS order_date,
  round(50 + (rand(number) % 450), 2) AS revenue,
  toUInt32(1 + (rand(number + 1) % 20)) AS units_sold
FROM numbers(100)
ORDER BY category, order_date
```

{% dropdown
    id="category_filter"
    data="mock_data"
    value_column="category"
    title="Category"
/%}

{% bar_chart
    data="mock_data"
    x="order_date"
    y="sum(revenue)"
    filters=["category_filter"]
    date_grain="month"
    title="Monthly Revenue"
    fmt="usd0"
/%}

{% table
    data="mock_data"
    title="Order Details"
    filters=["category_filter"]
%}
    {% dimension
        value="product"
    /%}
    {% dimension
        value="order_date"
        date_grain="month"
    /%}
    {% measure
        value="sum(revenue)"
        fmt="usd0"
    /%}
    {% measure
        value="sum(units_sold)"
        fmt="num0"
    /%}
{% /table %}