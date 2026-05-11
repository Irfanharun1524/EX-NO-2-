## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

 

## AIM:
 

 

To write a C program to implement the Playfair Substitution technique.

## DESCRIPTION:

The Playfair cipher starts with creating a key table. The key table is a 5×5 grid of letters that will act as the key for encrypting your plaintext. Each of the 25 letters must be unique and one letter of the alphabet is omitted from the table (as there are 25 spots and 26 letters in the alphabet).

To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. The two letters of the diagram are considered as the opposite corners of a rectangle in the key table. Note the relative position of the corners of this rectangle. Then apply the following 4 rules, in order, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair.
## EXAMPLE:
![image](https://github.com/Hemamanigandan/EX-NO-2-/assets/149653568/e6858d4f-b122-42ba-acdb-db18ec2e9675)

 

## ALGORITHM:

STEP-1: Read the plain text from the user.
STEP-2: Read the keyword from the user.
STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.
STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.
STEP-5: Display the obtained cipher text.




Program:
```
SIZE = 5

def generate_key_matrix(key):
    key = key.upper().replace('J', 'I')
    seen = set()
    filtered = []
    for ch in key:
        if ch.isalpha() and ch not in seen:
            seen.add(ch)
            filtered.append(ch)
    
    for ch in 'ABCDEFGHIKLMNOPQRSTUVWXYZ':
        if ch not in seen:
            seen.add(ch)
            filtered.append(ch)
    
    matrix = [filtered[i*SIZE:(i+1)*SIZE] for i in range(SIZE)]
    return matrix

def find_position(matrix, ch):
    if ch == 'J':
        ch = 'I'
    for i in range(SIZE):
        for j in range(SIZE):
            if matrix[i][j] == ch:
                return i, j

def process_digraph(a, b, matrix, encrypt):
    r1, c1 = find_position(matrix, a)
    r2, c2 = find_position(matrix, b)
    step = 1 if encrypt else SIZE - 1

    if r1 == r2:
        return matrix[r1][(c1 + step) % SIZE], matrix[r2][(c2 + step) % SIZE]
    elif c1 == c2:
        return matrix[(r1 + step) % SIZE][c1], matrix[(r2 + step) % SIZE][c2]
    else:
        return matrix[r1][c2], matrix[r2][c1]

def preprocess_text(text):
    text = text.upper().replace('J', 'I')
    return ''.join(ch for ch in text if ch.isalpha())

def encrypt_decrypt(text, matrix, encrypt):
    text = preprocess_text(text)
    result = []
    i = 0
    while i < len(text):
        a = text[i]
        if i + 1 < len(text):
            b = text[i + 1]
        else:
            b = 'X'

        if a == b:
            b = 'X'
            i += 1
        else:
            i += 2

        res_a, res_b = process_digraph(a, b, matrix, encrypt)
        result.extend([res_a, res_b])

    return ''.join(result)

def print_matrix(matrix):
    print("KEY MATRIX:")
    for row in matrix:
        print(' '.join(row))

def main():
    key = input("ENTER THE KEY: ")
    text = input("ENTER TEXT TO ENCRYPT: ")

    matrix = generate_key_matrix(key)
    print_matrix(matrix)

    encrypted = encrypt_decrypt(text, matrix, encrypt=True)
    print(f"ENCRYPTED TEXT: {encrypted}")

    decrypted = encrypt_decrypt(encrypted, matrix, encrypt=False)
    print(f"DECRYPTED TEXT: {decrypted}")

if __name__ == "__main__":
    main()
```




Output:

<img width="428" height="1015" alt="Screenshot 2026-05-11 092211" src="https://github.com/user-attachments/assets/7a433083-320e-48ce-bb10-f1f1e41e471c" />
