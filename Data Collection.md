# I Data Collection

Lecture Notes 1

## 1. 4 Ways

### (1) File: Download

### (2) Database: Query

### (3) API: Query -> usually REST APIs (Representational State Transfer)

* Uses standard HTTP interface and methods (GET, PUT, POST, DELETE)


* Stateless – the server doesn’t remember what you were doing

```python
pythontoken = "" # not going to tell you mine
response = requests.get("https://api.github.com/user", params={"access_token":token}) 
# usually have params
print response.content
```

Authentication: OAuth

### (4) Webpage: Scrap/Parse -> usually HTTP request
```python
# The Get command
response = requests.get("http://www.datasciencecourse.org")
# some relevant fields
response.status_coderesponse.content # or
response.textresponse.headersresponse.headers['Content-Type']
# Handeling Parameters in a URL like this:
# https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=9&cad=rja&uact=8
params = {"sa":"t", "rct":"j", "q":"", "esrc":"s", "source":"web", "cd":"9", "cad":"rja", "uact":"8"}
response = requests.get("http://www.google.com/url", params=params)
# Ohter commands that will change the state on the server
response = requests.put(...)response = requests.post(...)
response = requests.delete(...)```
```

Build a parser using regular expressions: re

* search, match, finditer, findall

```python
import re
match = re.search(r"data science", text) # find the first occurance of "data science"
print match.start()
match = re.match(r"data science", text) # check if start of text matches
match = re.search(r"data science", text) # find first match or None
for match in re.finditer("data science", text):
# iterate over all matches in the text
...
all_matches = re.findall(r"data science", text) # return all matches
```

* Compiled version

```python
regex = re.compile(r"data science")
regex.match(text, [startpos, [endpos]])
regex.search(...)
regex.finditer(...)
regex.findall(...)
```

* Matching multiple potential characters

  Match the character ‘a’: a
  Match the character ‘a’, ‘b’, or ‘c’: [abc]
  Many any character except ‘a’, ‘b’, or ‘c’: [^abc]
  Match any digit: \d (= [0-9])
  Match any alpha-numeric: \w (= [a-zA-z0-9_])
  Match whitespace: \s (= [ \t\n\r\f\v])
  Match any character:. (including newline with re.DOTALL)

* Matching repeated characters

  Match character ‘a’ exactly once: a
  Match character ‘a’ zero or one time: a?
  Match character ‘a’ zero or more times: a*
  Match character ‘a’ one or more times: a+
  Match character ‘a’ exactly n times: a{n}

* Grouping

  Match all instances of “ science” where  is an alphanumeric string with at least one character: \w+\s+science

```python
match = re.search(r"(\w+)\s([Ss]cience)", response.content)
print match.start(), match.groups()
# 315 ('Data', 'Science')
```

* Substitution

```python
# replace "data science" with "schmada science"
better_text = re.sub(r"data science", r"schmada science", text)
# include text that was remembered in the matching using groups
better_text = re.sub(r"(\w+)\s([Ss])cience", r"\1 \2hmience", text)
```

* Special characters in regular expressions: .^$*+?{}\[]|() 

  if you want to match these characters exactly, you need to escape them: \$

* Greedy matching: By default, regular expressions try to capture as much text as possible

Example:

1. <(.*)> applied to \<a>text\</a> will match the entire expression

   To capture the least amount of text possible use <(.*?)> 

2. abc|def matches the strings “abc” or “def”, not “ab(c or d)ef”

   get around this using parenthesis e.g. a(bc|de)f

##  2. Data Formats

### (1) CSV files: pandas

(comma separate value) that are not always separated by commas

```python
import pandas as pd
dataframe = pd.read_csv("CourseRoster_F16_15688_B_08.30.2016.csv", delimiter = ',', quotechar = '"')
```

### (2) JSON files and string: json

(Javascript object notation) that looks like this:

{  "message": "Not Found",  

"documentation_url": "https://developer.github.com/v3"}

```python
import json # load json from a REST API call
response = pythonrequests.get("https://api.github.com/user", params={"access_token":token})
data = json.loads(response.content)
json.load(file) # load json from file
json.dumps(obj) # return json string
json.dump(obj, file) # write json to file
```

### (3) HTML/XML files and string: BeautifulSoup

(hypertext markup language / extensible markup language) that contains hiearchical content delineated by tags

```python
# get all the links within the data science course schedule
from bs4 import BeautifulSoup
import requests
response = requests.get("http://www.datasciencecourse.org")
root = BeautifulSoup(response.content)
root.find("section",id="schedule").find("table").find("tbody").findAll("a")
```
