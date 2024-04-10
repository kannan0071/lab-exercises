##AIM:

To write a C program to implement the MD5 hashing technique.

##DESCRIPTION:

MD5 processes a varia le-length message into a fixed-length output of 128 bits. The input message is broken up into chunks of 512-bit blocks. The message is paded so that its length is divisible by 512. The padding works as follows: first a single bit, 1, is appended to the end of the message. This is followed by as many zeros as are required to bring the length of the message up to 64 bits less than a multiple of 512. The remaining bits are filled up with

64 bits representing the length of the original message, modulo 2^64.The main D5 algorithm operates on a 128-bit state, divided into four 32-bit words, denoted A, B, C, and D. These are initialized to certain fixed constants. The main algorithm then uses each 512-bit message block in turn to modify the state.

##EXAMPLE:

![image](https://github.com/kannan0071/lab-exercises/assets/119641638/a8d053e2-2f6d-456d-aafc-bfea67cd98b0)

##ALGORITHM:

STEP-1: Read the 128-bit plain text.

STEP-2: Divide into four blocks of 32-bits named as A, B, C and D.

STEP-3: Compute the functions f, g, h and i with operations such as, rotations, permutations, etc,.

STEP-4: The output of these functions are combined together as F and performed circular shifting and then given to key round.

STEP-5: Finally, right shift of ‘s’ times are performed and the results are combined together to produce the final output.

##PROGRAM:
```c
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include <math.h>
#include<conio.h>
typedef union uwb
{
    unsigned w;
    unsigned char b[4];
} MD5union;
typedef unsigned DigestArray[4];
unsigned func0( unsigned abcd[] )
{
    return ( abcd[1] & abcd[2]) | (~abcd[1] & abcd[3]);
}
unsigned func1( unsigned abcd[] )
{
    return ( abcd[3] & abcd[1]) | (~abcd[3] & abcd[2]);
}
unsigned func2( unsigned abcd[] )
{
    return abcd[1] ^ abcd[2] ^ abcd[3];
}
unsigned func3( unsigned abcd[] )
{
    return abcd[2] ^ (abcd[1] |~ abcd[3]);
}
typedef unsigned (*DgstFctn)(unsigned a[]);
unsigned *calctable( unsigned *k)
{
    double s, pwr; int i;
    pwr = pow( 2, 32);
    for (i=0; i<64; i++)
    {
        s = fabs(sin(1+i));
        k[i] = (unsigned)( s * pwr );
    }
    return k;
}
unsigned rol( unsigned r, short N )
{
    unsigned mask1 = (1<<N) -1;
    return ((r>>(32-N)) & mask1) | ((r<<N) & ~mask1);
}
unsigned *md5( const char *msg, int mlen)
{
    static DigestArray h0 = { 0x67452301, 0xEFCDAB89, 0x98BADCFE, 0x10325476 };
    static DgstFctn ff[] = { &func0, &func1, &func2, &func3}; static short M[] = { 1, 5, 3, 7 };
    static short O[] = { 0, 1, 5, 0 };
    static short rot0[] = { 7,12,17,22};
    static short rot1[] = { 5, 9,14,20};
    static short rot2[] = { 4,11,16,23};
    static short rot3[] = { 6,10,15,21};
    static short *rots[] = {rot0, rot1, rot2, rot3 };
    static unsigned kspace[64];
    static unsigned *k;
    static DigestArray h;
    DigestArray abcd;
    DgstFctn fctn;
    short m, o, g;
    unsigned f;
    short *rotn;
    union
    {
        unsigned w[16];
        char b[64];
    }mm;
    int os = 0;
    int grp, grps, q, p;
    unsigned char *msg2;
    if (k==NULL) k= calctable(kspace);
    for (q=0; q<4; q++) h[q] = h0[q];	// initialize
    {
        grps = 1 + (mlen+8)/64;
        msg2 = malloc( 64*grps);
        memcpy( msg2, msg, mlen);
        msg2[mlen] = (unsigned char)0x80;
        q = mlen + 1;
        while (q < 64*grps)
        {
            msg2[q] = 0;
            q++ ;
        }
        {
            MD5union u;
            u.w = 8*mlen; q -= 8;
            memcpy(msg2+q, &u.w, 4 );
        }
    }
    for (grp=0; grp<grps; grp++)
    {
        memcpy( mm.b, msg2+os, 64);
        for(q=0;q<4;q++)
            abcd[q] = h[q];
        for (p = 0; p<4; p++)
        {
            fctn = ff[p];
            rotn = rots[p];
            m = M[p];
            o= O[p];
            for (q=0; q<16; q++)
            {
                g = (m*q + o) % 16;
                f = abcd[1] + rol( abcd[0]+ fctn(abcd)+k[q+16*p]+ mm.w[g], rotn[q%4]); abcd[0] = abcd[3];
                abcd[3] = abcd[2];
                abcd[2] = abcd[1];
                abcd[1] = f;
            }
        }
        for (p=0; p<4; p++)
            h[p] += abcd[p];
            os += 64;
    }
    return h;
}
void main()
{
    int j,k;
    const char *msg = "The quick brown fox jumps over the lazy dog";
    unsigned *d = md5(msg, strlen(msg));
    MD5union u;
    //clrscr();
    printf("\t MD5 ENCRYPTION ALGORITHM IN C \n\n");
    printf("Input String to be Encrypted using MD5 : %s",msg);
    printf("\n\nThe MD5 code for input string is: 0x");
    for (j=0;j<4; j++)
    {
        u.w = d[j];
        for (k=0;k<4;k++)
            printf("%02x",u.b[k]);
    }
    printf("\n");
    printf("\nMD5 Encyption Successfully Completed!!!\n\n");
    getch();
    system("pause");
    getch();
}
```
##OUTPUT:

![image](https://github.com/kannan0071/lab-exercises/assets/119641638/6e51aee9-f31c-487c-b362-08f043bb30e6)


##RESULT:

Thus the implementation of MD5 hashing algorithm had been implemented successfully using C.

