# Session-13-Assignment

Deepnote link:  [Deepnote](https://deepnote.com/project/Starter-Project-3hCGzG0RTV2p2HFU7WcdPw/%2FAssignment_13.ipynb)

### ASSIGNMENT:

##### Goal 1

Create a lazy iterator that will return a named tuple of the data in each row. The data types should be appropriate - i.e. if the column is a date, you should be storing dates in the named tuple, if the field is an integer, then it should be stored as an integer, etc.

##### Goal 2

Calculate the number of violations by car make.

### Solution:



***Goal 1***

Initially all the data is type converted respectively, then added to a namedtuple.



```
def type_cast(data_type, value):
    """
    type_cast converts the data into respective datatypes
    """
    if data_type == 'INT':
        return int(value)
    elif data_type =='DATE':
        return datetime.strptime(value,'%m/%d/%Y').date()
    else:
        return str(value)
        
data_types = ['INT', 'STRING', 'STRING', 'STRING', 'DATE', 'INT', 'STRING', 'STRING', 'STRING']

def type_cast_row(data_types: 'list', data_row:'iterable'):
    """
    type_cast_row convert elements of each data_row into respective data types 
    """
    return [type_cast(type_, value) 
            for type_, value in zip(data_types, data_row)]
```



```
file = 'nyc_parking_tickets_extract.csv'

def read_cars(file):
    """
    This function takes a file as input and generates an generator object.
    this generator object returns a namedtuple
    """
    with open(file) as files:
        file_iter = iter(files)
        headers = next(file_iter).strip('\n').split(',')
        header = [string.replace(" ","_") for string in headers]
        Car = namedtuple('Car', header)
        for line in file_iter:
            data = line.strip('\n').split(',')
            data = type_cast_row(data_types, data)
            car = Car(*data)
            yield car
```





***Goal 2***

A dictionary is used to store the number of violations made by each Vehicle_make

```
gen = read_cars(file)
def count_violations(generator):
    """
    this function returns a dictionary which contains the count of
    violations by each car_make
    """
    violations = {}
    for cars in generator:
        if len(cars.Violation_Description) > 0:
            if cars.Vehicle_Make in list(violations.keys()):
                violations[cars.Vehicle_Make] += 1
            else:
                violations[cars.Vehicle_Make] = 1

    return violations
    
violation = count_violations(gen)
```

