# Google-Cloud-Arcade-2025

# Product Search for Marketing with BigQuery || Lab Solution || Google Cloud Arcade 2025 🎮

## Subscribe : "Learn With Ashish" [![Subscribe on YouTube](https://img.shields.io/badge/-Subscribe%20on%20YouTube-FF0000?style=for-thebadge&logo=youtube&logoColor=white&labelColor=FF0000)](https://www.youtube.com/channel/UChSkWopRk1ErP2i0k4aa0KQ)   


## 💡 Solution here

### ⚙️ Run the Following Commands in Cloud Shell

```
bq load --source_format=CSV --skip_leading_rows=1 --autodetect DATASET.products_information gs://PROJECT-ID-bucket/products.csv
bq query --use_legacy_sql=false 'CREATE SEARCH INDEX product_search_index ON DATASET.products_information(ALL COLUMNS)'
bq query --use_legacy_sql=false 'SELECT * FROM DATASET.products_information WHERE SEARCH(products_information, "22 oz Water Bottle")'
```

### 🎉 Woohoo! You Did It! 🎉

Your hard work and determination paid off! 💻  
You've successfully completed the lab. Way to go! 🚀  

### 💬 Stay Connected with Our Community!


## Subscribe :  [Learn With Ashish](https://www.youtube.com/channel/UChSkWopRk1ErP2i0k4aa0KQ)
