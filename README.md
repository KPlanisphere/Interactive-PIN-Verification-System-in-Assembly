# Interactive PIN Verification System in Assembly

This repository contains an assembly language project that creates an interactive PIN verification system. The program sets up a screen with a graphical border, prompts the user to enter a 4-digit PIN, and then verifies it against a predefined PIN.

## Features

- **Graphical Interface**: Sets up a screen with a colored border and background.
- **PIN Entry**: Prompts the user to enter a 4-digit PIN, with each digit masked as an asterisk.
- **PIN Verification**: Compares the entered PIN against a predefined PIN and displays a success or failure message.

### Code Snippets

#### Screen Setup
The program sets the video mode to 80x25 and clears the screen with a specified background and text color.

```assembly
MOV AH, 00H       ; Service to set video mode
MOV AL, 02H       ; 80x25 text mode
INT 10H

MOV AH, 06H       ; Scroll up function to clear screen
MOV AL, 00H       ; Clear entire screen
MOV BH, 003H      ; Attribute: background black, text blue
MOV CX, 00H       ; Starting position
MOV DH, 24        ; Ending row
MOV DL, 79        ; Ending column
INT 10H
```

#### Border Drawing

The program draws a border of asterisks around the screen.

```assembly
; Vertical left border
MOV CX, 23
BARRAIZQ:
    MOV AH, 02H
    MOV BH, 0
    MOV DH, IZQUIERDA
    MOV DL, 0
    INT 10H
    
    MOV AH, 02H
    MOV DL, 02Ah
    INT 21H
    
    ADD IZQUIERDA, 1
LOOP BARRAIZQ

; Horizontal top border
MOV AH, 02H
MOV BH, 0
MOV DH, 0
MOV DL, 0
INT 10H

MOV AH, 09
MOV DX, OFFSET MH
INT 21H
```

#### PIN Entry

The program prompts the user to enter a 4-digit PIN, masking each digit as it is entered.

```assembly
; Prompt for PIN
MOV BH, 0
MOV AH, 19
MOV CX, 12
MOV DH, 7
MOV DL, 32
LEA BP, NIP
MOV AL, 0
MOV BL, 5H
INT 10H

; Read first digit
MOV AH, 02H
MOV BH, 0
MOV DH, 10
MOV DL, 35
INT 10H
MOV AH, 07H
INT 21H
MOV NIP1, AL
MOV AH, 9    
MOV AL, 02AH
MOV CX, 1
MOV BH, 0
MOV BL, 2H
INT 10H

; Read second digit
MOV AH, 02H
MOV BH, 0
MOV DH, 10
MOV DL, 36
INT 10H
MOV AH, 07H 
INT 21H 
MOV NIP2, AL 
MOV AH, 9  
MOV AL, 02AH 
MOV CX, 1 
MOV BH, 0 
MOV BL, 2H 
INT 10H

; Read third digit 
MOV AH, 02H 
MOV BH, 0 
MOV DH, 10 
MOV DL, 37 
INT 10H 
MOV AH, 07H 
INT 21H 
MOV NIP3, AL 
MOV AH, 9  
MOV AL, 02AH 
MOV CX, 1 
MOV BH, 0 
MOV BL, 2H 
INT 10H

; Read fourth digit 
MOV AH, 02H 
MOV BH, 0 
MOV DH, 10 
MOV DL, 38 
INT 10H 
MOV AH, 07H 
INT 21H 
MOV NIP4, AL 
MOV AH, 9  
MOV AL, 02AH 
MOV CX, 1 
MOV BH, 0 
MOV BL, 2H 
INT 10H
```


#### PIN Verification
The program checks the entered PIN against the predefined PIN and displays a success or failure message.

```assembly
; Verify PIN
MOV AL, CLAVE[0]
CMP AL, NIP1
JNE DIFERENTES

MOV AL, CLAVE[1]
CMP AL, NIP2
JNE DIFERENTES

MOV AL, CLAVE[2]
CMP AL, NIP3
JNE DIFERENTES

MOV AL, CLAVE[3]
CMP AL, NIP4
JNE DIFERENTES

; Success message
MOV BH, 0
MOV AH, 19
MOV CX, 18
MOV DH, 13
MOV DL, 29
LEA BP, BIEN
MOV AL, 0
MOV BL, 2H
INT 10H
JMP FIN

DIFERENTES:
; Failure message
MOV BH, 0
MOV AH, 19
MOV CX, 20
MOV DH, 13
MOV DL, 28
LEA BP, MAL
MOV AL, 0
MOV BL, 4H
INT 10H
JMP FIN

FIN:
INT 20H
```

### Memory Variables

-   `NIP1`, `NIP2`, `NIP3`, `NIP4`: Store the individual digits of the entered PIN.
-   `CLAVE`: Predefined PIN for verification.
-   `BIEN`: Message displayed for correct PIN.
-   `MAL`: Message displayed for incorrect PIN.
-   `MH`: String for horizontal border.
-   `IZQUIERDA`, `DERECHA`, `AUX`: Variables used for border drawing.

