# Write Data Into CSV Files

## Scenario 1: Load all the content of txt files in a folder
```
import csv
import os
import re
```

```
path = r'D:\Peter\UOttawa\2020_09\Deep Learning\Assignments\3\aclImdb\test\pos'

with open('test1.csv', 'w', newline ='') as csvfile:
    fieldnames = ['review', 'sentiment']
    writer = csv.DictWriter(csvfile, fieldnames=fieldnames)
    writer.writeheader()
    
    cate=[x for x in os.listdir(path)]
    
    for idx,folder in enumerate(cate):
        full_path = path + '\\' + folder
        #f = open(full_path, encoding='utf-8', errors='ignore')
        f = open(full_path, 'rb')
        byte = f.readline()
        line = str(byte)[2:-1]
        line = re.sub(r'\\', '', line)
        writer.writerow({'review': line, 'sentiment': 'positive'})
        
path = r'D:\Peter\UOttawa\2020_09\Deep Learning\Assignments\3\aclImdb\test\neg'

with open('test1.csv', 'a+', newline ='') as csvfile:
    fieldnames = ['review', 'sentiment']
    writer = csv.DictWriter(csvfile, fieldnames=fieldnames)
    
    cate=[x for x in os.listdir(path)]
    
    for idx,folder in enumerate(cate):
        full_path = path + '\\' + folder
        #f = open(full_path, encoding='utf-8', errors='ignore')
        f = open(full_path, 'rb')
        byte = f.readline()
        line = str(byte)[2:-1]
        line = re.sub(r'\\', '', line)
        writer.writerow({'review': line, 'sentiment': 'negative'})
```