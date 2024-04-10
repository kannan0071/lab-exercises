##AIM:

To implement the Vigenere Cipher substitution technique using C program.

##DESCRIPTION:

  To encrypt, a table of alphabets can be used, termed a tabula recta, Vigenère square or Vigenère table. It consists of the alphabet written out 26 times in differnt rows, each alphabet shifted cyclically to the left compared to the previous alphabet, corresponding to the 26 possible Caesar ciphers. At different points in the encryption process, the cipher uses a different alphabet from one of the rows. The alphabet used at each point repeating keyword.

Each row starts with a key letter. The remainder of the row holds the letters A to Z. Although there are 26 key rows shown, you will only use as many keys as there are unique letters in the key string, here just 5 keys, {L, E, M, O, N}. For successive letters of the message, we are going to take successive letters of the key string, and encipher each message letter using its corresponding key row. Choose the next letter of the key, go al ng that row to find the column heading that	atches the message character; the letter at the intersection of [key-row, msg-col] is the enciphered letter.

##EXAMPLE:

![image](https://github.com/kannan0071/lab-exercises/assets/119641638/861d7bb2-2e1b-44cf-bd50-4c65c100abdd)

##ALGORITHM:

STEP-1: Arrange the alphabets in row and column of a 26*26 matrix.

STEP-2: Circulate the alphabets in each row to position left such that the first letter is attached to last.

STEP-3: Repeat this process for all 26 rows and construct the final key matrix.

STEP-4: The keyword and the plain text is read from the user.

STEP-5: The characters in the keyword are repeated sequentially so as to match with that of the plain text.

STEP-6: Pick the first letter of the plain text and that of the keyword as the row indices and column indices respectively.

STEP-7: The junction character where these two meet forms the cipher character.

STEP-8: Repeat the above steps to generate the entire cipher text.

##PROGRAM:
```c
#include <stdio.h>
#include<conio.h>
#include <ctype.h>
#include <string.h>
void encipher();
void decipher();
void main()
{
    int choice;
    //clrscr();
    while(1)
    {
        printf("\n\n1. Encrypt Text");
        printf("\t2. Decrypt Text");
        printf("\t3. Exit\n");
        printf("\nEnter Your Choice : ");
        scanf("%d",&choice);
        if(choice == 3)
        exit(0);
        else if(choice == 1)
        encipher();
        else if(choice == 2)
        decipher();
        else
        printf("Please Enter Valid Option.");
    }
}
void encipher()
{
    unsigned int i,j;
    char input[50],key[10];
    printf("\nEnter Plain Text: ");
    scanf("%s",input);
    printf("\nEnter Key Value: ");
    scanf("%s",key);
    printf("\nResultant Cipher Text: ");
    for(i=0,j=0;i<strlen(input);i++,j++)
    {
        if(j>=strlen(key))
        {
            j=0;
        }
        printf("%c",65+(((toupper(input[i])-65)+(toupper(key[j])-65))%26));
    }
}
void decipher()
{
    unsigned int i,j;
    char input[50],key[10];
    int value;
    printf("\nEnter Cipher Text: ");
    scanf("%s",input);
    printf("\nEnter the key value: ");
    scanf("%s",key);
    printf("\nResultant Plain Text: ");
    for(i=0,j=0;i<strlen(input);i++,j++)
    {
        if(j>=strlen(key))
        {
            j=0;
        }
        value = (toupper(input[i])-64)-(toupper(key[j])-64);
        if( value < 0)
        {
            value = value * -1;
        }
        printf("%c",65 + (value % 26));
    }
}
```
##OUTPUT:

![image](https://github.com/kannan0071/lab-exercises/assets/119641638/88145ad4-a125-4bdd-8d2f-aa3419d2d26f)


##RESULT:

Thus the Vigenere Cipher substitution technique had been implemented successfully.

