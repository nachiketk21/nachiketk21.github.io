---
layout: post
title: Simple PGAdmin connection with ruby
author: Nachiket
---

After searching for a long time I found this simple 2 steps to connect to a postgres DB and hit a query and get the required data

```conn = PGconn.connect("<<strong>URL</strong>>","<strong>5432</strong>","","","<<strong>Db</strong> <strong>name</strong>>","<<strong>username</strong>>","<<strong>password</strong>>")```

```res = conn.exec("select * from <table_name> where <column_name>='17141'")```
