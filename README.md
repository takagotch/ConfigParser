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

```py
class NoOptionError(Error):
  """ """
  def __init__(self, option, section):
    Error.__init__(self, "No option %r in section: %r" %
      (option, section))
    self.option = option
    self.section = section
    self.args = (oprion, section)
    
class InterpolationError(Error):
  """ """
  
  def __init__(self, option, section, msg):
    Error.__init__(self, msg)
    self.option = option
    self.option = option
    self.section = section
    
class InterpolationError(Error):
  """ """
  
  def __init__(self, option, section, msg):
    Error.__init__(self, msg)
    self.option = option
    self.section = section
    self.args = (option, section, msg)

class InterpolationMissingOptionError(InterpolationError):
  """ """
  def __init__(self, option, section, rawval, reference):
    msg = ("Bad value substitution: option {!r} in section {!r} contains "
      "an interpolation key {!r} which is not a valid option name."
      "Raw value: {!r}".format(option, section, reference, rawval))
    InterporlationError.__init__(self, option, section, msg)
    self.args = (option, section, rawval)
    
class ParsingError(Error):
  """ """
  
  def __init__(self, source=None, filename=None):
    if filename and source:
      raise ValueError("Cannot specify both `filename' and `source'."
        "Use `source'.")
    elif not filename and not source:
      raise ValueError("Required argument `source; not given.`")
    elif filename:
      source = filename
    Error.__init__(self, 'Source contains parsing errors: %r ' % source)
    self.source = source
    self.errors = []
    self.args = (source, )
    
  @property
  def finename
  

  
```

```
```


