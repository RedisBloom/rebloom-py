[![license](https://img.shields.io/github/license/RedisBloom/redisbloom-py.svg)](https://github.com/RedisBloom/redisbloom-py)
[![PyPI version](https://badge.fury.io/py/redisbloom.svg)](https://badge.fury.io/py/redisbloom)
[![CircleCI](https://circleci.com/gh/RedisBloom/redisbloom-py/tree/master.svg?style=svg)](https://circleci.com/gh/RedisBloom/redisbloom-py/tree/master)
[![GitHub issues](https://img.shields.io/github/release/RedisBloom/redisbloom-py.svg)](https://github.com/RedisBloom/redisbloom-py/releases/latest)
[![Codecov](https://codecov.io/gh/RedisBloom/redisbloom-py/branch/master/graph/badge.svg)](https://codecov.io/gh/RedisBloom/redisbloom-py)
[![Known Vulnerabilities](https://snyk.io/test/github/RedisBloom/redisbloom-py/badge.svg?targetFile=requirements.txt)](https://snyk.io/test/github/RedisBloom/redisbloom-py?targetFile=requirements.txt)
[![Total alerts](https://img.shields.io/lgtm/alerts/g/RedisBloom/redisbloom-py.svg?logo=lgtm&logoWidth=18)](https://lgtm.com/projects/g/RedisBloom/redisbloom-py/alerts/)

# Python client for RedisBloom
[![Forum](https://img.shields.io/badge/Forum-RedisBloom-blue)](https://forum.redislabs.com/c/modules/redisbloom)
[![Discord](https://img.shields.io/discord/697882427875393627?style=flat-square)](https://discord.gg/wXhwjCQ)

redisbloom-py is a package that gives developers easy access to several probabilistic data structures. The package extends [redis-py](https://github.com/andymccurdy/redis-py)'s interface with RedisBloom's API.

### Installation
```
$ pip install redisbloom
```

### Usage example

```python
# Using Bloom Filter
from redisbloom.client import Client
rb = Client()
rb.bfCreate('bloom', 0.01, 1000)
rb.bfAdd('bloom', 'foo')        # returns 1
rb.bfAdd('bloom', 'foo')        # returns 0
rb.bfExists('bloom', 'foo')     # returns 1
rb.bfExists('bloom', 'noexist') # returns 0

# Using Cuckoo Filter
from redisbloom.client import Client
rb = Client()
rb.cfCreate('cuckoo', 1000)
rb.cfAdd('cuckoo', 'filter')        # returns 1
rb.cfAddNX('cuckoo', 'filter')      # returns 0
rb.cfExists('cuckoo', 'filter')     # returns 1
rb.cfExists('cuckoo', 'noexist')    # returns 0

# Using Count-Min Sketch
from redisbloom.client import Client
rb = Client()
rb.cmsInitByDim('dim', 1000, 5)
rb.cmsIncrBy('dim', ['foo'], [5])
rb.cmsIncrBy('dim', ['foo', 'bar'], [5, 15])
rb.cmsQuery('dim', 'foo', 'bar')    # returns [10, 15]

# Using Top-K
from redisbloom.client import Client
rb = Client()
rb.topkReserve('topk', 3, 20, 3, 0.9)
rb.topkAdd('topk', 'A', 'B', 'C', 'D', 'E', 'A', 'A', 'B',
                   'C', 'G', 'D', 'B', 'D', 'A', 'E', 'E')
rb.topkQuery('topk', 'A', 'B', 'C', 'D')    # returns [1, 1, 0, 1]
rb.topkCount('topk', 'A', 'B', 'C', 'D')    # returns [4, 3, 2, 3]
rb.topkList('topk')                         # returns ['D', 'A', 'B']
```

### API
For complete documentation about RedisBloom's commands, refer to [RedisBloom's website](http://redisbloom.io).

### License
[BSD 3-Clause](https://github.com/RedisBloom/redisbloom-py/blob/master/LICENSE)

### Development

1. Create a virtualenv to manage your python dependencies, and ensure it's active.
   ```virtualenv -v venv```
2. Install [pypoetry](https://python-poetry.org/) to manage your dependencies.
   ```pip install -r requirements.txt```
3. Install dependencies.
   ```poetry install```

[tox](https://tox.readthedocs.io/en/latest/) runs all tests as its default target. Running *tox* by itself will run unit tests. Ensure you have a running redis, with the module loaded.
