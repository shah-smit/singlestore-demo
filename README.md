# singlestore-demo

100K 1 by 1 insert: Elapsed time: 0:00:38.924627 (38 Seconds)


Load Data Sample:

```
import json
import os
import random
import datetime
import singlestoredb as s2

# Function to generate random JSON data
def generate_random_json_data(i):
    data = {
        "id": i,
        "name": random.choice(["Alice", "Bob", "Charlie", "David"]),
        "age": random.randint(18, 65),
        "created_at": datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    }
    return data

# Connect to SingleStore
conn = s2.connect(host="localhost", port=3306, user="root", password="3pADCT1rVY7kEnTqaw38qyaaFDiaL0aV")

# Create a database and table (if needed)
cursor = conn.cursor()
cursor.execute("CREATE DATABASE IF NOT EXISTS my_database")
cursor.execute("USE my_database")
cursor.execute("CREATE TABLE IF NOT EXISTS my_table (id INT PRIMARY KEY, data JSON)")
cursor.close()


start_time = datetime.datetime.now()

# Generate and insert JSON data
num_records = 100000  # Adjust the number of records as needed
for i in range(num_records):
    json_data = generate_random_json_data(i)

    # Insert the data into SingleStore
    cursor = conn.cursor()
    cursor.execute("INSERT INTO my_table (id, data) VALUES (%s, %s)", (json_data["id"], json.dumps(json_data)))
    cursor.close()

print("Data generated and loaded successfully!")

# Capture end time
end_time = datetime.datetime.now()

# Calculate elapsed time
elapsed_time = end_time - start_time

print("Elapsed time:", elapsed_time)
```
