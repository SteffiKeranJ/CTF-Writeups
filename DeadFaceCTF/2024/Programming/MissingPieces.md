# Missing Pieces
Programming, 10points

## Description
> We found this code from deephax that looks like it was built to hide a flag using some form of cryptography. The program is missing code that will reveal the flag. Try to fix the program and submit the flag.
> 
> Submit the flag as flag{flag_text}.

Attached was a c file with the following program:

```
# include <stdio.h>
# include <string.h>

const char *hex_string = "b52195a4a82bc5ade23e9c9c8725c79cb07d90f0ae";  // The hex string for the flag
const char *key = "d34df4c3";

int hex_char_to_int(char c) {
    if (c >= '0' && c <= '9') return c - '0';
    if (c >= 'a' && c <= 'f') return c - 'a' + 10;
    if (c >= 'A' && c <= 'F') return c - 'A' + 10;
    return -1;
}

void hex_string_to_bytes(const char *hex, unsigned char *bytes, size_t length) {
    for (size_t i = 0; i < length; ++i) {
        bytes[i] = (hex_char_to_int(hex[i * 2]) << 4) | hex_char_to_int(hex[i * 2 + 1]);
    }
}

void xor_bytes(unsigned char *data, size_t data_length, const unsigned char *key, size_t key_length) {
    for (size_t i = 0; i < data_length; ++i) {
        data[i] ^= key[i % key_length];
    }
}

int main() {
    size_t flag_length = strlen(hex_string) / 2;
    unsigned char flag[flag_length];
    unsigned char key_bytes[4];

    // Convert hex string to bytes
    hex_string_to_bytes(hex_string, flag, flag_length);
    hex_string_to_bytes(key, key_bytes, 4);

    xor_bytes(flag, flag_length, key_bytes, 4);

    // Print the result as a string
    printf("Resulting string: ");
    for (size_t i = 0; i < flag_length; ++i) {
        printf("%c");
    }
    printf("\n");

    return 0;
}
```

## Solution 
The first thing to do is to run the file on C compiler and see what is the issue.
On running the program, it gave the following warning
```
.//main.c:40:18: warning: more '%' conversions than data arguments [-Wformat-insufficient-args]
        printf("%c");
                ~^
1 warning generated.
Resulting string: ���������������������

```

The warning was helpful as to where the missing code was.

After analysing the code, I updated the print statement to `printf("%c", flag[i]);` and reran the program

```
# include <stdio.h>
# include <string.h>

const char *hex_string = "b52195a4a82bc5ade23e9c9c8725c79cb07d90f0ae";  // The hex string for the flag
const char *key = "d34df4c3";

int hex_char_to_int(char c) {
    if (c >= '0' && c <= '9') return c - '0';
    if (c >= 'a' && c <= 'f') return c - 'a' + 10;
    if (c >= 'A' && c <= 'F') return c - 'A' + 10;
    return -1;
}

void hex_string_to_bytes(const char *hex, unsigned char *bytes, size_t length) {
    for (size_t i = 0; i < length; ++i) {
        bytes[i] = (hex_char_to_int(hex[i * 2]) << 4) | hex_char_to_int(hex[i * 2 + 1]);
    }
}

void xor_bytes(unsigned char *data, size_t data_length, const unsigned char *key, size_t key_length) {
    for (size_t i = 0; i < data_length; ++i) {
        data[i] ^= key[i % key_length];
    }
}

int main() {
    size_t flag_length = strlen(hex_string) / 2;
    unsigned char flag[flag_length];
    unsigned char key_bytes[4];

    // Convert hex string to bytes
    hex_string_to_bytes(hex_string, flag, flag_length);
    hex_string_to_bytes(key, key_bytes, 4);

    xor_bytes(flag, flag_length, key_bytes, 4);

    // Print the result as a string
    printf("Resulting string: ");
    for (size_t i = 0; i < flag_length; ++i) {
        printf("%c");
    }
    printf("\n");

    return 0;
}
```

This printed the flag

```
Resulting string: flag{f1n1sh_Th3_c0d3}


========== Execution Successful ==========


```
