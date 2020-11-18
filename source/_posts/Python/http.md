```python
import json
import requests

url = "http://localhost:5000/objecttrajactory/lastnminutes/2"
r = requests.get(url)
print(r.status_code)
dataJson = r.json()
print(dataJson)
```

