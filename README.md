# singlestore-demo

1M 1 by 1 insert: Elapsed time: 0:06:31.116132 (6 Mins)

100K 1 by 1 insert: Elapsed time: 0:00:43.442294 (43 Seconds)

select name, sum(amt) from my_table GROUP BY name; <-- 81ms


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
cursor.execute("CREATE TABLE IF NOT EXISTS my_table (id INT PRIMARY KEY, name varchar(10), amt INT, data JSON)")
cursor.close()


start_time = datetime.datetime.now()

# Generate and insert JSON data
num_records = 100000  # Adjust the number of records as needed
for i in range(num_records):
    json_data = generate_random_json_data(i)

    # Insert the data into SingleStore
    cursor = conn.cursor()
    cursor.execute("INSERT INTO my_table (id, name, amt, data) VALUES (%s, %s, %s, %s)", (json_data["id"], random.choice(["Alice", "Bob", "Charlie", "David"]), random.randint(1, 100000), json.dumps(json_data)))
    cursor.close()

print("Data generated and loaded successfully!")

# Capture end time
end_time = datetime.datetime.now()

# Calculate elapsed time
elapsed_time = end_time - start_time

print("Elapsed time:", elapsed_time)
```
