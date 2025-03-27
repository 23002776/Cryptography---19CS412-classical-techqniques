Name:Abinaya A
Reg no:212223040003

Hill Cipher

Hill Cipher using with different key values

AIM:
To develop a simple C program to implement Hill Cipher.

DESIGN STEPS:
Step 1:
Design of Hill Cipher algorithnm

Step 2:
Implementation using C or pyhton code

Step 3:
Testing algorithm with different key values. ALGORITHM DESCRIPTION: The Hill cipher is a substitution cipher invented by Lester S. Hill in 1929. Each letter is represented by a number modulo 26. To encrypt a message, each block of n letters is multiplied by an invertible n × n matrix, again modulus 26. To decrypt the message, each block is multiplied by the inverse of the matrix used for encryption. The matrix used for encryption is the cipher key, and it should be chosen randomly from the set of invertible n × n matrices (modulo 26). The cipher can, be adapted to an alphabet with any number of letters. All arithmetic just needs to be done modulo the number of letters instead of modulo 26.

PROGRAM:
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define SIZE 5

void generateKeyMatrix(char key[], char matrix[SIZE][SIZE]) {
    int alpha[26] = {0};
    int i, j, k = 0;
    char current;

    // Remove duplicates and replace 'J' with 'I'
    for (i = 0; key[i] != '\0'; i++) {
        current = toupper(key[i]);
        if (current == 'J') current = 'I';
        if (current < 'A' || current > 'Z' || alpha[current - 'A'])
            continue;
        alpha[current - 'A'] = 1;
        key[k++] = current;
    }
    key[k] = '\0';

    // Fill matrix with key characters and remaining alphabet
    i = 0; k = 0;
    for (int row = 0; row < SIZE; row++) {
        for (int col = 0; col < SIZE; col++) {
            if (i < strlen(key)) {
                matrix[row][col] = key[i++];
            } else {
                for (char ch = 'A'; ch <= 'Z'; ch++) {
                    if (ch == 'J' || alpha[ch - 'A'])
                        continue;
                    matrix[row][col] = ch;
                    alpha[ch - 'A'] = 1;
                    break;
                }
            }
        }
    }
}

void findPosition(char matrix[SIZE][SIZE], char ch, int *row, int *col) {
    if (ch == 'J') ch = 'I';  // Treat 'J' as 'I'
    for (int i = 0; i < SIZE; i++) {
        for (int j = 0; j < SIZE; j++) {
            if (matrix[i][j] == ch) {
                *row = i;
                *col = j;
                return;
            }
        }
    }
}

void processDigraph(char a, char b, char matrix[SIZE][SIZE], char *resA, char *resB, int encrypt) {
    int row1, col1, row2, col2;
    findPosition(matrix, a, &row1, &col1);
    findPosition(matrix, b, &row2, &col2);

    if (row1 == row2) {  // Same row
        *resA = matrix[row1][(col1 + (encrypt ? 1 : SIZE - 1)) % SIZE];
        *resB = matrix[row2][(col2 + (encrypt ? 1 : SIZE - 1)) % SIZE];
    } else if (col1 == col2) {  // Same column
        *resA = matrix[(row1 + (encrypt ? 1 : SIZE - 1)) % SIZE][col1];
        *resB = matrix[(row2 + (encrypt ? 1 : SIZE - 1)) % SIZE][col2];
    } else {  // Rectangle swap
        *resA = matrix[row1][col2];
        *resB = matrix[row2][col1];
    }
}

void preprocessText(char *text) {
    char temp[100] = {0};
    int k = 0;

    for (int i = 0; text[i]; i++) {
        if (isalpha(text[i])) {
            temp[k++] = toupper(text[i] == 'J' ? 'I' : text[i]);
        }
    }
    temp[k] = '\0';
    strcpy(text, temp);
}

void encryptDecryptText(char *text, char matrix[SIZE][SIZE], int encrypt) {
    preprocessText(text);
    char result[100] = {0};
    int len = strlen(text), k = 0;

    for (int i = 0; i < len; i += 2) {
        char a = text[i];
        char b = (i + 1 < len) ? text[i + 1] : 'X';

        if (a == b) {
            b = 'X';  // Insert 'X' between identical letters
            i--;      // Re-evaluate second character
        }

        char resA, resB;
        processDigraph(a, b, matrix, &resA, &resB, encrypt);
        result[k++] = resA;
        result[k++] = resB;
    }
    result[k] = '\0';
    strcpy(text, result);
}

void printMatrix(char matrix[SIZE][SIZE]) {
    printf("Key Matrix:\n");
    for (int i = 0; i < SIZE; i++) {
        for (int j = 0; j < SIZE; j++) {
            printf("%c ", matrix[i][j]);
        }
        printf("\n");
    }
}

int main() {
    char key[100], text[100], matrix[SIZE][SIZE];
    
    printf("Enter the key: ");
    fgets(key, sizeof(key), stdin);
    key[strcspn(key, "\n")] = '\0';

    printf("Enter text to encrypt: ");
    fgets(text, sizeof(text), stdin);
    text[strcspn(text, "\n")] = '\0';

    generateKeyMatrix(key, matrix);
    printMatrix(matrix);

    encryptDecryptText(text, matrix, 1);  // Encrypt
    printf("Encrypted Text: %s\n", text);

    encryptDecryptText(text, matrix, 0);  // Decrypt
    printf("Decrypted Text: %s\n", text);

    return 0;
}
```
OUTPUT:

![Screenshot (97)](https://github.com/user-attachments/assets/f3b4ad7a-351f-49a2-82f3-718cb74f76d7)

RESULT:
The program is executed successfully



