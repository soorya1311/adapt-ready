
[1]
import pandas as pd
import json
import csv
import matplotlib.pyplot as plt
import seaborn as sns
Question 1
[2]
df = pd.read_csv("complaints.csv")
C:\Users\vikne\AppData\Local\Temp\ipykernel_35568\1047253087.py:1: DtypeWarning: Columns (16) have mixed types. Specify dtype option on import or set low_memory=False.
  df = pd.read_csv("complaints.csv")

[3]
df.head()

[4]
df.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 5251126 entries, 0 to 5251125
Data columns (total 18 columns):
 #   Column                        Dtype 
---  ------                        ----- 
 0   Date received                 object
 1   Product                       object
 2   Sub-product                   object
 3   Issue                         object
 4   Sub-issue                     object
 5   Consumer complaint narrative  object
 6   Company public response       object
 7   Company                       object
 8   State                         object
 9   ZIP code                      object
 10  Tags                          object
 11  Consumer consent provided?    object
 12  Submitted via                 object
 13  Date sent to company          object
 14  Company response to consumer  object
 15  Timely response?              object
 16  Consumer disputed?            object
 17  Complaint ID                  int64 
dtypes: int64(1), object(17)
memory usage: 721.1+ MB

The number of complaints changes over time
[5]
df['Date received'] = pd.to_datetime(df['Date received'])
complaints_over_time = df.resample('M', on='Date received').size()
complaints_over_time.plot(title='Complaints Over Time')
plt.xlabel('Date')
plt.ylabel('Number of Complaints')
plt.show()

Identify which products receive the most complaints.
[6]
top_products = df['Product'].value_counts().head(10)
top_products.plot(kind='bar', title='Top 10 Complained About Products')
plt.xlabel('Product')
plt.ylabel('Number of Complaints')
plt.show()

Investigate the timeliness of company responses.
[7]
timely_responses = df['Timely response?'].value_counts(normalize=True) * 100
timely_responses.plot(kind='pie', autopct='%1.1f%%', title='Timeliness of Company Responses')
plt.ylabel('')
plt.show()

The proportion of complaints that consumers disputed.
[8]
disputed = df['Consumer disputed?'].value_counts(normalize=True) * 100
disputed.plot(kind='pie', autopct='%1.1f%%', title='Consumer Disputes')
plt.ylabel('')
plt.show()

Complaints by state to see if there are regional trends.
[9]
top_states = df['State'].value_counts().head(10)
top_states.plot(kind='bar', title='Top 10 States by Number of Complaints')
plt.xlabel('State')
plt.ylabel('Number of Complaints')
plt.show()

Question 2
[12]
def find_length_of_LCIS(nums):
    if not nums:
        return 0

    max_length = 1
    current_length = 1

    for i in range(1, len(nums)):
        if nums[i] > nums[i - 1]:
            current_length += 1
        else:
            max_length = max(max_length, current_length)
            current_length = 1

    max_length = max(max_length, current_length)

    return max_length
[13]
# Example usage:
print(find_length_of_LCIS([1, 3, 5, 4, 7]))
print(find_length_of_LCIS([2, 2, 2, 2, 2]))
3
1

Question 3
[14]
from functools import cmp_to_key

def largestNumber(nums):
    # Convert numbers to strings for concatenation comparison
    nums = list(map(str, nums))
    
    # Custom comparator for sorting
    def compare(x, y):
        if x + y > y + x:
            return -1
        elif x + y < y + x:
            return 1
        else:
            return 0
    
    # Sort numbers based on custom comparator
    nums.sort(key=cmp_to_key(compare))
    
    # Join sorted numbers into a single string
    largest_num = ''.join(nums)
    
    # Handle the case where the result is something like "0000..."
    if largest_num[0] == '0':
        return '0'
    else:
        return largest_num
[15]
# Example usage:
print(largestNumber([10, 2]))
print(largestNumber([3, 30, 34, 5, 9]))
210
9534330

Question 4
[19]
# Parse JSON data
data = json.load(open("DT A1 sample_json.json"))

# Extract "servlet-name" and "servlet-class"
servlets = data['web-app']['servlet']
rows = [{'servlet-name': servlet['servlet-name'], 'servlet-class': servlet['servlet-class']} for servlet in servlets]

# Write to CSV file
csv_file = 'servlets.csv'
with open(csv_file, 'w', newline='') as file:
    writer = csv.DictWriter(file, fieldnames=['servlet-name', 'servlet-class'])
    writer.writeheader()
    writer.writerows(rows)
[20]
df_4 = pd.read_csv('servlets.csv')
df_4.head()

