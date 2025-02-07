# City Crack
International hackers have left two different encrypted hashes behind. The first hash is `1fae39b5bc83699450281dc7bb472d59`, and the second hash is `5f6b9145ac86d19e917a08e6c20de0c907472a90`. To find the flag, decrypt both hashes and combine the results in alphabetical order, separated by an underscore. These paswords are based off of capital cites with some modifications.

## Analysis
Upon first glance, the first hash is 32 characters so is MD5, while the second is 40 characters so is SHA-1. It seems we will have to create a list of capital cities and compare them against the given hash - quick and easy bruteforce right? However, it says "with some modifications", which leads me to believe it won't be that simple. 

## First Attempt

I first tried the most obvious method, and compared the names of the capital cities directly with the hashes, as seen in the code below:


```python
import hashlib

capital_cities = [
    "Abu_Dhabi", "Accra", "Addis_Ababa", "Algiers", "Amman", "Ankara", "Antananarivo", "Apia", "Ashgabat", "Asmara",
    "Astana", "Athens", "Baghdad", "Baku", "Bamako", "Bangkok", "Bangui", "Beijing", "Beirut", "Belgrade", "Belmopan", "Berlin",
    "Bern", "Bishkek", "Bogota", "Brasilia", "Bratislava", "Bridgetown", "Brussels", "Bucharest", "Budapest", "Bujumbura",
    "Cairo", "Canberra", "Caracas", "Chisinau", "Copenhagen", "Dakar", "Damascus", "Dhaka", "Dili", "Djibouti", "Dublin",
    "Dushanbe", "Edinburgh", "Freetown", "Funafuti", "Gaborone", "Georgetown", "Hanoi", "Harare", "Havana", "Helsinki",
    "Honiara", "Islamabad", "Jakarta", "Kabul", "Kampala", "Kathmandu", "Khartoum", "Kigali", "Kingston", "Kinshasa",
    "Kuwait_City", "Kyiv", "La_Paz", "Lima", "Lisbon", "Ljubljana", "London", "Luanda", "Lusaka", "Luxembourg", "Madrid",
    "Malabo", "Male", "Manama", "Manila", "Maputo", "Maseru", "Minsk", "Mogadishu", "Monaco", "Monrovia", "Montevideo",
    "Moscow", "Muscat", "Nairobi", "Nassau", "New_Delhi", "Niamey", "Nicosia", "Nouakchott", "Nuku'alofa",
    "Oslo", "Ottawa", "Panama_City", "Paramaribo", "Paris", "Phnom_Penh", "Podgorica", "Port_Louis", "Port_Moresby",
    "Port-au-Prince", "Porto-Novo", "Prague", "Pretoria", "Pyongyang", "Quito", "Rabat", "Reykjavik", "Riga", "Riyadh",
    "Rome", "Roseau", "Saint_George's", "Saint_John's", "San_Jose", "San_Marino", "San_Salvador", "Sanaa", "Santiago",
    "Santo_Domingo", "Sao_Tome", "Sarajevo", "Seoul", "Singapore", "Skopje", "Sofia", "Stockholm", "Suva", "Taipei",
    "Tallinn", "Tashkent", "Tbilisi", "Tegucigalpa", "Tehran", "Thimphu", "Tokyo", "Tripoli", "Tunis", "Ulaanbaatar",
    "Vaduz", "Valletta", "Vienna", "Vientiane", "Vilnius", "Warsaw", "Washington_D.C.", "Wellington", "Windhoek", "Yaounde",
    "Yerevan", "Zagreb"
]

md5_hash = 1fae39b5bc83699450281dc7bb472d59
sha1_hash = 5f6b9145ac86d19e917a08e6c20de0c907472a90



for city in capital_cities:

    md5_attempt = hashlib.md5(city.encode()).hexdigest()
    sha1_attempt = hashlib.sha1(city.encode()).hexdigest()

    if md5_attempt == md5_hash:
        print(f"MD5 Match: City: {city} Hash: {md5_attempt}")

    if sha1_attempt == sha1_hash:
        print(f"SHA-1 Match: City: {city} Hash: {sha1_attempt}")

print("No match found")
```


But alas, 'twas too good to be true.


# Final Solution

Now, let's try addressing the "modifications" in question. It is most likely that the characters in each city have been substituted while still preserving their original purpose. Since we have no frame of reference to the scope of the changes, a safe bet would be to play around with capitalization and experiment with leet speak. I created a dictionary:

```python

all_substitutions = {
    'a': ['4', '@', 'a', 'A'],
    'b': ['8', '6' 'b', 'B'],
    'c': ['(', 'c', 'C'],
    'd': [ 'd', 'D'],
    'e': ['3', 'e', 'E'],
    'f': ['f', 'F'],
    'g': ['6', '9', '&', 'g', 'G'],
    'h': ['#', 'h', 'H'],
    'i': ['1', '!', 'i', 'I'],
    'j': [ 'j', 'J'],
    'k': [ 'X', 'k', 'K'],
    'l': ['1', 'l', 'L'],
    'm': ['m', 'M'],
    'n': [ 'n', 'N'],
    'o': ['0', 'o', 'O'],
    'p': [ 'p', 'P'],
    'q': ['9', 'q', 'Q'],
    'r': [ 'r', 'R'],
    's': ['5', '$', 'z', 's', 'S'],
    't': ['7', 't', 'T'],
    'u': [ 'v', 'u', 'U'],
    'v': ['v', 'V'],
    'w': [ 'w', 'W'],
    'x': [ 'x', 'X'],
    'y': [ 'y', 'Y'],
    'z': ['2', '%', 'z', 'Z'],

```

It was originally far more comprehensive, but it took an absurd amount of time to compute all possibilities for each city name, so I narrowed it down to more obvious substitutions. I experimented with substiuting one character at a time, and only the vowels, but it had yielded no results. I decided to employ the nuclear option and simply try **e v e r y t h i n g**. As I was creating this program, however, I was quickly losing the last vestiges of my sanity. So I settled on using the itertools library to make the process far easier. The final result is shown below:


```python

import hashlib
from itertools 

capital_cities = [
    "Abu_Dhabi", "Accra", "Addis_Ababa", "Algiers", "Amman", "Ankara", "Antananarivo", "Apia", "Ashgabat", "Asmara",
    "Astana", "Athens", "Baghdad", "Baku", "Bamako", "Bangkok", "Bangui", "Beijing", "Beirut", "Belgrade", "Belmopan", "Berlin",
    "Bern", "Bishkek", "Bogota", "Brasilia", "Bratislava", "Bridgetown", "Brussels", "Bucharest", "Budapest", "Bujumbura",
    "Cairo", "Canberra", "Caracas", "Chisinau", "Copenhagen", "Dakar", "Damascus", "Dhaka", "Dili", "Djibouti", "Dublin",
    "Dushanbe", "Edinburgh", "Freetown", "Funafuti", "Gaborone", "Georgetown", "Hanoi", "Harare", "Havana", "Helsinki",
    "Honiara", "Islamabad", "Jakarta", "Kabul", "Kampala", "Kathmandu", "Khartoum", "Kigali", "Kingston", "Kinshasa",
    "Kuwait_City", "Kyiv", "La_Paz", "Lima", "Lisbon", "Ljubljana", "London", "Luanda", "Lusaka", "Luxembourg", "Madrid",
    "Malabo", "Male", "Manama", "Manila", "Maputo", "Maseru", "Minsk", "Mogadishu", "Monaco", "Monrovia", "Montevideo",
    "Moscow", "Muscat", "Nairobi", "Nassau", "New_Delhi", "Niamey", "Nicosia", "Nouakchott", "Nuku'alofa",
    "Oslo", "Ottawa", "Panama_City", "Paramaribo", "Paris", "Phnom_Penh", "Podgorica", "Port_Louis", "Port_Moresby",
    "Port-au-Prince", "Porto-Novo", "Prague", "Pretoria", "Pyongyang", "Quito", "Rabat", "Reykjavik", "Riga", "Riyadh",
    "Rome", "Roseau", "Saint_George's", "Saint_John's", "San_Jose", "San_Marino", "San_Salvador", "Sanaa", "Santiago",
    "Santo_Domingo", "Sao_Tome", "Sarajevo", "Seoul", "Singapore", "Skopje", "Sofia", "Stockholm", "Suva", "Taipei",
    "Tallinn", "Tashkent", "Tbilisi", "Tegucigalpa", "Tehran", "Thimphu", "Tokyo", "Tripoli", "Tunis", "Ulaanbaatar",
    "Vaduz", "Valletta", "Vienna", "Vientiane", "Vilnius", "Warsaw", "Washington_D.C.", "Wellington", "Windhoek", "Yaounde",
    "Yerevan", "Zagreb"
]

all_substitutions = {
    'a': ['4', '@', 'a', 'A'],
    'b': ['8', '6' 'b', 'B'],
    'c': ['(', 'c', 'C'],
    'd': [ 'd', 'D'],
    'e': ['3', 'e', 'E'],
    'f': ['f', 'F'],
    'g': ['6', '9', '&', 'g', 'G'],
    'h': ['#', 'h', 'H'],
    'i': ['1', '!', 'i', 'I'],
    'j': [ 'j', 'J'],
    'k': [ 'X', 'k', 'K'],
    'l': ['1', 'l', 'L'],
    'm': ['m', 'M'],
    'n': [ 'n', 'N'],
    'o': ['0', 'o', 'O'],
    'p': [ 'p', 'P'],
    'q': ['9', 'q', 'Q'],
    'r': [ 'r', 'R'],
    's': ['5', '$', 'z', 's', 'S'],
    't': ['7', 't', 'T'],
    'u': [ 'v', 'u', 'U'],
    'v': ['v', 'V'],
    'w': [ 'w', 'W'],
    'x': [ 'x', 'X'],
    'y': [ 'y', 'Y'],
    'z': ['2', '%', 'z', 'Z'],


md5_hash = "1fae39b5bc83699450281dc7bb472d59"
sha1_hash = "5f6b9145ac86d19e917a08e6c20de0c907472a90"

for city in capital_cities:

    char_options = [all_substitutions.get(char.lower(), [char]) for char in city]

    for combo in product(*char_options):

        attempt = ''.join(combo)

        

        
        md5_attempt = hashlib.md5(attempt.encode()).hexdigest()
        sha1_attempt = hashlib.sha1(attempt.encode()).hexdigest()

        
        if md5_attempt == md5_hash:
            print(f"MD5 Match - City: {city}, Combination: {attempt} Hash: {md5_attempt}")
        
        if sha1_attempt == sha1_hash:
            print(f"SHA-1 Match - City: {city}, Combination: {attempt} Hash: {sha1_attempt}")


print("No matching combination found.")

```

## Breakdown and Result
Essentially, a list of possible substitutions is generated for each character in the city name. Then, `itertools.product` computes the Cartesian product of these substitutions, producing every possible modified version of the word. The two modified capital names to be combined into the final flag are:  **T0kyO** from the MD5 hash and **p@R!s** from the SHA-1 hash.

