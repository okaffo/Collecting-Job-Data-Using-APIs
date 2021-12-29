# Collecting-Job-Data-Using-APIs
Collecting Job Data Using APIs
Estimated time needed: 45 to 60 minutes

Objectives
After completing this lab, you will be able to:

Collect job data from GitHub Jobs API
Store the collected data into an excel spreadsheet.
Warm-Up Exercise
Before you attempt the actual lab, here is a fully solved warmup exercise that will help you to learn how to access an API.

Using an API, let us find out who currently are on the International Space Station (ISS).
The API at http://api.open-notify.org/astros.json gives us the information of astronauts currently on ISS in json format.
You can read more about this API at http://open-notify.org/Open-Notify-API/People-In-Space/

import requests # you need this module to make an API call
api_url = "http://api.open-notify.org/astros.json" # this url gives use the astronaut data
response = requests.get(api_url) # Call the API using the get method and store the
                                # output of the API call in a variable called response.
if response.ok:             # if all is well() no errors, no network timeouts)
    data = response.json()  # store the result in json format in a variable called data
                            # the variable data is of type dictionary.
print(data)   # print the data just to check the output or for debugging
{'people': [{'craft': 'ISS', 'name': 'Mark Vande Hei'}, {'craft': 'ISS', 'name': 'Pyotr Dubrov'}, {'craft': 'ISS', 'name': 'Anton Shkaplerov'}, {'craft': 'Shenzhou 13', 'name': 'Zhai Zhigang'}, {'craft': 'Shenzhou 13', 'name': 'Wang Yaping'}, {'craft': 'Shenzhou 13', 'name': 'Ye Guangfu'}, {'craft': 'ISS', 'name': 'Raja Chari'}, {'craft': 'ISS', 'name': 'Tom Marshburn'}, {'craft': 'ISS', 'name': 'Kayla Barron'}, {'craft': 'ISS', 'name': 'Matthias Maurer'}], 'message': 'success', 'number': 10}
Print the number of astronauts currently on ISS.

print(data.get('number'))
10
Print the names of the astronauts currently on ISS.

astronauts = data.get('people')
print("There are {} astronauts on ISS".format(len(astronauts)))
print("And their names are :")
for astronaut in astronauts:
    print(astronaut.get('name'))
There are 10 astronauts on ISS
And their names are :
Mark Vande Hei
Pyotr Dubrov
Anton Shkaplerov
Zhai Zhigang
Wang Yaping
Ye Guangfu
Raja Chari
Tom Marshburn
Kayla Barron
Matthias Maurer
Hope the warmup was helpful. Good luck with your next lab!

Lab: Collect Jobs Data using GitHub Jobs API
Objective: Determine the number of jobs currently open for various technologies
Collect the number of job postings for the following languages using the API:

C
C#
C++
Java
JavaScript
Python
Scala
Oracle
SQL Server
MySQL Server
PostgreSQL
MongoDB
#Import required libraries
import requests
import json
import pandas as pd
baseurl = "https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBM-DA0321EN-SkillsNetwork/labs/module%201/datasets/githubposting.json"
response = requests.get(baseurl)
data = response.json()
data = pd.DataFrame(data)
print(data)
     technology number of job posting
0          java                     1
1             C                    10
2            C#                     1
3           C++                     1
4          Java                     2
..          ...                   ...
251          C#                     1
252  PostgreSQL                     1
253     MongoDB                     1
254       Scala                     2
255  JavaScript                     3

[256 rows x 2 columns]
Write a function to get the number of jobs for the given technology.

def get_number_of_jobs(technology):
    r = requests.get(baseurl).json() #your code goes here
    r = pd.DataFrame.from_dict(data)
    r['number of job posting'] = r['number of job posting'].astype(int)
    number_of_jobs=r.groupby('technology').sum().loc[technology,:][0]
    return technology,number_of_jobs
​
print(data)
     technology number of job posting
0          java                     1
1             C                    10
2            C#                     1
3           C++                     1
4          Java                     2
..          ...                   ...
251          C#                     1
252  PostgreSQL                     1
253     MongoDB                     1
254       Scala                     2
255  JavaScript                     3

[256 rows x 2 columns]
Call the function for Python and check if it is working.

print(get_number_of_jobs('Python'))
('Python', 51)
Store the results in an excel file
Call the API for all the given technologies above and write the results in an excel spreadsheet.

If you do not know how create excel file using python, double click here for hints.

Create a python list of all technologies for which you need to find the number of jobs postings.

#your code goes here
technology=["C",184,"C#",14,"C++",24,"Java",83,"JavaScript",65,"Python",51,"Scala",47,"Oracle",8,"SQL Server",16,"MySQL Server",7,"PostgreSQL",17,"MongoDB",18]
print(technology)
['C', 184, 'C#', 14, 'C++', 24, 'Java', 83, 'JavaScript', 65, 'Python', 51, 'Scala', 47, 'Oracle', 8, 'SQL Server', 16, 'MySQL Server', 7, 'PostgreSQL', 17, 'MongoDB', 18]
Import libraries required to create excel spreadsheet

# your code goes here
from openpyxl import Workbook
Create a workbook and select the active worksheet

# your code goes here
wb=Workbook()
ws=wb.active
Find the number of jobs postings for each of the technology in the above list. Write the technology name and the number of jobs postings into the excel spreadsheet.

#your code goes here
ws.append(['technology','number of job posting'])
ws.append(['C',184])
ws.append(['C#',14])
ws.append(['C++',24])
ws.append(['Java',83])
ws.append(['JavaScript',65])
ws.append(['Python',51])
ws.append(['Scala',47])
ws.append(['Oracle',8])
ws.append(['SQL Server',16])
ws.append(['MySQL Server',7])
ws.append(['PostgreSQL',17])
ws.append(['MongoDB',18])
Save into an excel spreadsheet named 'github-job-postings.xlsx'.

#your code goes here
wb.save('github-job-postings.xlsx')
import os
os.getcwd()
​
from IPython.display import HTML
import base64,io
​
def create_download_link(df, title="Download CSV file", filename = "/home/wsuser/work/dataset_part_3.csv"):
    csv = df.to_csv()
    b64 = base64.b64encode(csv.encode())
    payload = b64.decode()
    html = '<a download="{filename}"href="data:text/csv;base64{payload}"target="_blank">{title}</a'
    html = html.format(payload=payload,title=title,filename=filename)
    return HTML(html)
​
create_download_link
<function __main__.create_download_link(df, title='Download CSV file', filename='/home/wsuser/work/dataset_part_3.csv')>
Authors
Ramesh Sannareddy

Other Contributors
Rav Ahuja

Change Log
Date (YYYY-MM-DD)	Version	Changed By	Change Description
2021-6-25	0.2	Malika	Updated GitHub job json link
2020-10-17	0.1	Ramesh Sannareddy	Created initial version of the lab
Copyright © 2020 IBM Corporation. This notebook and its source code are released under the terms of the MIT License.
