# VIGENERE-CIPHER
## EX. NO: 4
 

## IMPLEMETATION OF VIGENERE CIPHER
 

## AIM:

To implement the Vigenere Cipher substitution technique using C program.

## DESCRIPTION:

To encrypt, a table of alphabets can be used, termed a tabula recta, Vigenère square,or Vigenère table. It consists of the alphabet written out 26 times in differnt rows, each
 
alphabet shifted cyclically to the left compared to the previous alphabet, corresponding to the 26 possible Caesar ciphers. At different points in the encryption process, the cipher uses adifferent alphabet from one of the rows. The alphabet used at each point repeating keyword.depends on a Each row starts with a key letter. The remainder of the row holds the letters A to Z. Although there are 26 key rows shown, you will only use as many keys as there are unique letters in the key string, here just 5 keys, {L, E, M, O, N}. For successive letters of the message, we are going to take successive letters of the key string, and encipher each message letter using its corresponding key row. Choose the next letter of the key, go along that row to find the column heading that	atches the message character; the letter at the intersection of
[key-row, msg-col] is the enciphered letter.


## ALGORITHM:

STEP-1: Arrange the alphabets in row and column of a 26*26 matrix.
STEP-2: Circulate the alphabets in each row to position left such that the first letter is attached to last.
STEP-3: Repeat this process for all 26 rows and construct the final key matrix.
STEP-4: The keyword and the plain text is read from the user.
STEP-5: The characters in the keyword are repeated sequentially so as to match with that of the plain text.
STEP-6: Pick the first letter of the plain text and that of the keyword as the row indices and column indices respectively.
STEP-7: The junction character where these two meet forms the cipher character.
STEP-8: Repeat the above steps to generate the entire cipher text.


## PROGRAM
~~~
#include <stdio.h>
#include <string.h>
#include <ctype.h>

char m[5][5];

void makeMatrix(char *key) {
    char all[200] = "";
    strcat(all, key);
    strcat(all, "ABCDEFGHIKLMNOPQRSTUVWXYZ");   // alphabet without J
    int used[26] = {0}, k = 0;
    for (int i = 0; all[i]; i++) {
        char c = toupper(all[i]);
        if (c == 'J') c = 'I';
        if (isalpha(c) && !used[c - 'A']) {
            used[c - 'A'] = 1;
            m[k / 5][k % 5] = c;
            k++;
        }
    }
}

void prepare(char *in, char *out) {
    char t[200];
    int n = 0, k = 0;
    for (int i = 0; in[i]; i++)
        if (isalpha(in[i])) t[n++] = (toupper(in[i]) == 'J') ? 'I' : toupper(in[i]);
    for (int i = 0; i < n;) {
        out[k++] = t[i++];
        if (i < n && t[i] != out[k - 1]) out[k++] = t[i++];
        else out[k++] = 'X';                    // pad odd end or double letter
    }
    out[k] = 0;
}

void find(char c, int *r, int *col) {
    for (int i = 0; i < 25; i++)
        if (m[i / 5][i % 5] == c) { *r = i / 5; *col = i % 5; }
}

// s = 1 encrypt, s = 4 decrypt
void cipher(char *t, int s) {
    int r1, c1, r2, c2;
    for (int i = 0; t[i]; i += 2) {
        find(t[i], &r1, &c1);
        find(t[i + 1], &r2, &c2);
        if (r1 == r2)      { c1 = (c1 + s) % 5; c2 = (c2 + s) % 5; }  // same row
        else if (c1 == c2) { r1 = (r1 + s) % 5; r2 = (r2 + s) % 5; }  // same column
        else               { int x = c1; c1 = c2; c2 = x; }           // rectangle
        t[i] = m[r1][c1];
        t[i + 1] = m[r2][c2];
    }
}

int main() {
    char key[100], pt[200], text[200];
    printf("Enter the key: ");
    fgets(key, 100, stdin);
    printf("Enter the plaintext: ");
    fgets(pt, 200, stdin);

    makeMatrix(key);
    prepare(pt, text);

    printf("\nMatrix:\n");
    for (int i = 0; i < 5; i++) {
        for (int j = 0; j < 5; j++) printf("%c ", m[i][j]);
        printf("\n");
    }

    cipher(text, 1);
    printf("\nEncrypted: %s\n", text);
    cipher(text, 4);
    printf("Decrypted: %s\n", text);
    return 0;
}
~~~

## OUTPUT
<img width="1912" height="912" alt="Screenshot 2026-10-29 121241" src="https://github.com/user-attachments/assets/1092886b-76d0-4f69-a470-72b1cb04c7b1" />


## RESULT
