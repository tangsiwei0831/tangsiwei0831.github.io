---
title: 'Miscellaneous'
date: 2025-01-27
permalink: /posts/miscellaneous
tags:
  - Software
  - Data
---
# Excel
1. When download online Google sheet with multiple tabs, the only version to choose is xlsx which is excel. 

2. Note that on Google sheet, the tab of Google sheet does not have length limit, but in excel, the limit is 30 characters. So the tab name of downloaded version may cut a few characters in this case.

# Github
1. For a fork repo, the github action yaml will not shown in action section initially. You need to make some modifications to yaml script and then it will become normal. Note that the configuration will not be inherited from original repo.

# Oracle
1. DPY-3013: unsupported Python type int for database type DB_TYPE_VARCHAR

    The problem happened due to the data insertion into Orcle DB is separated into different chunks, therefore, for some of the columns, the value may be null for the first chunk, then Oracle database regards tis column as VARCHAR type, which will be conflicted with the real type value (float) later. Check [issue](https://github.com/oracle/python-oracledb/issues/187) for the details.

    According to comments in the link, the issue can be resolved quickly by create a new cursor each chunk. 
    ```
    curr = conn.cursor()
    curr.executemany(query, values)

    curr.close()
    conn.commit()
    ```