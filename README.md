# Password Checker Documentation

## Overview
This script is a simple password checker that utilizes the Have I Been Pwned (HIBP) API to check whether a given password has been exposed in any known data breaches. The HIBP API allows querying for password hashes to determine if they have been previously compromised.

## How It Works
The script works by taking one or more passwords as command-line arguments and then performs the following steps for each password:

1. **Hashing the Password**: The script uses the SHA-1 hashing algorithm to convert the password into a hash. This hash is then converted to uppercase for consistency.

2. **Dividing the Hash**: The first five characters of the hashed password are extracted as the prefix, while the remaining characters are stored as the tail.

3. **Requesting API Data**: The script constructs a URL using the prefix and sends a request to the HIBP API to retrieve a list of hashed passwords with the same prefix.

4. **Checking for Leaks**: The script then searches the response from the API to find a match for the tail of the hashed password. If a match is found, it indicates that the password has been previously leaked.

5. **Displaying Results**: Depending on the result, the script prints a message indicating whether the password has been found in any breaches. If found, it suggests changing the password; otherwise, it encourages the user to continue using the password.

## Requirements
- Python 3.x
- `requests` library (can be installed using `pip install requests`)

## Usage
The script is designed to be run from the command line. Example usage:

```bash
python password_checker.py password1 password2 secret123
```

## Functions

### `request_api_data(query_char)`
- **Input**: `query_char` - The first five characters of the hashed password.
- **Output**: A response object from the HIBP API containing a list of hashed passwords with the same prefix.

### `get_password_leaks_count(hashes, hash_to_check)`
- **Input**: 
  - `hashes` - The response object from the HIBP API containing a list of hashed passwords.
  - `hash_to_check` - The tail of the hashed password to check for in the list.
- **Output**: The number of times the password hash was found in the data breaches. If not found, returns 0.

### `pwned_api_check(password)`
- **Input**: `password` - The plaintext password to check.
- **Output**: The number of times the password has been found in data breaches. If not found, returns 0.

### `main(args)`
- **Input**: `args` - Command-line arguments representing passwords to check.
- **Output**: Prints the results of the password checks and returns 'done!'.

### `__main__` Block
The script checks if it is being run directly and then calls the `main` function with command-line arguments. The script exits with a return code, indicating the success or failure of the execution.

## Example Output
```
$ python password_checker.py password1 secret123
password1 was NOT found. Carry on!
secret123 was found 2211 times... you should probably change your password!
```

## Error Handling
- If there is an issue fetching data from the HIBP API (e.g., non-200 status code), a `RuntimeError` is raised.

## Notes
- The script does not send the actual password to the HIBP API. Instead, it sends the first five characters of the hashed password, preserving a level of security.

## Security Considerations
- Always use the latest version of the script and libraries to ensure the latest security updates.
- Do not store or log actual passwords. The script only processes hashed values for security reasons.
- Exercise caution when dealing with passwords and sensitive information.
