## Display a message
```python
print("Bonjour")
```

## Perform a calculation
```python
(100+81)*11
```
You can display the result directly in the print function : 
```python 
print((100+81)*11)
```

## Create a variable name + value and display it
```python
langage = "Python"
print(langage)
```
```python
formation = "AIS"
print(formation)
```
You can change the variable in a simple way : 
```powershell
langage = "Powershell"
```

## Using the f-string
f-string allows you to insert variables into the string to be displayed, it is a character string preceded by an f or F containing expressions between braces {}.
```python
prenom = "Nicolas"
langage = "Python"
jour = "vendredi"
print(f"Je m'appelle {prenom} et je révise sur {langage} chaque {jour}.")
```

## Understanding the data types
- integers (entiers)
- floats (virgules flottantes)
- strings (chaînes de caractères)
- booleans

### Numeric types 
- integers (nombres entiers) : 1 ; 16 ; 2024...
- floats (virgules flottantes) : 3.14 ; 99.9
```python
harry_potter_books = 7
croissant_price = 1.10
```

### Strings 
Information - usually a word - surrounded by quotation marks (single or double). 
```python
quote = "Ôte-toi de mon soleil."
```

### Booleans
Two options : ``True`` or ``False``.

To display a data type :
```python
print(f"Type name: {type(name)}")
```

## Save data groups with lists
Square brackets [] are used to indicate a list : 
```python
animals = ["dogs", "cats", "rabbits"]
```

### List items
You can get the proper index with the following command : ``list[index], starting from [0]``
```python
>>>animals[2]
'cats'
```
You can also use [-1] to start from the end. 

### Characters in a string as an item in a list
```python
langage = "Python"
langage[2]
't'

