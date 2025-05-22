# Yii Queries vs GraphQL – A Complete Comparison

This document provides a detailed comparison between **Yii (ActiveRecord or DAO)** and **GraphQL** for fetching data from a MySQL database, highlighting key differences in control, data access patterns, and efficiency, with a visual representation of data fetching.

## Table of Contents
1. [Who Controls the Data Fetching?](#who-controls-the-data-fetching)
2. [Data Access Pattern](#data-access-pattern)
3. [Example: Getting Product Data](#example-getting-product-data)

## Who Controls the Data Fetching?

| Aspect | Yii Queries (Traditional) | GraphQL |
|--------|---------------------------|---------|
| **Control** | Backend decides what data is returned | Frontend decides what data is needed |
| **Example** | Controller method returns all product fields | Frontend queries only `name` and `price`, not `id` or `internal_code` |

## Data Access Pattern

| Aspect | Yii | GraphQL |
|--------|-----|---------|
| **Access Type** | Multiple REST endpoints (e.g., `/products`, `/products/1`) | Single `/graphql` endpoint |
| **Data Shape** | Fixed – determined by backend code | Flexible – client defines response shape |
| **Over-fetching** | Possible – returns unnecessary fields | Avoided – get only requested fields |
| **Under-fetching** | Requires multiple API calls | Fetches nested/related data in one query |

## Example: Getting Product Data

### Yii (Controller Method)
```php
public function actionGetProducts() {
    return Product::find()
        ->select(['id', 'name', 'price', 'internal_code'])
        ->asArray()
        ->all();
}
```

### GraphQL Query
```graphql
query {
  products {
    name
    price
  }
}
```

## Conclusion
GraphQL offers greater flexibility and efficiency over traditional Yii queries by allowing clients to request specific data, reducing over-fetching and under-fetching issues. The provided chart visualizes how GraphQL minimizes data transfer compared to Yii's fixed response structure.

For further details or implementation guidance, refer to the official documentation for [Yii](https://www.yiiframework.com/doc/guide/2.0/en) or [GraphQL](https://graphql.org/learn/).
