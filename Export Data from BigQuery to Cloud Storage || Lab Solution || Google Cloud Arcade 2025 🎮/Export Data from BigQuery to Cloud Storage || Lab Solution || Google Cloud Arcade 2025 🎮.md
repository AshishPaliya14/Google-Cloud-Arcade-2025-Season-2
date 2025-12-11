# Google-Cloud-Arcade-2025

# Export Data from BigQuery to Cloud Storage || Lab Solution || Google Cloud Arcade 2025 🎮

## Subscribe : "Learn With Ashish" [![Subscribe on YouTube](https://img.shields.io/badge/-Subscribe%20on%20YouTube-FF0000?style=for-thebadge&logo=youtube&logoColor=white&labelColor=FF0000)](https://www.youtube.com/channel/UChSkWopRk1ErP2i0k4aa0KQ)   


## 💡 Solution here

### ⚙️ Run the Following Commands in Cloud Shell

```
bq load --source_format=CSV --autodetect customer_details.customers customers.csv
bq query --use_legacy_sql=false --destination_table customer_details.male_customers 'SELECT CustomerID, Gender FROM customer_details.customers WHERE Gender="Male"'
bq extract customer_details.male_customers gs://PROJECT-ID-bucket/exported_male_customers.csv
bq query --use_legacy_sql=false --replace --destination_table=customer_details.male_customers 'SELECT CustomerID, Gender FROM customer_details.customers WHERE Gender = "Male"'
```

### 🎉 Woohoo! You Did It! 🎉

Your hard work and determination paid off! 💻  
You've successfully completed the lab. Way to go! 🚀  

### 💬 Stay Connected with Our Community!


## Subscribe :  [Learn With Ashish](https://www.youtube.com/channel/UChSkWopRk1ErP2i0k4aa0KQ)
