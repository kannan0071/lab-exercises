# Ex.No.1B Implementation of Playfair Cipher 

## AIM:

To write a C program to implement the Playfair Substitution technique.

## DESCRIPTION:

 The Playfair cipher starts with creating a key table. The key table is a 5×5 grid of letters that will act as the key for encrypting your plaintext. Each of the 25 letters must be unique and one letter of the alphabet is omitted from the table (as there are 25 spots and 26 letters in the alphabet).
 
To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. The two letters of the diagram are considered as the opposite corners of a rectangle in the key table. Note the relative position of the corners of this rectangle. Then apply the following 4 rules, in order, to each pair of letters in the plaintext:

1.If both letters are the same (or only one letter is left), add an "X" after the first letter.

2.If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively.

3.If the letters appear on the same column of your table, replace them with the letters immediately below respectively.

4.If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair.

## EXAMPLE:

![image](https://github.com/kannan0071/lab-exercises/assets/119641638/3e00991e-c11f-40ef-ba73-ba2ca9b06974)

## ALGORITHM:

STEP-1: Read the plain text from the user.

STEP-2: Read the keyword from the user.

STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.

STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.

STEP-5: Display the obtained cipher text.

## PROGRAM:
```c
#include<stdio.h>
#include<string.h>
#include<ctype.h>
#define MX 5

void playfair(char ch1, char ch2, char key[MX][MX], FILE *out) {
    int i, j, w, x, y, z;

    for (i = 0; i < MX; i++) {
        for (j = 0; j < MX; j++) {
            if (ch1 == key[i][j]) {
                w = i;
                x = j;
            } else if (ch2 == key[i][j]) {
                y = i;
                z = j;
            }
        }
    }

    if (x == z) {
        x = (x + 1) % 5;
        z = (z + 1) % 5;
        printf("%c%c", key[w][x], key[y][z]);
        fprintf(out, "%c%c", key[w][x], key[y][z]);
    } else {
        w = (w + 1) % 5;
        y = (y + 1) % 5;
        printf("%c%c", key[w][x], key[y][z]);
        fprintf(out, "%c%c", key[w][x], key[y][z]);
        printf("%c%c", key[w][z], key[y][x]);
        fprintf(out, "%c%c", key[w][z], key[y][x]);
    }
}

int main() {
    int i, j, k = 0, l, m = 0, n;
    char key[MX][MX], keyminus[25], keystr[10], str[25] = {0};
    char alpa[26] = {'A','B','C','D','E','F','G','H','I','J','K','L','M','N','O','P','Q','R','S','T','U','V','W','X','Y','Z'};
    FILE *out;

    printf("\nEnter key:");
    fgets(keystr, sizeof(keystr), stdin);
    printf("\nEnter the plain text:");
    fgets(str, sizeof(str), stdin);

    n = strlen(keystr);
    for (i = 0; i < n; i++) {
        if (keystr[i] == 'j' || keystr[i] == 'J')
            keystr[i] = 'I';
        keystr[i] = toupper(keystr[i]);
    }

    for (i = 0; i < strlen(str); i++) {
        if (str[i] == 'j' || str[i] == 'J')
            str[i] = 'I';
        str[i] = toupper(str[i]);
    }

    j = 0;
    for (i = 0; i < 26; i++) {
        for (k = 0; k < n; k++) {
            if (keystr[k] == alpa[i])
                break;
            else if (alpa[i] == 'J')
                break;
        }
        if (k == n) {
            keyminus[j] = alpa[i];
            j++;
        }
    }

    k = 0;
    for (i = 0; i < MX; i++) {
        for (j = 0; j < MX; j++) {
            if (k < n)
                key[i][j] = keystr[k++];
            else
                key[i][j] = keyminus[m++];
            printf("%c ", key[i][j]);
        }
        printf("\n");
    }

    printf("\nEntered text: %s\nCipher Text: ", str);
    out = fopen("cipher.txt", "a+");
    if (out == NULL) {
        printf("File Corrupted.");
        return 1;
    }

    for (i = 0; i < strlen(str); i++) {
        if (str[i] == 'J')
            str[i] = 'I';
        if (str[i + 1] == '\0')
            playfair(str[i], 'X', key, out);
        else {
            if (str[i + 1] == 'J')
                str[i + 1] = 'I';
            if (str[i] == str[i + 1])
                playfair(str[i], 'X', key, out);
            else {
                playfair(str[i], str[i + 1], key, out);
                i++;
            }
        }
    }

    fclose(out);
    return 0;
}

```
## OUTPUT:

![image](https://github.com/kannan0071/lab-exercises/assets/119641638/92b414de-c241-4da3-8d70-5ad6d499c258)

## RESULT:

Thus the Playfair cipher substitution technique had been implemented successfully.


