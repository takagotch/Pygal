### pygal
---
https://github.com/Kozea/pygal

http://www.pygal.org/en/latest/

```py
// pygal_gen.py
import argparse

import pygal

parser = argparse.ArgumentParser(
  description='Generate pygal chart in command line',
  prog='pygal_gen')
  
parser.add_argument('-t', '--type', dest='type', default='Line',
  choices=map(lambda x: x.__name__, pygal.CHARTS),
  help='Kind of chart to generate')
  
parser.add_argument('-o', '--output', dest='filename', default='pygal_out.svg',
  help='Filename to write the svg to')
  
parser.add_argument('-s', '--serie', dest='series', nargs='+', action='append',
  help='Add a serie in the form (tilte val1 val2...)')

parser.add_argument('--version', action='version',
  version='pygal %s' % pygal.__version__)
  
for key in pygal.config.CONFIG_ITEMS:
  opt_name = key.name
  val = key.value
  opts = {}
  if key.type == list:
    opts['type'] = key.subtype
    opts['nargs'] = '+'
  else:
    opts['type'] = key.type
    
  if opts['type'] == bool:
    del opts['type']
    opts['action'] = 'store_ture' if not val else 'store_false'
    if val:
      opt_name = 'no-' + opt_name
  if key.name == 'interpolate':
    opts['choices'] = list(pygal.interpolate.INTERPOLATIONS.keys())
  parser.add_argument(
    '--%s' % opt_name, dest=key.name, default=val, **opts)
    
config = parser.parser_args()

chart = getattr(pygal, config.type)(**vars(config))

forserie in config.series:
  chart.add(serie[0], map(float, serie[1:]))

chart.render_to_file(config.filename)
```

```sh
pip install pygal
pip install pytest
py.test
```

```
```


