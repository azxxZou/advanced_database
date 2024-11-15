# 实验三 聚合操作练习

**学院：省级示范性软件学院**

**课程：高级数据库技术与应用**

**题目：**《实验三 聚合操作练习》

**姓名：**邹子昂

**学号：**2100240124

**班级：**软工2202

**日期：**2024-10-20

**实验环境：** Kibana8.12.2 & Elasticsearch8.12.2

## 一、实验目的

掌握Elasticsearch聚合操作

- Elasticsearch 聚合操作练习

- 完成题目中的任意15道题。

## 二、实验内容

#### (1)插入电商数据分析（ecommerce）索引信息

```json
PUT /ecommerce
{
  "mappings": {
    "properties": {
      "order_id": { "type": "keyword" },
      "order_date": { "type": "date" },
      "customer_id": { "type": "keyword" },
      "customer_name": { "type": "keyword" },
      "customer_gender": { "type": "keyword" },
      "customer_age": { "type": "integer" },
      "customer_city": { "type": "keyword" },
      "product_id": { "type": "keyword" },
      "product_name": { "type": "keyword" },
      "product_category": { "type": "keyword" },
      "quantity": { "type": "integer" },
      "price": { "type": "float" },
      "total_amount": { "type": "float" },
      "payment_method": { "type": "keyword" },
      "is_returned": { "type": "boolean" }
    }
  }
}
```

![image-20241020142616235](./images/image-20241020142616235.png)

#### (2)批量插入电商数据

```json
POST /ecommerce/_bulk
{"index": {"_id": "1"}}
{"order_id": "6945", "order_date": "2023-03-06", "customer_id": "C242", "customer_name": "Bob Smith", "customer_gender": "female", "customer_age": 60, "customer_city": "Philadelphia", "product_id": "P041", "product_name": "Pro Accessory", "product_category": "Sports", "quantity": 4, "price": 205.89, "total_amount": 823.56, "payment_method": "Credit Card", "is_returned": false}
{"index": {"_id": "2"}}
{"order_id": "5629", "order_date": "2023-11-14", "customer_id": "C262", "customer_name": "Frank Garcia", "customer_gender": "female", "customer_age": 52, "customer_city": "Dallas", "product_id": "P097", "product_name": "Ultra Accessory", "product_category": "Sports", "quantity": 3, "price": 475.18, "total_amount": 1425.54, "payment_method": "Debit Card", "is_returned": true}
{"index": {"_id": "3"}}
{"order_id": "4488", "order_date": "2023-03-28", "customer_id": "C342", "customer_name": "Frank Garcia", "customer_gender": "female", "customer_age": 62, "customer_city": "New York", "product_id": "P001", "product_name": "Ultra Device", "product_category": "Books", "quantity": 5, "price": 722.89, "total_amount": 3614.45, "payment_method": "Credit Card", "is_returned": false}
{"index": {"_id": "4"}}
{"order_id": "7960", "order_date": "2023-09-23", "customer_id": "C408", "customer_name": "Eva Johnson", "customer_gender": "male", "customer_age": 37, "customer_city": "San Antonio", "product_id": "P079", "product_name": "Ultra Accessory", "product_category": "Home Appliances", "quantity": 2, "price": 485.3, "total_amount": 970.6, "payment_method": "Cash on Delivery", "is_returned": false}
{"index": {"_id": "5"}}
{"order_id": "2111", "order_date": "2023-02-18", "customer_id": "C981", "customer_name": "Eva Garcia", "customer_gender": "female", "customer_age": 19, "customer_city": "Los Angeles", "product_id": "P091", "product_name": "Mega Widget", "product_category": "Books", "quantity": 5, "price": 61.52, "total_amount": 307.6, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "6"}}
{"order_id": "2121", "order_date": "2023-01-10", "customer_id": "C030", "customer_name": "Jack Davis", "customer_gender": "male", "customer_age": 18, "customer_city": "Chicago", "product_id": "P070", "product_name": "Ultra Tool", "product_category": "Home Appliances", "quantity": 3, "price": 860.58, "total_amount": 2581.74, "payment_method": "Debit Card", "is_returned": true}
{"index": {"_id": "7"}}
{"order_id": "1211", "order_date": "2023-07-06", "customer_id": "C672", "customer_name": "Jack Williams", "customer_gender": "female", "customer_age": 64, "customer_city": "San Diego", "product_id": "P092", "product_name": "Mega Device", "product_category": "Electronics", "quantity": 4, "price": 389.1, "total_amount": 1556.4, "payment_method": "Cash on Delivery", "is_returned": true}
{"index": {"_id": "8"}}
{"order_id": "7394", "order_date": "2023-08-15", "customer_id": "C719", "customer_name": "Frank Rodriguez", "customer_gender": "female", "customer_age": 59, "customer_city": "San Diego", "product_id": "P083", "product_name": "Super Device", "product_category": "Beauty", "quantity": 2, "price": 593.15, "total_amount": 1186.3, "payment_method": "Credit Card", "is_returned": true}
{"index": {"_id": "9"}}
{"order_id": "7593", "order_date": "2023-07-22", "customer_id": "C145", "customer_name": "Bob Johnson", "customer_gender": "female", "customer_age": 33, "customer_city": "Dallas", "product_id": "P031", "product_name": "Pro Tool", "product_category": "Furniture", "quantity": 3, "price": 740.32, "total_amount": 2220.96, "payment_method": "PayPal", "is_returned": false}
{"index": {"_id": "10"}}
{"order_id": "3304", "order_date": "2023-03-03", "customer_id": "C111", "customer_name": "Jack Jones", "customer_gender": "female", "customer_age": 53, "customer_city": "Chicago", "product_id": "P027", "product_name": "Elite Device", "product_category": "Jewelry", "quantity": 1, "price": 489.24, "total_amount": 489.24, "payment_method": "Credit Card", "is_returned": true}
{"index": {"_id": "11"}}
{"order_id": "7928", "order_date": "2023-05-02", "customer_id": "C441", "customer_name": "Henry Williams", "customer_gender": "male", "customer_age": 57, "customer_city": "San Diego", "product_id": "P005", "product_name": "Elite Accessory", "product_category": "Groceries", "quantity": 3, "price": 989.17, "total_amount": 2967.51, "payment_method": "Debit Card", "is_returned": true}
{"index": {"_id": "12"}}
{"order_id": "9071", "order_date": "2023-04-01", "customer_id": "C798", "customer_name": "Grace Williams", "customer_gender": "male", "customer_age": 66, "customer_city": "Phoenix", "product_id": "P055", "product_name": "Ultra Widget", "product_category": "Furniture", "quantity": 4, "price": 332.32, "total_amount": 1329.28, "payment_method": "Cash on Delivery", "is_returned": false}
{"index": {"_id": "13"}}
{"order_id": "4022", "order_date": "2023-08-04", "customer_id": "C763", "customer_name": "Jack Martinez", "customer_gender": "female", "customer_age": 23, "customer_city": "Phoenix", "product_id": "P026", "product_name": "Ultra Widget", "product_category": "Books", "quantity": 5, "price": 826.39, "total_amount": 4131.95, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "14"}}
{"order_id": "1363", "order_date": "2023-04-28", "customer_id": "C727", "customer_name": "Frank Davis", "customer_gender": "female", "customer_age": 67, "customer_city": "New York", "product_id": "P012", "product_name": "Super Widget", "product_category": "Toys", "quantity": 3, "price": 248.55, "total_amount": 745.65, "payment_method": "Credit Card", "is_returned": false}
{"index": {"_id": "15"}}
{"order_id": "6142", "order_date": "2023-04-27", "customer_id": "C005", "customer_name": "Charlie Davis", "customer_gender": "male", "customer_age": 28, "customer_city": "San Jose", "product_id": "P048", "product_name": "Ultra Gadget", "product_category": "Home Appliances", "quantity": 4, "price": 510.43, "total_amount": 2041.72, "payment_method": "PayPal", "is_returned": true}
{"index": {"_id": "16"}}
{"order_id": "2769", "order_date": "2023-01-03", "customer_id": "C393", "customer_name": "Alice Rodriguez", "customer_gender": "male", "customer_age": 19, "customer_city": "San Jose", "product_id": "P093", "product_name": "Ultra Accessory", "product_category": "Jewelry", "quantity": 3, "price": 317.89, "total_amount": 953.67, "payment_method": "Cash on Delivery", "is_returned": true}
{"index": {"_id": "17"}}
{"order_id": "3810", "order_date": "2023-07-30", "customer_id": "C808", "customer_name": "Ivy Garcia", "customer_gender": "male", "customer_age": 52, "customer_city": "Philadelphia", "product_id": "P013", "product_name": "Elite Gadget", "product_category": "Electronics", "quantity": 3, "price": 541.31, "total_amount": 1623.93, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "18"}}
{"order_id": "4772", "order_date": "2023-04-12", "customer_id": "C043", "customer_name": "David Garcia", "customer_gender": "male", "customer_age": 18, "customer_city": "Los Angeles", "product_id": "P009", "product_name": "Elite Gadget", "product_category": "Beauty", "quantity": 3, "price": 953.55, "total_amount": 2860.65, "payment_method": "Credit Card", "is_returned": false}
{"index": {"_id": "19"}}
{"order_id": "9792", "order_date": "2023-02-25", "customer_id": "C974", "customer_name": "Henry Miller", "customer_gender": "male", "customer_age": 37, "customer_city": "Los Angeles", "product_id": "P012", "product_name": "Ultra Device", "product_category": "Beauty", "quantity": 1, "price": 898.19, "total_amount": 898.19, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "20"}}
{"order_id": "8033", "order_date": "2023-02-17", "customer_id": "C280", "customer_name": "Bob Rodriguez", "customer_gender": "female", "customer_age": 63, "customer_city": "New York", "product_id": "P084", "product_name": "Ultra Device", "product_category": "Electronics", "quantity": 1, "price": 847.14, "total_amount": 847.14, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "21"}}
{"order_id": "4105", "order_date": "2023-11-30", "customer_id": "C605", "customer_name": "Henry Martinez", "customer_gender": "male", "customer_age": 24, "customer_city": "San Antonio", "product_id": "P003", "product_name": "Elite Widget", "product_category": "Groceries", "quantity": 3, "price": 871.75, "total_amount": 2615.25, "payment_method": "Cash on Delivery", "is_returned": false}
{"index": {"_id": "22"}}
{"order_id": "5339", "order_date": "2023-05-25", "customer_id": "C323", "customer_name": "David Johnson", "customer_gender": "female", "customer_age": 41, "customer_city": "Dallas", "product_id": "P054", "product_name": "Elite Gadget", "product_category": "Fashion", "quantity": 4, "price": 501.88, "total_amount": 2007.52, "payment_method": "Credit Card", "is_returned": false}
{"index": {"_id": "23"}}
{"order_id": "2291", "order_date": "2023-02-22", "customer_id": "C180", "customer_name": "Bob Smith", "customer_gender": "male", "customer_age": 53, "customer_city": "Chicago", "product_id": "P065", "product_name": "Ultra Tool", "product_category": "Groceries", "quantity": 5, "price": 229.9, "total_amount": 1149.5, "payment_method": "PayPal", "is_returned": true}
{"index": {"_id": "24"}}
{"order_id": "6857", "order_date": "2023-03-15", "customer_id": "C552", "customer_name": "Alice Smith", "customer_gender": "male", "customer_age": 21, "customer_city": "San Diego", "product_id": "P099", "product_name": "Elite Widget", "product_category": "Jewelry", "quantity": 2, "price": 621.51, "total_amount": 1243.02, "payment_method": "Credit Card", "is_returned": false}
{"index": {"_id": "25"}}
{"order_id": "9291", "order_date": "2023-07-12", "customer_id": "C424", "customer_name": "Grace Garcia", "customer_gender": "male", "customer_age": 63, "customer_city": "San Diego", "product_id": "P062", "product_name": "Super Device", "product_category": "Electronics", "quantity": 1, "price": 302.94, "total_amount": 302.94, "payment_method": "Debit Card", "is_returned": true}
{"index": {"_id": "26"}}
{"order_id": "1897", "order_date": "2023-01-20", "customer_id": "C056", "customer_name": "Grace Martinez", "customer_gender": "female", "customer_age": 40, "customer_city": "San Diego", "product_id": "P018", "product_name": "Mega Tool", "product_category": "Sports", "quantity": 5, "price": 635.06, "total_amount": 3175.3, "payment_method": "PayPal", "is_returned": false}
{"index": {"_id": "27"}}
{"order_id": "5953", "order_date": "2023-04-14", "customer_id": "C363", "customer_name": "Henry Brown", "customer_gender": "female", "customer_age": 70, "customer_city": "New York", "product_id": "P024", "product_name": "Pro Widget", "product_category": "Home Appliances", "quantity": 2, "price": 882.01, "total_amount": 1764.02, "payment_method": "Cash on Delivery", "is_returned": true}
{"index": {"_id": "28"}}
{"order_id": "3156", "order_date": "2023-03-12", "customer_id": "C878", "customer_name": "Henry Martinez", "customer_gender": "male", "customer_age": 22, "customer_city": "Houston", "product_id": "P087", "product_name": "Mega Accessory", "product_category": "Jewelry", "quantity": 1, "price": 535.02, "total_amount": 535.02, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "29"}}
{"order_id": "6906", "order_date": "2023-12-20", "customer_id": "C071", "customer_name": "Bob Martinez", "customer_gender": "male", "customer_age": 69, "customer_city": "Philadelphia", "product_id": "P034", "product_name": "Mega Accessory", "product_category": "Home Appliances", "quantity": 2, "price": 639.65, "total_amount": 1279.3, "payment_method": "PayPal", "is_returned": false}
{"index": {"_id": "30"}}
{"order_id": "8191", "order_date": "2023-08-02", "customer_id": "C458", "customer_name": "Ivy Smith", "customer_gender": "female", "customer_age": 42, "customer_city": "Dallas", "product_id": "P016", "product_name": "Elite Tool", "product_category": "Books", "quantity": 2, "price": 366.21, "total_amount": 732.42, "payment_method": "PayPal", "is_returned": false}
{"index": {"_id": "31"}}
{"order_id": "3182", "order_date": "2023-10-12", "customer_id": "C207", "customer_name": "Bob Garcia", "customer_gender": "female", "customer_age": 33, "customer_city": "San Diego", "product_id": "P047", "product_name": "Ultra Widget", "product_category": "Jewelry", "quantity": 3, "price": 833.95, "total_amount": 2501.85, "payment_method": "Debit Card", "is_returned": true}
{"index": {"_id": "32"}}
{"order_id": "4709", "order_date": "2023-05-21", "customer_id": "C451", "customer_name": "Grace Martinez", "customer_gender": "male", "customer_age": 40, "customer_city": "Chicago", "product_id": "P021", "product_name": "Mega Device", "product_category": "Groceries", "quantity": 3, "price": 106.01, "total_amount": 318.03, "payment_method": "PayPal", "is_returned": true}
{"index": {"_id": "33"}}
{"order_id": "1550", "order_date": "2023-02-25", "customer_id": "C029", "customer_name": "Frank Smith", "customer_gender": "female", "customer_age": 44, "customer_city": "San Jose", "product_id": "P037", "product_name": "Elite Tool", "product_category": "Groceries", "quantity": 4, "price": 743.62, "total_amount": 2974.48, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "34"}}
{"order_id": "5735", "order_date": "2023-08-19", "customer_id": "C765", "customer_name": "David Davis", "customer_gender": "female", "customer_age": 42, "customer_city": "Philadelphia", "product_id": "P037", "product_name": "Super Accessory", "product_category": "Groceries", "quantity": 3, "price": 724.34, "total_amount": 2173.02, "payment_method": "Credit Card", "is_returned": true}
{"index": {"_id": "35"}}
{"order_id": "2189", "order_date": "2023-01-20", "customer_id": "C988", "customer_name": "Charlie Brown", "customer_gender": "male", "customer_age": 24, "customer_city": "San Diego", "product_id": "P004", "product_name": "Ultra Widget", "product_category": "Jewelry", "quantity": 1, "price": 206.12, "total_amount": 206.12, "payment_method": "Credit Card", "is_returned": true}
{"index": {"_id": "36"}}
{"order_id": "4261", "order_date": "2023-04-16", "customer_id": "C199", "customer_name": "Grace Brown", "customer_gender": "female", "customer_age": 30, "customer_city": "New York", "product_id": "P027", "product_name": "Mega Tool", "product_category": "Furniture", "quantity": 1, "price": 291.97, "total_amount": 291.97, "payment_method": "Cash on Delivery", "is_returned": false}
{"index": {"_id": "37"}}
{"order_id": "3166", "order_date": "2023-03-21", "customer_id": "C949", "customer_name": "Ivy Rodriguez", "customer_gender": "female", "customer_age": 24, "customer_city": "Los Angeles", "product_id": "P008", "product_name": "Mega Widget", "product_category": "Books", "quantity": 1, "price": 600.69, "total_amount": 600.69, "payment_method": "PayPal", "is_returned": false}
{"index": {"_id": "38"}}
{"order_id": "7539", "order_date": "2023-08-15", "customer_id": "C581", "customer_name": "Alice Williams", "customer_gender": "male", "customer_age": 69, "customer_city": "San Antonio", "product_id": "P010", "product_name": "Super Tool", "product_category": "Home Appliances", "quantity": 4, "price": 584.18, "total_amount": 2336.72, "payment_method": "Credit Card", "is_returned": false}
{"index": {"_id": "39"}}
{"order_id": "4588", "order_date": "2023-02-23", "customer_id": "C131", "customer_name": "Alice Smith", "customer_gender": "female", "customer_age": 62, "customer_city": "San Diego", "product_id": "P057", "product_name": "Elite Accessory", "product_category": "Electronics", "quantity": 5, "price": 553.25, "total_amount": 2766.25, "payment_method": "PayPal", "is_returned": true}
{"index": {"_id": "40"}}
{"order_id": "4199", "order_date": "2023-11-10", "customer_id": "C494", "customer_name": "Charlie Jones", "customer_gender": "male", "customer_age": 45, "customer_city": "Houston", "product_id": "P020", "product_name": "Elite Tool", "product_category": "Beauty", "quantity": 1, "price": 342.63, "total_amount": 342.63, "payment_method": "Cash on Delivery", "is_returned": true}
{"index": {"_id": "41"}}
{"order_id": "9196", "order_date": "2023-02-06", "customer_id": "C534", "customer_name": "Alice Garcia", "customer_gender": "female", "customer_age": 32, "customer_city": "Phoenix", "product_id": "P020", "product_name": "Mega Device", "product_category": "Furniture", "quantity": 1, "price": 635.12, "total_amount": 635.12, "payment_method": "PayPal", "is_returned": false}
{"index": {"_id": "42"}}
{"order_id": "1941", "order_date": "2023-06-23", "customer_id": "C475", "customer_name": "Bob Rodriguez", "customer_gender": "female", "customer_age": 48, "customer_city": "Chicago", "product_id": "P090", "product_name": "Ultra Device", "product_category": "Electronics", "quantity": 4, "price": 215.28, "total_amount": 861.12, "payment_method": "Cash on Delivery", "is_returned": false}
{"index": {"_id": "43"}}
{"order_id": "9818", "order_date": "2023-05-27", "customer_id": "C532", "customer_name": "Bob Johnson", "customer_gender": "male", "customer_age": 47, "customer_city": "Phoenix", "product_id": "P084", "product_name": "Super Tool", "product_category": "Home Appliances", "quantity": 3, "price": 827.32, "total_amount": 2481.96, "payment_method": "PayPal", "is_returned": false}
{"index": {"_id": "44"}}
{"order_id": "1809", "order_date": "2023-07-13", "customer_id": "C317", "customer_name": "Charlie Miller", "customer_gender": "male", "customer_age": 36, "customer_city": "San Jose", "product_id": "P021", "product_name": "Pro Gadget", "product_category": "Jewelry", "quantity": 3, "price": 121.06, "total_amount": 363.18, "payment_method": "Credit Card", "is_returned": true}
{"index": {"_id": "45"}}
{"order_id": "1361", "order_date": "2023-01-29", "customer_id": "C138", "customer_name": "Charlie Davis", "customer_gender": "female", "customer_age": 50, "customer_city": "San Diego", "product_id": "P078", "product_name": "Ultra Accessory", "product_category": "Toys", "quantity": 2, "price": 948.74, "total_amount": 1897.48, "payment_method": "Cash on Delivery", "is_returned": false}
{"index": {"_id": "46"}}
{"order_id": "7019", "order_date": "2023-03-01", "customer_id": "C165", "customer_name": "Grace Martinez", "customer_gender": "male", "customer_age": 18, "customer_city": "Phoenix", "product_id": "P005", "product_name": "Elite Accessory", "product_category": "Furniture", "quantity": 5, "price": 963.32, "total_amount": 4816.6, "payment_method": "Debit Card", "is_returned": true}
{"index": {"_id": "47"}}
{"order_id": "2564", "order_date": "2023-12-31", "customer_id": "C983", "customer_name": "Grace Martinez", "customer_gender": "male", "customer_age": 34, "customer_city": "San Diego", "product_id": "P067", "product_name": "Super Gadget", "product_category": "Home Appliances", "quantity": 5, "price": 919.89, "total_amount": 4599.45, "payment_method": "Cash on Delivery", "is_returned": false}
{"index": {"_id": "48"}}
{"order_id": "8300", "order_date": "2023-08-19", "customer_id": "C556", "customer_name": "Jack Martinez", "customer_gender": "female", "customer_age": 25, "customer_city": "Philadelphia", "product_id": "P069", "product_name": "Pro Tool", "product_category": "Toys", "quantity": 2, "price": 345.59, "total_amount": 691.18, "payment_method": "Credit Card", "is_returned": false}
{"index": {"_id": "49"}}
{"order_id": "1156", "order_date": "2023-06-15", "customer_id": "C438", "customer_name": "David Davis", "customer_gender": "male", "customer_age": 59, "customer_city": "Houston", "product_id": "P020", "product_name": "Mega Tool", "product_category": "Beauty", "quantity": 2, "price": 687.43, "total_amount": 1374.86, "payment_method": "Cash on Delivery", "is_returned": true}
{"index": {"_id": "50"}}
{"order_id": "2897", "order_date": "2023-03-13", "customer_id": "C320", "customer_name": "Charlie Rodriguez", "customer_gender": "male", "customer_age": 68, "customer_city": "San Jose", "product_id": "P060", "product_name": "Pro Tool", "product_category": "Jewelry", "quantity": 5, "price": 918.59, "total_amount": 4592.95, "payment_method": "Credit Card", "is_returned": false}
{"index": {"_id": "51"}}
{"order_id": "5636", "order_date": "2023-05-12", "customer_id": "C643", "customer_name": "Jack Martinez", "customer_gender": "male", "customer_age": 33, "customer_city": "San Jose", "product_id": "P073", "product_name": "Ultra Tool", "product_category": "Fashion", "quantity": 4, "price": 141.53, "total_amount": 566.12, "payment_method": "Credit Card", "is_returned": false}
{"index": {"_id": "52"}}
{"order_id": "3983", "order_date": "2023-08-05", "customer_id": "C973", "customer_name": "Frank Johnson", "customer_gender": "female", "customer_age": 44, "customer_city": "San Jose", "product_id": "P084", "product_name": "Super Widget", "product_category": "Furniture", "quantity": 5, "price": 171.0, "total_amount": 855.0, "payment_method": "Cash on Delivery", "is_returned": false}
{"index": {"_id": "53"}}
{"order_id": "5290", "order_date": "2023-01-02", "customer_id": "C369", "customer_name": "Jack Williams", "customer_gender": "male", "customer_age": 56, "customer_city": "Los Angeles", "product_id": "P095", "product_name": "Ultra Gadget", "product_category": "Jewelry", "quantity": 2, "price": 634.53, "total_amount": 1269.06, "payment_method": "Cash on Delivery", "is_returned": false}
{"index": {"_id": "54"}}
{"order_id": "8195", "order_date": "2023-11-25", "customer_id": "C782", "customer_name": "Jack Martinez", "customer_gender": "male", "customer_age": 62, "customer_city": "Phoenix", "product_id": "P040", "product_name": "Ultra Gadget", "product_category": "Books", "quantity": 1, "price": 595.85, "total_amount": 595.85, "payment_method": "PayPal", "is_returned": true}
{"index": {"_id": "55"}}
{"order_id": "6632", "order_date": "2023-03-01", "customer_id": "C596", "customer_name": "Frank Garcia", "customer_gender": "female", "customer_age": 23, "customer_city": "Philadelphia", "product_id": "P080", "product_name": "Super Tool", "product_category": "Fashion", "quantity": 1, "price": 641.04, "total_amount": 641.04, "payment_method": "Credit Card", "is_returned": true}
{"index": {"_id": "56"}}
{"order_id": "1364", "order_date": "2023-12-18", "customer_id": "C701", "customer_name": "Charlie Smith", "customer_gender": "male", "customer_age": 67, "customer_city": "Phoenix", "product_id": "P060", "product_name": "Pro Gadget", "product_category": "Fashion", "quantity": 1, "price": 978.14, "total_amount": 978.14, "payment_method": "Cash on Delivery", "is_returned": false}
{"index": {"_id": "57"}}
{"order_id": "7109", "order_date": "2023-08-25", "customer_id": "C480", "customer_name": "Bob Jones", "customer_gender": "female", "customer_age": 43, "customer_city": "San Antonio", "product_id": "P010", "product_name": "Elite Widget", "product_category": "Fashion", "quantity": 1, "price": 29.28, "total_amount": 29.28, "payment_method": "Debit Card", "is_returned": true}
{"index": {"_id": "58"}}
{"order_id": "5865", "order_date": "2023-04-29", "customer_id": "C658", "customer_name": "Frank Garcia", "customer_gender": "male", "customer_age": 58, "customer_city": "Los Angeles", "product_id": "P004", "product_name": "Super Gadget", "product_category": "Groceries", "quantity": 3, "price": 798.19, "total_amount": 2394.57, "payment_method": "Credit Card", "is_returned": true}
{"index": {"_id": "59"}}
{"order_id": "2212", "order_date": "2023-10-09", "customer_id": "C863", "customer_name": "Henry Garcia", "customer_gender": "female", "customer_age": 50, "customer_city": "Chicago", "product_id": "P085", "product_name": "Mega Gadget", "product_category": "Home Appliances", "quantity": 2, "price": 291.02, "total_amount": 582.04, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "60"}}
{"order_id": "6114", "order_date": "2023-05-21", "customer_id": "C365", "customer_name": "Charlie Garcia", "customer_gender": "male", "customer_age": 36, "customer_city": "Los Angeles", "product_id": "P080", "product_name": "Super Accessory", "product_category": "Jewelry", "quantity": 1, "price": 254.71, "total_amount": 254.71, "payment_method": "PayPal", "is_returned": false}
{"index": {"_id": "61"}}
{"order_id": "2867", "order_date": "2023-10-13", "customer_id": "C854", "customer_name": "Charlie Martinez", "customer_gender": "male", "customer_age": 21, "customer_city": "Houston", "product_id": "P031", "product_name": "Pro Widget", "product_category": "Beauty", "quantity": 4, "price": 679.3, "total_amount": 2717.2, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "62"}}
{"order_id": "5073", "order_date": "2023-12-25", "customer_id": "C285", "customer_name": "Bob Smith", "customer_gender": "male", "customer_age": 29, "customer_city": "San Diego", "product_id": "P078", "product_name": "Elite Accessory", "product_category": "Home Appliances", "quantity": 3, "price": 473.43, "total_amount": 1420.29, "payment_method": "Cash on Delivery", "is_returned": false}
{"index": {"_id": "63"}}
{"order_id": "9574", "order_date": "2023-06-26", "customer_id": "C023", "customer_name": "Charlie Garcia", "customer_gender": "female", "customer_age": 40, "customer_city": "San Jose", "product_id": "P076", "product_name": "Mega Widget", "product_category": "Furniture", "quantity": 2, "price": 115.01, "total_amount": 230.02, "payment_method": "Cash on Delivery", "is_returned": true}
{"index": {"_id": "64"}}
{"order_id": "5086", "order_date": "2023-01-28", "customer_id": "C965", "customer_name": "Jack Jones", "customer_gender": "female", "customer_age": 33, "customer_city": "Philadelphia", "product_id": "P036", "product_name": "Ultra Device", "product_category": "Fashion", "quantity": 4, "price": 326.84, "total_amount": 1307.36, "payment_method": "PayPal", "is_returned": false}
{"index": {"_id": "65"}}
{"order_id": "2320", "order_date": "2023-11-17", "customer_id": "C789", "customer_name": "David Smith", "customer_gender": "male", "customer_age": 21, "customer_city": "Chicago", "product_id": "P041", "product_name": "Elite Accessory", "product_category": "Sports", "quantity": 5, "price": 498.44, "total_amount": 2492.2, "payment_method": "PayPal", "is_returned": true}
{"index": {"_id": "66"}}
{"order_id": "2463", "order_date": "2023-12-07", "customer_id": "C038", "customer_name": "Jack Jones", "customer_gender": "male", "customer_age": 66, "customer_city": "Dallas", "product_id": "P017", "product_name": "Super Device", "product_category": "Beauty", "quantity": 3, "price": 499.37, "total_amount": 1498.11, "payment_method": "Credit Card", "is_returned": true}
{"index": {"_id": "67"}}
{"order_id": "9420", "order_date": "2023-06-25", "customer_id": "C336", "customer_name": "Grace Brown", "customer_gender": "female", "customer_age": 24, "customer_city": "Los Angeles", "product_id": "P035", "product_name": "Elite Device", "product_category": "Fashion", "quantity": 3, "price": 70.95, "total_amount": 212.85, "payment_method": "Cash on Delivery", "is_returned": true}
{"index": {"_id": "68"}}
{"order_id": "1322", "order_date": "2023-01-27", "customer_id": "C457", "customer_name": "Jack Garcia", "customer_gender": "male", "customer_age": 19, "customer_city": "Los Angeles", "product_id": "P076", "product_name": "Ultra Tool", "product_category": "Beauty", "quantity": 5, "price": 742.85, "total_amount": 3714.25, "payment_method": "Debit Card", "is_returned": true}
{"index": {"_id": "69"}}
{"order_id": "5018", "order_date": "2023-07-10", "customer_id": "C409", "customer_name": "Frank Martinez", "customer_gender": "male", "customer_age": 47, "customer_city": "San Jose", "product_id": "P021", "product_name": "Ultra Gadget", "product_category": "Fashion", "quantity": 5, "price": 266.16, "total_amount": 1330.8, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "70"}}
{"order_id": "7009", "order_date": "2023-05-10", "customer_id": "C923", "customer_name": "Jack Williams", "customer_gender": "male", "customer_age": 62, "customer_city": "Dallas", "product_id": "P001", "product_name": "Ultra Tool", "product_category": "Books", "quantity": 1, "price": 811.29, "total_amount": 811.29, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "71"}}
{"order_id": "5884", "order_date": "2023-05-29", "customer_id": "C528", "customer_name": "Charlie Johnson", "customer_gender": "female", "customer_age": 18, "customer_city": "Philadelphia", "product_id": "P031", "product_name": "Elite Gadget", "product_category": "Books", "quantity": 1, "price": 379.51, "total_amount": 379.51, "payment_method": "Cash on Delivery", "is_returned": false}
{"index": {"_id": "72"}}
{"order_id": "6635", "order_date": "2023-01-15", "customer_id": "C575", "customer_name": "Frank Brown", "customer_gender": "male", "customer_age": 51, "customer_city": "New York", "product_id": "P089", "product_name": "Elite Accessory", "product_category": "Electronics", "quantity": 4, "price": 781.98, "total_amount": 3127.92, "payment_method": "Cash on Delivery", "is_returned": true}
{"index": {"_id": "73"}}
{"order_id": "9082", "order_date": "2023-10-10", "customer_id": "C116", "customer_name": "Ivy Brown", "customer_gender": "male", "customer_age": 37, "customer_city": "Houston", "product_id": "P008", "product_name": "Elite Widget", "product_category": "Sports", "quantity": 4, "price": 111.95, "total_amount": 447.8, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "74"}}
{"order_id": "8042", "order_date": "2023-11-25", "customer_id": "C041", "customer_name": "Bob Davis", "customer_gender": "female", "customer_age": 67, "customer_city": "Chicago", "product_id": "P062", "product_name": "Ultra Accessory", "product_category": "Books", "quantity": 2, "price": 352.53, "total_amount": 705.06, "payment_method": "Credit Card", "is_returned": true}
{"index": {"_id": "75"}}
{"order_id": "4047", "order_date": "2023-01-19", "customer_id": "C200", "customer_name": "Ivy Miller", "customer_gender": "female", "customer_age": 27, "customer_city": "San Diego", "product_id": "P056", "product_name": "Pro Device", "product_category": "Electronics", "quantity": 2, "price": 298.64, "total_amount": 597.28, "payment_method": "Debit Card", "is_returned": true}
{"index": {"_id": "76"}}
{"order_id": "4072", "order_date": "2023-12-01", "customer_id": "C126", "customer_name": "Grace Williams", "customer_gender": "female", "customer_age": 31, "customer_city": "Houston", "product_id": "P083", "product_name": "Pro Gadget", "product_category": "Groceries", "quantity": 2, "price": 480.09, "total_amount": 960.18, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "77"}}
{"order_id": "7911", "order_date": "2023-12-31", "customer_id": "C503", "customer_name": "Bob Jones", "customer_gender": "female", "customer_age": 21, "customer_city": "Los Angeles", "product_id": "P005", "product_name": "Elite Gadget", "product_category": "Electronics", "quantity": 3, "price": 192.63, "total_amount": 577.89, "payment_method": "Debit Card", "is_returned": true}
{"index": {"_id": "78"}}
{"order_id": "3590", "order_date": "2023-06-30", "customer_id": "C003", "customer_name": "Henry Smith", "customer_gender": "male", "customer_age": 23, "customer_city": "San Jose", "product_id": "P099", "product_name": "Pro Widget", "product_category": "Home Appliances", "quantity": 4, "price": 46.63, "total_amount": 186.52, "payment_method": "Cash on Delivery", "is_returned": true}
{"index": {"_id": "79"}}
{"order_id": "4306", "order_date": "2023-07-09", "customer_id": "C589", "customer_name": "Bob Rodriguez", "customer_gender": "male", "customer_age": 42, "customer_city": "Philadelphia", "product_id": "P051", "product_name": "Ultra Accessory", "product_category": "Beauty", "quantity": 3, "price": 570.6, "total_amount": 1711.8, "payment_method": "Cash on Delivery", "is_returned": true}
{"index": {"_id": "80"}}
{"order_id": "4255", "order_date": "2023-02-02", "customer_id": "C003", "customer_name": "Henry Miller", "customer_gender": "male", "customer_age": 66, "customer_city": "San Antonio", "product_id": "P076", "product_name": "Ultra Accessory", "product_category": "Toys", "quantity": 4, "price": 901.45, "total_amount": 3605.8, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "81"}}
{"order_id": "1162", "order_date": "2023-12-25", "customer_id": "C018", "customer_name": "Grace Smith", "customer_gender": "female", "customer_age": 63, "customer_city": "New York", "product_id": "P067", "product_name": "Ultra Device", "product_category": "Groceries", "quantity": 4, "price": 155.11, "total_amount": 620.44, "payment_method": "Cash on Delivery", "is_returned": false}
{"index": {"_id": "82"}}
{"order_id": "9635", "order_date": "2023-05-12", "customer_id": "C237", "customer_name": "Henry Johnson", "customer_gender": "female", "customer_age": 61, "customer_city": "New York", "product_id": "P079", "product_name": "Ultra Widget", "product_category": "Beauty", "quantity": 2, "price": 773.0, "total_amount": 1546.0, "payment_method": "Debit Card", "is_returned": true}
{"index": {"_id": "83"}}
{"order_id": "6795", "order_date": "2023-06-14", "customer_id": "C336", "customer_name": "Ivy Jones", "customer_gender": "male", "customer_age": 54, "customer_city": "Philadelphia", "product_id": "P010", "product_name": "Mega Device", "product_category": "Electronics", "quantity": 4, "price": 394.85, "total_amount": 1579.4, "payment_method": "Cash on Delivery", "is_returned": false}
{"index": {"_id": "84"}}
{"order_id": "6288", "order_date": "2023-09-22", "customer_id": "C327", "customer_name": "Jack Smith", "customer_gender": "male", "customer_age": 68, "customer_city": "Houston", "product_id": "P072", "product_name": "Ultra Tool", "product_category": "Home Appliances", "quantity": 3, "price": 919.6, "total_amount": 2758.8, "payment_method": "Credit Card", "is_returned": false}
{"index": {"_id": "85"}}
{"order_id": "5127", "order_date": "2023-01-13", "customer_id": "C243", "customer_name": "David Williams", "customer_gender": "female", "customer_age": 69, "customer_city": "Philadelphia", "product_id": "P043", "product_name": "Elite Device", "product_category": "Home Appliances", "quantity": 3, "price": 954.01, "total_amount": 2862.03, "payment_method": "PayPal", "is_returned": false}
{"index": {"_id": "86"}}
{"order_id": "2322", "order_date": "2023-10-03", "customer_id": "C543", "customer_name": "Frank Miller", "customer_gender": "male", "customer_age": 49, "customer_city": "Los Angeles", "product_id": "P099", "product_name": "Super Widget", "product_category": "Beauty", "quantity": 5, "price": 971.79, "total_amount": 4858.95, "payment_method": "PayPal", "is_returned": false}
{"index": {"_id": "87"}}
{"order_id": "9268", "order_date": "2023-01-07", "customer_id": "C136", "customer_name": "Henry Rodriguez", "customer_gender": "male", "customer_age": 63, "customer_city": "Houston", "product_id": "P046", "product_name": "Super Widget", "product_category": "Furniture", "quantity": 3, "price": 896.47, "total_amount": 2689.41, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "88"}}
{"order_id": "7091", "order_date": "2023-05-04", "customer_id": "C551", "customer_name": "Grace Brown", "customer_gender": "male", "customer_age": 61, "customer_city": "Chicago", "product_id": "P016", "product_name": "Ultra Gadget", "product_category": "Toys", "quantity": 4, "price": 95.13, "total_amount": 380.52, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "89"}}
{"order_id": "2123", "order_date": "2023-10-05", "customer_id": "C483", "customer_name": "Henry Rodriguez", "customer_gender": "male", "customer_age": 55, "customer_city": "San Antonio", "product_id": "P006", "product_name": "Mega Device", "product_category": "Jewelry", "quantity": 2, "price": 499.08, "total_amount": 998.16, "payment_method": "PayPal", "is_returned": true}
{"index": {"_id": "90"}}
{"order_id": "8013", "order_date": "2023-04-29", "customer_id": "C431", "customer_name": "Eva Rodriguez", "customer_gender": "male", "customer_age": 65, "customer_city": "Phoenix", "product_id": "P046", "product_name": "Elite Device", "product_category": "Furniture", "quantity": 4, "price": 889.3, "total_amount": 3557.2, "payment_method": "Credit Card", "is_returned": false}
{"index": {"_id": "91"}}
{"order_id": "1329", "order_date": "2023-02-27", "customer_id": "C198", "customer_name": "David Johnson", "customer_gender": "male", "customer_age": 64, "customer_city": "Dallas", "product_id": "P001", "product_name": "Pro Tool", "product_category": "Electronics", "quantity": 1, "price": 734.57, "total_amount": 734.57, "payment_method": "Cash on Delivery", "is_returned": true}
{"index": {"_id": "92"}}
{"order_id": "1937", "order_date": "2023-06-27", "customer_id": "C928", "customer_name": "Jack Garcia", "customer_gender": "female", "customer_age": 27, "customer_city": "Los Angeles", "product_id": "P002", "product_name": "Ultra Accessory", "product_category": "Toys", "quantity": 3, "price": 241.4, "total_amount": 724.2, "payment_method": "Cash on Delivery", "is_returned": false}
{"index": {"_id": "93"}}
{"order_id": "1472", "order_date": "2023-03-17", "customer_id": "C365", "customer_name": "Frank Miller", "customer_gender": "male", "customer_age": 40, "customer_city": "Los Angeles", "product_id": "P077", "product_name": "Elite Tool", "product_category": "Furniture", "quantity": 4, "price": 150.69, "total_amount": 602.76, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "94"}}
{"order_id": "5187", "order_date": "2023-06-07", "customer_id": "C725", "customer_name": "Ivy Martinez", "customer_gender": "female", "customer_age": 49, "customer_city": "Dallas", "product_id": "P025", "product_name": "Super Accessory", "product_category": "Jewelry", "quantity": 1, "price": 904.01, "total_amount": 904.01, "payment_method": "Credit Card", "is_returned": false}
{"index": {"_id": "95"}}
{"order_id": "1737", "order_date": "2023-09-15", "customer_id": "C977", "customer_name": "Ivy Brown", "customer_gender": "female", "customer_age": 40, "customer_city": "New York", "product_id": "P031", "product_name": "Elite Widget", "product_category": "Sports", "quantity": 5, "price": 977.22, "total_amount": 4886.1, "payment_method": "Cash on Delivery", "is_returned": true}
{"index": {"_id": "96"}}
{"order_id": "3054", "order_date": "2023-02-16", "customer_id": "C872", "customer_name": "David Miller", "customer_gender": "female", "customer_age": 52, "customer_city": "San Jose", "product_id": "P077", "product_name": "Ultra Gadget", "product_category": "Jewelry", "quantity": 5, "price": 493.64, "total_amount": 2468.2, "payment_method": "Debit Card", "is_returned": false}
{"index": {"_id": "97"}}
{"order_id": "5606", "order_date": "2023-06-12", "customer_id": "C053", "customer_name": "Charlie Williams", "customer_gender": "female", "customer_age": 55, "customer_city": "San Antonio", "product_id": "P027", "product_name": "Elite Device", "product_category": "Jewelry", "quantity": 1, "price": 698.18, "total_amount": 698.18, "payment_method": "Cash on Delivery", "is_returned": true}
{"index": {"_id": "98"}}
{"order_id": "6873", "order_date": "2023-12-19", "customer_id": "C209", "customer_name": "Henry Davis", "customer_gender": "male", "customer_age": 63, "customer_city": "New York", "product_id": "P012", "product_name": "Super Gadget", "product_category": "Electronics", "quantity": 2, "price": 287.91, "total_amount": 575.82, "payment_method": "Cash on Delivery", "is_returned": true}
{"index": {"_id": "99"}}
{"order_id": "2082", "order_date": "2023-02-27", "customer_id": "C184", "customer_name": "Charlie Rodriguez", "customer_gender": "male", "customer_age": 45, "customer_city": "San Jose", "product_id": "P043", "product_name": "Super Tool", "product_category": "Toys", "quantity": 4, "price": 621.0, "total_amount": 2484.0, "payment_method": "PayPal", "is_returned": false}
{"index": {"_id": "100"}}
{"order_id": "1211", "order_date": "2023-04-11", "customer_id": "C549", "customer_name": "Alice Garcia", "customer_gender": "male", "customer_age": 62, "customer_city": "Houston", "product_id": "P047", "product_name": "Ultra Device", "product_category": "Electronics", "quantity": 5, "price": 758.18, "total_amount": 3790.9, "payment_method": "PayPal", "is_returned": false}

```

![image-20241020143446526](./images/image-20241020143446526.png)

#### (3)聚合操作练习：

##### 1. 统计每个产品类别的总销售额。
```json
POST /ecommerce/_search
{
  "size": 0, 
  "aggs": {
    "sales_by_category": {
      "terms": {
        "field": "product_category", 
        "size": 10
      },
      "aggs": {
        "total_sales": {
          "sum": {
            "field": "total_amount"
          }
        }
      }
    }
  }
}
```

![image-20241020153603087](./images/image-20241020153603087.png)

##### 2. 计算每个城市的平均订单金额。

```json
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "avg_order_by_city": {
      "terms": {
        "field": "customer_city",
        "size": 20
      },
      "aggs": {
        "average_order_amount": {
          "avg": {
            "field": "total_amount"
          }
        }
      }
    }
  }
}
```

![image-20241020153800789](./images/image-20241020153800789.png)

##### 3. 找出销量最高的前5个产品。

```json
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "rank_production": {
      "terms": {
        "field": "product_name",
        "size": 5,
        "order": {
          "sum_production": "desc"
        }
      },
      "aggs": {
        "sum_production": {
          "sum": {
            "field": "quantity"
          }
        }
      }
    }
  }
}
```

![image-20241020154717048](./images/image-20241020154717048.png)

##### 4. 计算男性和女性客户的平均年龄。

```json
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "avg_gender": {
      "terms": {
        "field": "customer_gender",
        "size": 2
      },
      "aggs": {
        "avg_age": {
          "avg": {
            "field": "customer_age"
          }
        }
      }
    }
  }
}
```

![image-20241020155157482](./images/image-20241020155157482.png)

##### 5. 统计每种支付方式的使用次数和总金额。

```json
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "status_pay": {
      "terms": {
        "field": "payment_method",
        "size": 10
      },
      "aggs": {
        "count_pay": {
          "value_count": {
            "field": "payment_method"
          }
        },
        "sum_amount": {
          "sum": {
            "field": "total_amount"
          }
        }
      }
    }
  }
}
```

![image-20241020161738222](./images/image-20241020161738222.png)

##### 6. 计算每月的总销售额。

```json
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "status_sale": {
      "date_histogram": {
        "field": "order_date",
        "calendar_interval": "month"
      },
      "aggs": {
        "sum_amount": {
          "sum": {
            "field": "total_amount"
          }
        }
      }
    }
  }
}
```

![image-20241020162817485](./images/image-20241020162817485.png)

##### 7. 找出平均订单金额最高的前3个客户。

```json
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "rank3_customer": {
      "terms": {
        "field": "customer_id",
        "size": 3,
        "order": {
          "avg_amount": "desc"
        }
      },
      "aggs": {
        "avg_amount": {
          "avg": {
            "field": "total_amount"
          }
        }
      }
    }
  }
}
```

![image-20241021164904442](./images/image-20241021164904442.png)

##### 8. 计算每个年龄段（18-30，31-50，51+）的客户数量。

```json
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "range_age": {
      "range": {
        "field": "customer_age",
        "ranges": [
          {"from": 18, "to": 30},
          {"from": 31, "to": 50},
          {"from": 51}
        ]
      }
    }
  }
}
```

![image-20241021164836352](./images/image-20241021164836352.png)

##### 9. 计算每个产品类别的平均单价。

```json
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "status_price": {
      "terms": {
        "field": "product_category"
      },
      "aggs": {
        "avg_price": {
          "avg": {
            "field": "price"
          }
        }
      }
    }
  }
}
```

![image-20241021165251691](./images/image-20241021165251691.png)

##### 10. 找出订单数量最多的前5个城市。

```json
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "status_country": {
      "terms": {
        "field": "customer_city",
        "size": 5,
        "order": {
          "count_city": "desc"
        }
      },
      "aggs": {
        "count_city": {
          "value_count": {
            "field": "order_id"
          }
        }
      }
    }
  }
}
```

![image-20241021165505679](./images/image-20241021165505679.png)

##### 11. 计算每个季度的平均订单金额。

```json
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "status_season": {
      "date_histogram": {
        "field": "order_date",
        "calendar_interval": "quarter"
      },
      "aggs": {
        "avg_count": {
          "avg": {
            "field": "total_amount"
          }
        }
      }
    }
  }
}
```

![image-20241021170153430](./images/image-20241021170153430.png)

##### 12. 统计每个产品类别中的商品数量。

```json
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "status_product": {
      "terms": {
        "field": "product_category"
      },
      "aggs": {
        "count_product": {
          "value_count": {
            "field": "product_id"
          }
        }
      }
    }
  }
}
```

![image-20241021170336928](./images/image-20241021170336928.png)

##### 13. 计算男性和女性客户的平均订单金额。

```json
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "status_gender": {
      "terms": {
        "field": "customer_gender",
        "size": 2
      },
      "aggs": {
        "avg_gender_count": {
          "avg": {
            "field": "total_amount"
          }
        }
      }
    }
  }
}
```

![image-20241021170526887](./images/image-20241021170526887.png)

##### 14. 找出总销售额最高的前10个日期。

```json
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "status_date": {
      "terms": {
        "field": "order_date",
        "size": 10,
        "order": {
          "daily_amount": "desc"
        }
      },
      "aggs": {
        "daily_amount": {
          "sum": {
            "field": "total_amount"
          }
        }
      }
    }
  }
}
```

![image-20241021170941742](./images/image-20241021170941742.png)

##### 15. 计算每个季度销售额最高的产品类别。

```json
POST /ecommerce/_search
{
  "size": 0,
  "aggs": {
    "status_season": {
      "date_histogram": {
        "field": "order_date",
        "calendar_interval": "quarter"
      },
      "aggs": {
        "sale_amount": {
          "terms": {
            "field": "product_category",
            "size": 1,
            "order": {
              "sum_amount": "desc"
            }
          },
          "aggs": {
            "sum_amount": {
              "sum": {
                "field": "total_amount"
              }
            }
          }
        }
      }
    }
  }
}
```

![image-20241021172515740](./images/image-20241021172515740.png)

## 三、问题及解决办法

1. 批量输入数据时需要在末行输入一行空行