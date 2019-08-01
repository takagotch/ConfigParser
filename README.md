### configparser
---
https://docs.python.org/3/library/configparser.html

https://github.com/python/cpython/blob/master/Lib/configparser.py

```py
import configparser
config = configparser.ConfigParser()
config['DEFAULT'] = {'ServerAliveInterval': '45',
  'Compression': 'yes',
  'CompresionLevel': '9'}

config['bitbucket.org'] = {}
config['bitbucket.org']['User'] = 'hg'
config['topsecret.server.com'] = {}
topsecret = config[]
topsecret['Port'] = '50022'
topsecret['ForwarddX11'] = 'no'
config['DEFAULT']['ForwarddX11'] = 'yes'
with open('example.ini', 'w') as configfile:
  config.write(configfile)
```

```
```

```
```


