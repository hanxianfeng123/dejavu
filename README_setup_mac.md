dejavu
==========
First, create a virtual environment with python2.7.10 on macOS
Second, install the requirement.
```bash
brew install portaudio
brew install ffmpeg
```
```pip install -r requirement.txt```
Then, change some codes as below:
1. dejavu/database_sql.py 第5行位置需要安装mysql-python，而我尝试很久没有成功，所以替换为pymysql
```python
import pymysql as mysql
from pymysql.cursors import DictCursor
```
2. dejavu/fingerprint.py 第3行位置倒入matplotlib部分，提示Python is not installed as a framework.
```python
import numpy as np
import matplotlib
matplotlib.use('TkAgg')
import matplotlib.mlab as mlab
import matplotlib.pyplot as plt
```
3. dejavu/__init__.py 第86行位置，之前提示'numpy.int64' object has no attribute 'translate'
```python
            else:
                def _convert_hashes(hashes=None):
                    new_hashes = set()
                    if hashes:
                        for i in hashes:
                            new_i =(i[0], int(i[1]))
                            new_hashes.add(new_i)
                    return new_hashes
                sid = self.db.insert_song(song_name, file_hash)
                hashes = _convert_hashes(hashes)
                self.db.insert_hashes(sid, hashes)
                self.db.set_song_fingerprinted(sid)
                self.get_fingerprinted_songs()
```
4. 创建本地数据库

$ mysql -u root -p
	Enter password: **********
	mysql> CREATE DATABASE IF NOT EXISTS dejavu;

5. 修改数据库配置 dejavu/dejavu.cnf.SAMPLE文件

6. 运行example.py测试
