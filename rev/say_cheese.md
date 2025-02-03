# Say Cheese

Say Cheese is a reverse engineering challenge focused on a python script given straight in the challenge prompt. It is as follows...

```py
import base64

def encoder(input_str, key):
    encoded_chars = []
    for i in range(len(input_str)):
        key_c = key[i % len(key)]
        encoded_c = chr((ord(input_str[i]) + ord(key_c)) % 256)
        encoded_chars.append(encoded_c)
    encoded_str = ''.join(encoded_chars)
    return base64.b64encode(encoded_str.encode()).decode()

def main():
    print("""         _--"-.
      .-"      "-.
     |""--..      '-.
     |      ""--..   '-.
     |.-. .-".    ""--..".
     |'./  -_'  .-.      |
     |      .-. '.-'   .-'
     '--..  '.'    .-  -.
          ""--..   '_'   :
                ""--..   |
                      ""-' """)
    ciphertext = "wpbCi8KIwpfCh8OPwph5wqnCosK6woTCqcKnwq13wrfCh8KzwqnCpMKKccOJwrh8wqTCl3LDgcKHw4U="
    key = "THECAT"
    inp = input("Enter your cat: ")
    encoded_flag = encoder(inp, key)
    if ciphertext == encoded_flag:
        print("YAY you found the cat!")
    else:
        print("Not so easy cheesy huh?")

if __name__ == "__main__":
    main()
```

## Code Breakdown
1. When executing the code (`python3 cheese.py`) it starts by printing a block of cheese and prompts the user for an input.
2. Whatever you enter, it encodes via the `encoder()` function and checks if it's equal to the cipher text.
3. If equal, it'll print "YAY you found the cat!". If not, it says "Not so easy cheesy huh?"

## Analysis
The first thing that I realized is that the code will never give you the flag, even if you type the write cat into the prompt.
ETC 
I figured that the flag must be hidden in the ciphertext and that I need to write a decoder so I can decrypt the ciphertext into a usable flag.

### encoder()
- `encoder()` take in an `input_str` (string) and `key` (string)
- then it defines `encoded_chars` as an array for the encoded characters to live
- then a for loop loops through the range of `0` to `len(input_str)`
	- within the loop we get the value of the key at `i % len(key)` allowing for a smaller key to still encode a large string
	-  `ord(input_str[i])` and `ord(key_c)` turns the value at `i` of the `input_str` and `key_c` into its Unicode integer value
	- it then adds together these values and modulo's by 256 to ensure a signle byte of data is not exceeded
	- `chr()` then turns it back into a character
	- and it is appended to the array (`encoded_chars`)
- then the array is joined with no joining value to form `encoded_str`
- the encoded string is then is then turned into bytes (via `.encode()`), base64 encoded (via `base64.b64encode()`), and then is decoded from bytes to a string (via `.decode()`)
- the string is then finally returned to be used elsewhere in the code

## Building a Decoder
- we first have to pull in an `encoded_str` and the `key` (both of which we know from the previous code)
- unlike `base64.b64encode()` we can use a string in `base64.b64decode()` so we don't have to encode our `encoded_str`, but we do have to decode it back to a string after the decode
- then we have to loop through the `encoded_chars` (the result of the base 64 decode process
	- we'll do mostly the same things as the encoded
	- however we'll subtract the key character value instead of adding it
- joining it all together gets us the results we want

### Decoder Code
```py
import base64

def decoder(encoded_str, key):
    encoded_chars = base64.b64decode(encoded_str).decode()
    decoded_chars = []

    for i in range(len(encoded_chars)):
        key_c = key[i % len(key)]
        decoded_c = chr((ord(encoded_chars[i]) - ord(key_c)) % 256)
        decoded_chars.append(decoded_c)
        
    return ''.join(decoded_chars)
```

## Solution
- using the decoder with `encoded_str`=`"wpbCi8KIwpfCh8OPwph5wqnCosK6woTCqcKnwq13wrfCh8KzwqnCpMKKccOJwrh8wqTCl3LDgcKHw4U"` and `key`=`"THECAT"` we can run our decoder
- the decoder then returns our flag in the BCCTF{<flag\>} format
- using the flag we found we can try it again in the original program and now we get the text "YAY you found the cat!" printed to our screen!



