---
layout: post
title:  "Android 数据库快速打印"
date:   2014-06-04 15:17:43
categories: Android
---

{% highlight python %}
#!/usr/bin/env python
__author__ = "Arthur"
__copyright__ = "Copyright (C) 2014 CVTE"
__version__ = "1.0"

import sqlite3
import os
import sys
import shutil
import cgi
import codecs

'''TO CONFIG'''
DB_PATH = "/data/data/com.cvte.weather/databases/weather.db"

DIR = "db_files"
INDEX_FILE = DIR + "/index.html"
AUTO_FILE = DIR + "/my.db"

def make_dir():
  shutil.rmtree(DIR, True)
  os.makedirs(DIR)

def pull_file(fn):
  print "pull_file: " + fn
  rv = os.system("adb pull"
    + " " + DB_PATH
    + " " + fn);
  if rv != 0:
    print "adb pull failed"
    sys.exit(1)

def get_columns_rows(conn, tab):
  c = conn.cursor()
  c.execute("SELECT * FROM " + tab)
  columns = [d[0] for d in c.description]
  rows = []
  for row in c:
    rows.append(row)
  return columns,rows

def print_cell(out, id, i, cell):
  if not cell is None:
    tmp = "%s"%(cell)
    out.write(tmp);

FUNCTIONS = {}

def print_top(out):
  out.write("""<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8" /> 
</head>
<body>
""")

def print_bottom(out):
  out.write("""
</body>
</html>
""")
  out.close()

def print_table(conn, out, tab):
  columns,rows = get_columns_rows(conn, tab)
  data = [dict(zip(columns,row)) for row in rows]

  # Data table
  out.write("""<b> %s table</b><br/>\n""" %(tab))
  out.write("""
<table border=1 cellspacing=0 cellpadding=4>
<tr>
""")
  print_functions = []
  for col in columns:
    print_functions.append(FUNCTIONS.get(col, print_cell))
  for i in range(0,len(columns)):
    col = columns[i]
    out.write("""  <th>%s</th>
""" % ( col ))
  out.write("""
</tr>
""")
  for row in rows:
    out.write("""<tr>
""")
    for i in range(0,len(row)):
      cell = row[i]
      # row[0] is always _id
      out.write("""  <td>""")
      print_functions[i](out, row[0], row, cell)
      out.write("""</td>
""")
    out.write("""</tr>
""")
  out.write("""</table>
""")
  out.write("<br><br>");

def process_file(fn):
  print "process_file: " + fn
  conn = sqlite3.connect(fn)
  out = codecs.open(INDEX_FILE, "w", 'utf-8')
  print_top(out)

  columns, rows = get_columns_rows(conn, "sqlite_master")

  for i in range(0,len(rows)):
    cell = rows[i]
    tab = cell[2]; '''tab name'''
    '''skip android_metadata and sqlite_sequence'''
    if (tab == "android_metadata") or (tab == "sqlite_sequence"):
      continue
    print_table(conn, out, tab)

  print_table(conn, out, "sqlite_master")
  print_bottom(out)
  path = os.path.dirname(os.path.abspath(__file__)) + "/" + INDEX_FILE
  os.system("firefox " + path)

def main(argv):
  if len(argv) == 1:
    make_dir()
    pull_file(AUTO_FILE)
    process_file(AUTO_FILE)
  elif len(argv) == 2:
    make_dir()
    process_file(argv[1])
  else:
    usage()

if __name__=="__main__":
  main(sys.argv)


{% endhighlight %}   
