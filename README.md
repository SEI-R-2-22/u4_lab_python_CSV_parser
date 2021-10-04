# Python CSV Parser

![](https://www.oflox.com/blog/wp-content/uploads/2021/01/CSV-To-JSON-Converter-768x432.png)

## Overview
In this lab, we'll be building a CSV parser with Python! This will help us get comfortable with using standard Python libraries, data manipulation, and working with tuples.

## Objectives
- Read and parse `csv` files into an easier to manipulate format.
- Build a functional Python CSV parser
- Write data to an easier to read json file

## Getting Started

- `Fork` and `clone` this repository
- We'll be working in the `parser.py` file

___
## Challenge

We have a raw csv file in our `data` folder. It contains useful information about employees for a company but the data isn't accessible in its current state. 

Our backend engineers would love to work with the data, but aren't having any success with it. This is why we're being hired, to convert the data into a useful JSON file!

How are we going to do this?
- First, we'll need to get the data from the CSV file
- Then we'll need to format each employee into a python dictionary
- Finally, we'll need to convert a list of employee dictionaries into a JSON file

## Setup

To start out, we'll need to import a few standard Python modules and create a few global variables at the top of our script

Let's add in our imports first at the top of the file

```py
import csv
import json
```

- Were importing the csv module to make reading and writing csv files more accessible. More on the csv module [here](https://docs.python.org/3/library/csv.html?highlight=csv#module-csv).
- We're importing the json library to give ourselves access to useful JSON encoding/decoding methods like `json.dump()` and `json.loads()`. More on the json module [here](https://docs.python.org/3/library/json.html?highlight=json#module-json)

Now let's add in a few global variables. They'll all be empty lists to start:

```py
read_data = []
# for csv data read and stored as tuples
fields = []
# for the name of each column in the csv file's rows
employee_list = []
# for formatted data, a dictionary for each row in the csv file
```

And with that, we've set up our script with the proper imports and global variables! We can move on to the fun part now - manipulating the data in our parsing function!

![](https://media0.giphy.com/media/sRFEa8lbeC7zbcIZZR/giphy.gif)

___
## CSV to JSON

Let's start out with defining our function. It should take in 2 arguments: one for the path of our CSV file, and on for an output path to create a new JSON file. This way, we'll be able to take in _any_ CSV file and convert it to JSON.

```py
def csv_to_json(csv_path, json_path):
  # your code here

...

csv_to_json('data/employee_data.csv','data/employees.json')
```

We'll write out each part of the function in the steps below. Make sure that each part is _within_ the indented block of `csv_to_json`!

___
### Extracting Data from the CSV File
The first thing we'll want to do here is extract the data from our CSV file. Once we've read the data, we'll append each entry into our `read_data` list.

But first, _how do we read a CSV file_?

```py
with open(csv_path, mode='r', encoding='utf-8-sig') as csv_file:
  csv_data = csv.reader(csv_file, delimiter=',')
```

- More on python's `open()` method [here](https://www.programiz.com/python-programming/methods/built-in/open).
- More on python `with ... as` statements [here](https://www.geeksforgeeks.org/with-statement-in-python/).
- More on using the csv module's `reader()` method [here](https://docs.python.org/3/library/csv.html#csv.reader).

With our read CSV data stored in `csv_data`, we can now add it into our `read_data` list. 

- After setting the data to `csv_data` you'll need to loop through each `entry in csv_data` and `append` each entry as a `tuple` to our `read_data` list

We're setting these entries as tuples to ensure that the original data entries aren't changed .

Finally, outside of our `with open` block, let's create two new variables for the headers of the CSV rows and the employee entries. These variables will be sub-lists of `read_data`.

- One variable should be for the `headers`, the column titles of the CSV data. 
<details><summary>HINT</summary>
  
  `print(data_list[0])` 
  
</details>

- The next variable should be for the `employees`, the data entries representing each employee in `data_list`. Since the `headers` will be at the first index of `read_data`, everything else should be an employee's data entries.
 everything after the first index of read_data


Try `print()`ing your `headers` variable below to check that you've set it correcty.

<details><summary>print(headers)</summary>

  ```
  ('Emp ID', 'First Name', 'Middle Initial', 'Last Name', 'Gender', 'EMail', 'Date of Birth', 'Age', 'Weight', 'Date of Joining', 'Quarter of Joining', 'Year of Joining', 'Month of Joining', 'Month Name of Joining', 'Short Month', 'Day of Joining', 'Age in Company', 'Salary', 'Last % Hike', 'SSN', 'Phone Num', 'Place Name', 'County', 'City', 'State', 'Zip')
  ```

</details>

___
### Formatting The Data

We know that we have access to the data in `read_data` now, but what about formatting it before we convert it into JSON?

First, let's format the `headers` and store them in our `fields` list in a more accessible form.

Since the `headers` are currently stored as a tuple, we'll need to use python's [enumerate()](https://www.bitdegree.org/learn/python-enumerate#working-with-tuples) method to iterate over them with a for loop.

<details><summary>HINT</summary>
  
  ```py
  for i, some_value in enumerate(some_tuple):
  ```

</details>

- Set a variable `name` to represent each header inside the loop. It's value should:
- Replace the empty spaces in each `header` with underscores and lowercase the whole string
- Append the formatted header `name` to our `fields` list


A few useful python string methods:
- [replace](https://www.programiz.com/python-programming/methods/string/replace)
- [lower](https://www.programiz.com/python-programming/methods/string/lower)

If done properly, your `fields` list should now have properly formatted python strings! Try printing the `fields` list below the loop to check.

<details><summary>print(fields)</summary>

  ```
  ['emp_id', 'first_name', 'middle_initial', 'last_name', 'gender', 'email', 'date_of_birth', 'age', 'weight', 'date_of_joining', 'quarter_of_joining', 'year_of_joining', 'month_of_joining', 'month_name_of_joining', 'short_month', 'day_of_joining', 'age_in_company', 'salary', 'last_%_hike', 'ssn', 'phone_num', 'place_name', 'county', 'city', 'state', 'zip']
  ```

</details>


Now that we have a list of the fields needed for each employee, let's add their data! We'll store each employee as a dictionary within `employee_list`.

 We'll need to loop through the `employees` list that we set as a variable above. Inside the loop we'll also need to `enumerate` through each employee tuple, just like we did with the `headers` above.  Inside the main loop of `employees`, the following needs to happen:

- An empty dictionary should be created for each `employee`
- Each employee's data entries should be accessed by `enumerating` over the `employee`
- For each `entry` in the enumerated `employee` loop, we'll need to create a key value pair for the dictionary
  - The `key` should be equal to the `field` at the same index as the `entry`
  - The `value` should be equal to the `entry`
- Update the dictionary with each key value pair for an employee's entries 
- After updating the dictionary, append it to our `employee_list` and clear the dictionary by setting it back to an empty dictionary so the next employee can be added

  
Once finished, try printing `employee_list[1]` to check if you're getting the right values. You should be getting a properly formatted dictionary in your console.


<details><summary>print(employee_list[1])</summary>

  ```
  {'emp_id': '178566', 
  'first_name': 'Juliette', 
  'middle_initial': 'M', 
  'last_name': 'Rojo', 
  'gender': 'F', 'email': 
  'juliette.rojo@yahoo.co.uk', 
  'date_of_birth': '5/8/1967', 
  'age': '50.26', 
  'weight': '55', 
  'date_of_joining': '6/4/2011', 
  'quarter_of_joining': 'Q2', 
  'year_of_joining': '2011', 
  'month_of_joining': '6', 
  'month_name_of_joining': 'June', 
  'short_month': 'Jun', 
  'day_of_joining': '4', 
  'age_in_company': '6.15', 
  'salary': '193912', 
  'last_%_hike': '27%', 
  'ssn': '671-48-9915', 
  'phone_num': '215-254-9594', 
  'place_name': 'Glenside', 
  'county': 'Montgomery', 
  'city': 'Glenside', 
  'state': 'PA', 
  'zip': '19038'}
  ```

</details>


If you're getting the right return, we're done with formatting! Yay!

___
## Converting to JSON
The final step in this process is converting our `employee_list` into JSON and writing it into a readable JSON file.

Like we did to open the CSV file to start, we'll use the `with open as` python statement, but this time we'll be giving it a mode of `'w'` to specify that we're _writing_ a new file.

```py
with open(json_path, mode='w') as output_file:
  
```

We've done two things here:
- Specified the path for a new json file we're creating from the path input in our function call
- Stored the file that we'll be writing to in a variable called `output_file`


We need to do one last thing to create our new `employees.json` file from the CSV data:
- Use the json package to [dump](https://docs.python.org/3/library/json.html?highlight=json#basic-usage) our `employee_list` data into our `output_file`. We'll give it an indent of 2 for it to be nicely formatted.

```py
with open(json_path, mode='w') as output_file:
  # json.dump(???, ???, indent=2)
```

If everything was done correctly, you should have a brand new, cleanly formatted `employees.json` file in your `data` folder! Congrats, we've done it!

___
## Requirements
- The `csv_to_json` function should create a new `employees.json` file with properly formatted data when it is run


## Bonus

- Read about one important reason that [csv files are used with python](https://machinelearningmastery.com/load-machine-learning-data-python/)


## Resources
- [python csv module](https://docs.python.org/3/library/csv.html?highlight=csv#module-csv)
- [python json module](https://docs.python.org/3/library/json.html?highlight=json#module-json)
- [python math library](https://docs.python.org/3/library/math.html)
- [python enumerating tuples](https://www.bitdegree.org/learn/python-enumerate#working-with-tuples)
- [python string.replace()](https://www.programiz.com/python-programming/methods/string/replace)
- [python string.lower()](https://www.programiz.com/python-programming/methods/string/lower)
