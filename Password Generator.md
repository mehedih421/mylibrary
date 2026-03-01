## The Simplest Password Generator

This is explanation text.

```python
import random 
import string 
# All possible characters 
characters = string.ascii_letters + string.digits + string.punctuation 
# Pick 12 random characters and join them 
password = ''.join(random.choices(characters, k=12)) 
print("Your password:", password)
```

## Basic Password Generator

```Python
import random
import string

def generate_password(length=12, use_upper=True, use_lower=True, use_digits=True, use_symbols=True):
    characters = ""
    
    if use_upper:
        characters += string.ascii_uppercase
    if use_lower:
        characters += string.ascii_lowercase
    if use_digits:
        characters += string.digits
    if use_symbols:
        characters += string.punctuation

    if not characters:
        raise ValueError("At least one character type must be selected!")

    password = ''.join(random.choice(characters) for _ in range(length))
    return password

# Example usage
print(generate_password(length=16))
```

## Better Version (Guaranteed Character Types)

```Python
import random
import string

def generate_password(length=12, use_upper=True, use_lower=True, use_digits=True, use_symbols=True):
    if length < 4:
        raise ValueError("Password length should be at least 4.")

    pool = ""
    guaranteed = []

    if use_upper:
        pool += string.ascii_uppercase
        guaranteed.append(random.choice(string.ascii_uppercase))
    if use_lower:
        pool += string.ascii_lowercase
        guaranteed.append(random.choice(string.ascii_lowercase))
    if use_digits:
        pool += string.digits
        guaranteed.append(random.choice(string.digits))
    if use_symbols:
        pool += string.punctuation
        guaranteed.append(random.choice(string.punctuation))

    if not pool:
        raise ValueError("Select at least one character type.")

    remaining = [random.choice(pool) for _ in range(length - len(guaranteed))]
    password_list = guaranteed + remaining
    random.shuffle(password_list)

    return ''.join(password_list)

# Test
print(generate_password(length=16, use_symbols=True))
```

## Interactive CLI Version

```Python
import random
import string

def generate_password(length, use_upper, use_lower, use_digits, use_symbols):
    pool = ""
    guaranteed = []

    if use_upper:
        pool += string.ascii_uppercase
        guaranteed.append(random.choice(string.ascii_uppercase))
    if use_lower:
        pool += string.ascii_lowercase
        guaranteed.append(random.choice(string.ascii_lowercase))
    if use_digits:
        pool += string.digits
        guaranteed.append(random.choice(string.digits))
    if use_symbols:
        pool += string.punctuation
        guaranteed.append(random.choice(string.punctuation))

    remaining = [random.choice(pool) for _ in range(length - len(guaranteed))]
    password_chars = guaranteed + remaining
    random.shuffle(password_chars)
    return ''.join(password_chars)

def get_yes_no(prompt):
    return input(prompt).strip().lower() in ('y', 'yes', '')

def main():
    print("=== Password Generator ===")
    length = int(input("Enter password length (default 12): ") or 12)
    use_upper   = get_yes_no("Include uppercase letters? (Y/n): ")
    use_lower   = get_yes_no("Include lowercase letters? (Y/n): ")
    use_digits  = get_yes_no("Include digits? (Y/n): ")
    use_symbols = get_yes_no("Include symbols? (Y/n): ")

    how_many = int(input("How many passwords to generate? (default 1): ") or 1)

    print("\nGenerated Passwords:")
    for i in range(how_many):
        pw = generate_password(length, use_upper, use_lower, use_digits, use_symbols)
        print(f"  {i+1}. {pw}")

if __name__ == "__main__":
    main()
```