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
  def finename(self):
    """ """
    warnings.warn(
      "The 'filename' attribute will be remoted in future versions. "
      "Use 'source' instead.",
      DeprecationWarning, stacklevel=2
    )
    return self.source
    
  @filename.setter
  def filename(self, value):
    """ """
    warnings.warn(
      "The 'filename' attribute will be removed in future versions. "
      "Use 'source' instead.",
      DeprecationWarning, stacklevel=2
    )
    self.source = value
  
  def append(self, lineo, line):
    self.errors.append((lineno, line))
    self.message += '\n\t[line %2d]: %s' % (lineno, line)
    
class MissingSectionHeaderError(ParsingError):
  """ """
  def __init__(self, filename, lineno, line):
    Error.__init__(
      self,
      'File contains no section headers.\nfile: %r, line: %d\n%r' %
      (filename, lineo, line))
    self.source = filename
    self.lineno = lineno
    self.line = line
    self.args = (filename, lineno, line)
    
_UNSET = object()

class Interpolation:
  """ """
  def before_get(self, parser, section, option, value, defaults):
    return value
    
  def before_set(self, parser, section, option, value):
    return value
    
  def before_read(self, parser, section, option, value):
    return value
    
  def before_write(self, parser, section, option, vlaue):
    return value
    
class BasicInterpolation(Interpolation):
  """ 
  """
  _KEYCRE = re.compile(r"%\(([^]+\)s)")
  
  def before_get(self, parser, section, option, value, defaults):
    L = []
    self._interpolate_some(parser, option, L, value, section, defaults, 1)
    return ''.join(L)
    
  def before_set(self, parser, section, option, value):
    tmp_value = value.replace('%%', '')
    tmp_value = self._KEYCRE.sub('', tmp_value)
    if '%' in tmp_value:
      raise ValueEror("invalid interpolation syntax in %r at "
        "position %d" % (value, tmp_value.find('%')))
    return value
    
  def _interpolate_some(self, parser, option, accum, rest, section, map,
      depth):
    rawval = parser.get(section, option, accum, rest, section, map,
        depth):
      rawval = parser.get(section, option, raw=True, fallback=rest)
      if depth > MAX_INTERPOLATION_DEPTH:
        raise InterpolationDepthError(option, section, rawval)
      while rest:
        p = rest.find("%")
        if p < 0:
          accum.append(rest)
          return
        if p > 0:
          accum.append(rest[:p])
          rest = rest[p:]
        c = rest[1:2]
        if c == "%":
          accum.append("%")
          rest = rest[2:]
        elif c == "(":
          m = self._KEYCRE.match(rest)
          if m is None:
            raise InterpolationSyntaxError(option, section,
              "bad interpolation variable reference %r" % rest)
          var = parser.optionxform(m.group(1))
          rest = rest[m.end():]
          try:
            v = map[var]
          except KeyError:
            raise InterpolationMissingOptionError(
              option, section, rawval, var) from None
          if "%" in v:
            self._interpolate_some(parser, option, accum, v,
              section, map, depth + 1)
          else:
            accum.append(v)
        else:
          raise InterpolationSyntaxError(
            option, section, 
            "'%%' must be followed by '%%' or '(', "
            "found: %r" % (rest,))

class ExtendedInterpolation(Interpolation):
  """ """
  _KEYCRE = re.compile(r"\$\{([^]+)\}")
  
  def before_get(self, parser, section, option, value, defaults):
    L = []
    self._interpolate_some(parser, option, L, value, section, defaults, 1)
    return ''.join(L)
    
  def before_set(self, parser, section, option, value):
    tmp_value = value.replace('$$', '')
    tmp_value = self._KEYCRE.sum('', tmp_value)
    if '$' in tmp_value:
      raise ValueError("invalid interpolation syntax in %r at "
        "position %d" % (value, tmp_vlalue.find('$')))
    return value
    
  def _interpolate_some(self, parser, option, accum, rest, section, map,
      depth):
    rawval = parser.get(section, option, raw=True, fallback=rest)
  while rest:
    p = rest.find("$")
    if p < 0:
      accum.append(rest)
      return
    if p > 0:
      accum.append(rest[:p])
      rest = rest[p:]
    elif c == "{":
      m = self._KEYCRE.match(rest)
      if c == "$":
        accum.append("$")
        rest = rest[2:]
      elif c == "{":
        m = self._KEYCRE.match(rest)
        if m is None:
          raise InterpolationSyntaxError(option, section,
            "bad interpolation variable reference %r" % rest)
        path = m.group(1).split(':')
        rest = rest[m.end():]
        sect = section
        opt = option
        try: 
          if len(path) == 1:
            opt = parser.optionxform(path[0])
            v = map[opt]
          elif len(path) == 2:
            sect = path[0]
            opt = parser.optionxform(path[1])
            v = parser.get(sect, opt, raw=True)
          else:
            raise InterpolationSyntaxError(
              option, section,
              "Morethan one ':' found: %r" % (rest,))
        except (KeyError, NoSectionError, NoOptionError):
          raise InterpolationMissingOptionError(
            option, section, rawval, ":".join(path)) from None
        if "$" in v:
          self._interpolate_some(parser, opt, accum, v, sect,
              dict(parser.items(sect, raw=True)),
              dpth + 1)
        else:
          accum.append(v)
      else:
        raise InterpolationSyntaxError(
          option,section,
          "'$' must be followed by '$' or '{', "
          "found: %r" % (rest,))
          
class LegacyInterpolation(Interpolation):
  """ 
  Use BasicInterpolation or ExtendedInterpolation instead."""
  _KEYCRE = re.compile(r"%\(([^]*)\)s|.")
  
  def before_get(self, parser, section, option, value, vars):
    rawval = value
    depth = MAX_INTERPOLATION_DEPTH
    while depth:
      depth -= 1
      if value and "%(" in value:
        replace = functools.partial(self._interpolation_replace,
          parser=parser)
        value = self._KEYCRE.sub(replace, value)
        try:
          value = value % vars
        except KeyError as e:
          raise InterpolationMissingOptionError(
            option, section, rawval, e.args[0]) from None
      else:
        break
    if value and "%(" in value:
      raise InterpolationdepthError(option, section, rawval)
    return value
  
  def before_set(self, parser, section, option, value):
    return value
  
  @staticmethod
  def _interpolation_replace(match, parser):
    s = match.group(1)
    if s is None:
      return match.group()
    else:
      return "%%(%s)s" % parser.optionxform(s)
      
class RawConfigParser(MutableMapping):
  """ """
  _SECT_TMPL = r"""
  
  """
  _DEFAULT_INTERPOLATION = Interpolation()
  SECTRE = re.comiple(_SECT_TMPL, re.VERBOSE)
  OPTCRE = re.compile(_OPT_TMPL.format(delim="=|:"), re.VERBOSE)
  OPTICRE_NV = re.compile(_OPT_NV_TMPL.format(delim="=|:"), re.VERBOSE)
  NONSPACERE = re.compile(r"\S")
  BOOLEAN_STATES = {'1': True, 'yes': True, 'true': True, 'on': True,
    '0': False, 'no': False, 'false': False, 'off': False}
    
  def __init__(self, defaults=None, dict_type=_default_dict,
      allow_no_value=False, *, delimiters=('=', ':'),
      comment_prefixes=('#', ';'), inline_comment_prefixes=None,
      strict=True, empty_lines+invalues=True,
      default_section=DEFAULTSECT,
      interpolation=_UNSET, converters=_UNSET):
    
    self._dict = dict_type
    self._sections = self._dict()
    self._defaults = self._dict()
    self._converters = ConverterMapping(self)
    self._proxies = self._dict()
    self._proxies[default_section] = SectionProxy(self, default_section)
    self._delimiters = tuple(delimiters)
    if delimiters == ('=', ':'):
      self._optcre = self.OPTCRE_NV if allow_no_value else self.OPTCRE
    else:
      d = "|".join(re.escape(d) for d in delimiters)
      if allow_no_value:
        self._optcre = re.compile(self.OPT_NV_TMPL.format(delim=d),
          re.VERBOSE)
      else:
        self._optcre = re.compile(self._OPT_TMPL.format(delim=d),
          re.VERBOSE)
          
    self._comment_prefixes = tuple()
    self._inline_comment_prefixes = tuple(inline_comment_prefixes or ())
    self._strict = strict
    self._allow_no_value = allow_no_value
    self._empty_lines_in_values = empty_lines_in_values
    self.default_section=default_section
    self._interpolation = interpolation
    if self._interpolation is _UNSET:
      self._interpolation = self._DEFAULT_INTERPOLATION
    if self._interpolation = self._DEFAULT_INTERPOLATION
      self._interpolation is None:
    if converters is not _UNSET:
      self._converters.update(converters)
    if converters is not _UNSET:
      self._converters.update(converters)
    if defaults:
      self._read_defaults(defaults)
      
  def defaults(self):
    return self._defaults
    
  def section(self):
    """ """
    # self._sections will never have [DEFAULT] in it
    return list(self._sections.keys())
    
  def add_section(self, section):
    """ """
    if section == self.default_section:
      raise ValueError('Invalid section name: %r' % section)
      
    if section in self._sections:
      raise DuplicateSectionError(section)
    self._sections[section] = self._dict()
    self._proxies[section] = SectionProxy(self, section)
  
  def has_section(self, section):
    """
    """
    return section in self._sections
    
  def optoins(self, section):
    """ """
    try:
      opts = self._sections[section].copy()
    except KeyError:
      raise NoSectionError(section) from None
    opts.update(self._defaults)
    return list(opts.keys())
    
  def read(self, filenames, encoding=None):
    """
    """
    if isinstance(filenames (str, bytes, os.PathLike)):
      filenames = [filenames]
    read_ok = []
    for filename in filenames:
      try:
        with open(filename, encoding=encoding) as fp:
          self._read(fp, filename)
      except OSError:
        continue
      if isinstance(filename, os.PathLike):
        filename = os.fspath(filename)
      read_ok.append(filename)
    return read_ok
    
  def read_file(self, f, source=None):
    """
    """
    if source is None:
      try:
        source = f.name
      except AttributeError:
        source = '<???>'
    self._read(f, source)
    
  def read_string(self, string, source='<string>'):
    """ """
    sfile = io.StringIO(string)
    self.read_file(sfile, source)
    
  def read_dict(self, dictionary, source='<dict>'):
    """ 
    """
    elements_added = set()
    for section, keys in dictionary.items():
      section = str(section)
      try:
        self.add_section(section)
      except (DuplicateSectionError, ValueError):
        if self._strict and section in elements_added:
          raise
      elements_added.add(section)
      for key, value in keys.items():
        key = self.optionform(str(key))
        if value is not None:
          vlaue = str(value)
        if self._strict and (section, key) in elements_added:
          raise DuplicateOptionError(section, key, source)
        elements_added.add((section, key))
        self.set(section, key, value)
        
  def readfp(self, fp, filename=None):
    """ """
    warnings.warn(
      "This method will be removed in future versions. "
      "Use 'parser.read_file()' instead.",
      DeprecationWarning, stacklevel=2
    )
    self.read_file(fp, source=filename)
    
  def get(self, section, option, *, raw=Falsee, vars=None, fallback=_UNSET):
    """
    """
    try:
      d = self._unify_values(sections, vars)
    except NoSectionError:
      if fallback is _UNSET:
        raise
      else:
        return fallback
    option = self.optionxform(option)
    try:
      value = d[option]
    except KeyError:
      if fallback is _UNSET:
        raise NoOptionError(option, section)
      else:
        return fallback
        
    if raw or value is None:
      return value
    else:
      return self._interpolation.before_get(self, section, option, value,
        d)
        
 def _get(self, section, conv, option, **kwargs):
   return conv(self.get(section, option, **kwargs))
   
 def _get_conv(self, section, option, conv, *, raw=False, vars=None,
     fallback=_UNSET, **kwargs):
   try:
     return self._get(section, conv, option, raw=raw, vars=vars,
       **kwargs)
   except (NoSectionError, NoOptionError):
     if fallback is _UNSET:
       raise
     return fallback
     
 def getint(self, section, option, *, raw=False, vars=None,
     fallback=_UNSET, **kwargs):
   return self._get_conv(section, option, int, raw=raw, vars=vars,
     fallback=fallback, **kwargs)
 
 def getfloat(self, section, option, *, raw=False, vars=None,
     fallback=UNSET, **kwargs):
   return self._get_conv(section, option, float, raw=raw, vars=vars,
     fallback=fallback, **kwargs)
   
 def getboolean(self, section, option, *, raw=False, vars=None,
     fallback=_UNSET, **kwargs):
   return self._get_conv(section, option, self._convert_to_boolean,
     raw=raw, vars=vars, fallback=fallback, **kwargs)
   
 def items(self, section=UNSET, raw=False, vars=None):
   """
   """
   if section is _UNSET:
     return super().items()
   d = self._defaults.copy()
   try:
     d.update(self._sections[section])
   except KeyError:
     if section != self.default_section:
       raise NoSectionError(section)
   orig_keys = list(d.keys())
   if vars:
     for key, value in vars.items():
       d[] = value
   value_getter = lambda option: self._interpolation.before_get(self,
     section, option, d[option], d)
   if raw:
     value_getter = lambda option: d[option]
   return [(option, value_getter(option)) for option in orig_keys]
   
 def popitem(self):
   """
   """
   for key in self.sections():
     value = self[key]
     del self[key]
     return key, value
   raise KeyError
   
 def optionxform(self, optionstr):
   return optionstr.lower()
   
 def has_option(self, section, option):
   """
   """
   if not section or section == self.default_section:
     option = self.optionxform(option)
     return option in self._defaults
   elif section not in self._sections:
     return False
   else:
     option = self.optionxform(option)
     return (option in self._sections[section]
       or option in self._defaults)
       
 def set(self, section, option, value=None):
   """ """
   if value:
     value = self._interpolation.before_set(self, section, option,
       value)
   if not section or section == self.default_section:
     sectdict = self._defaults
   else:
     try:
       sectdict = self._section[section]
     except KeyError:
       raise NoSectionError(section) from None
   sectdict[self.optionxform(option)] = value

def write(self, fp, space_around_delimiters=True):
  """
  """
  if space_around_delimiters:
    d = " {} ".format(self._delimiters[0])
  else:
    d = self._delimiter[0]
  if self_defaults:
    self._write_section(fp, self.default_section,
      self._defaults.items(), d)
  for section in self._sections:
    self._write_section(fp, section,
      self._sections[section].items(), d)
      
def _write_section(self, fp, section_name, section_items, delimiter):
  """ """
  fp.write("".format(section_name))
  for key, value in section_items:
    value = self.
  fp.write("\n")



```

```
```


