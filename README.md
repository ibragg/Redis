# Redis
Устанавливаем redis с официального сайта следуя всем инструкциям.

## Сохраняем большой json file (размер ~20мб) из [kaggle](https://www.kaggle.com/datasets/gauravduttakiit/covid-19) при помощи скрипта на питоне:
Данный json грузился очень долго поэтому для теста взял поменьше

```javascript
import json
import redis
import time

if __name__ == '__main__':
    r = redis.StrictRedis(host='localhost', port=6379, db=1)
    with open('csvjson-2.json', encoding='cp1251') as data_file:
        test_data = json.load(data_file)
        count = len(test_data)
        start = time.time()
        for index, data in enumerate(test_data):
            value = str(data).encode('utf-8')
            r.set('obj:%s' % index, value)
            r.save()
        end = time.time()
        print('Imported', index, 'rows in', end - start, 'sec')
```
 В итоге вывод:
 
 ![import](import.png)
 
 Далее протестируем скорость чтения при помощи скрипта:
 ```javascript
import json
import redis
import time

if __name__ == '__main__':
    r = redis.StrictRedis(host='localhost', port=6379, db=1)
    start = time.time()
    for index, key in enumerate(r.keys('*')):
        unit = r.get(key)
    end = time.time()
    print(index+1, 'rows readed in ', end-start, 'sec')
```

 ![read](read.png)
