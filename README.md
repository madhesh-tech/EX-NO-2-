## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER
## Name :MADHESH I
## Reg No:212224220055
 

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
#include <stdio.h>
#include <string.h>
#include <ctype.h>

char keyTable[5][5];

// Generate the 5x5 key matrix
void generateKeyTable(char key[])
{
    int used[26] = {0};
    int i, k = 0;

    for (i = 0; key[i] != '\0'; i++)
    {
        if (key[i] == 'J')
            key[i] = 'I';

        if (!used[key[i] - 'A'])
        {
            keyTable[k / 5][k % 5] = key[i];
            used[key[i] - 'A'] = 1;
            k++;
        }
    }

    for (i = 0; i < 26; i++)
    {
        if (i + 'A' == 'J')
            continue;

        if (!used[i])
        {
            keyTable[k / 5][k % 5] = i + 'A';
            k++;
        }
    }
}

// Find row and column of a character
void findPosition(char ch, int *row, int *col)
{
    int i, j;

    if (ch == 'J')
        ch = 'I';

    for (i = 0; i < 5; i++)
        for (j = 0; j < 5; j++)
            if (keyTable[i][j] == ch)
            {
                *row = i;
                *col = j;
                return;
            }
}

// Encrypt plaintext
void encrypt(char text[])
{
    int i, r1, c1, r2, c2;

    printf("Encrypted Text: ");
    for (i = 0; i < strlen(text); i += 2)
    {
        findPosition(text[i], &r1, &c1);
        findPosition(text[i + 1], &r2, &c2);

        if (r1 == r2) // Same row
        {
            printf("%c%c",
                   keyTable[r1][(c1 + 1) % 5],
                   keyTable[r2][(c2 + 1) % 5]);
        }
        else if (c1 == c2) // Same column
        {
            printf("%c%c",
                   keyTable[(r1 + 1) % 5][c1],
                   keyTable[(r2 + 1) % 5][c2]);
        }
        else // Rectangle
        {
            printf("%c%c",
                   keyTable[r1][c2],
                   keyTable[r2][c1]);
        }
    }
}

// Decrypt cipher text
void decrypt(char text[])
{
    int i, r1, c1, r2, c2;

    printf("\nDecrypted Text: ");
    for (i = 0; i < strlen(text); i += 2)
    {
        findPosition(text[i], &r1, &c1);
        findPosition(text[i + 1], &r2, &c2);

        if (r1 == r2) // Same row
        {
            printf("%c%c",
                   keyTable[r1][(c1 + 4) % 5],
                   keyTable[r2][(c2 + 4) % 5]);
        }
        else if (c1 == c2) // Same column
        {
            printf("%c%c",
                   keyTable[(r1 + 4) % 5][c1],
                   keyTable[(r2 + 4) % 5][c2]);
        }
        else // Rectangle
        {
            printf("%c%c",
                   keyTable[r1][c2],
                   keyTable[r2][c1]);
        }
    }
}

int main()
{
    char key[25], plainText[50], cipherText[50];
    int i, len;

    printf("Enter the key: ");
    scanf("%s", key);

    // Convert key to uppercase
    for (i = 0; key[i]; i++)
        key[i] = toupper(key[i]);

    generateKeyTable(key);

    printf("Enter the plaintext: ");
    scanf("%s", plainText);

    // Convert plaintext to uppercase
    for (i = 0; plainText[i]; i++)
        plainText[i] = toupper(plainText[i]);

    // Handle repeated letters and odd length
    len = strlen(plainText);
    for (i = 0; i < len; i += 2)
    {
        if (plainText[i] == plainText[i + 1])
        {
            memmove(&plainText[i + 2], &plainText[i + 1], len - i);
            plainText[i + 1] = 'X';
            len++;
        }
    }
    if (len % 2 != 0)
    {
        plainText[len] = 'X';
        plainText[len + 1] = '\0';
    }

    encrypt(plainText);

    printf("\nEnter the cipher text to decrypt: ");
    scanf("%s", cipherText);

    for (i = 0; cipherText[i]; i++)
        cipherText[i] = toupper(cipherText[i]);

    decrypt(cipherText);

    return 0;
}
```




Output:
<img width="1608" height="795" alt="image" src="https://github.com/user-attachments/assets/7539173a-c8ab-4ecc-bf04-631d0354583a" />
