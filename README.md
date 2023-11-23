#CD

#1))  IMPLEMENTATION OF LEXICAL ANALYZER USING C

```c
#include <stdio.h>
#include <ctype.h>
#include <string.h>
enum TokenType {
    KEYWORD,
    IDENTIFIER,
    INTEGER,
    OPERATOR,
    DELIMITER,
    INVALID
};
enum TokenType classifyToken(char token[]) {
    if (strcmp(token, "if") == 0 || strcmp(token, "else") == 0) {
        return KEYWORD;
    } else if (isdigit(token[0])) {
        return INTEGER;
    } else if (isalpha(token[0]) || token[0] == '_') {
        return IDENTIFIER;
    } else if (token[0] == '+' || token[0] == '-') {
        return OPERATOR;
    } else if (token[0] == ';' || token[0] == ',') {
        return DELIMITER;
    } else {
        return INVALID;
    }
}
void tokenize(char input[]) {
    char token[100];
    int i, j = 0;

    for (i = 0; i <= strlen(input); i++) {
        if (input[i] == ' ' || input[i] == '\0') {
            // Check the type of token and process accordingly
            token[j] = '\0';
            enum TokenType type = classifyToken(token);
            switch (type) {
                case KEYWORD:
                    printf("Keyword: %s\n", token);
                    break;
                case IDENTIFIER:
                    printf("Identifier: %s\n", token);
                    break;
                case INTEGER:
                    printf("Integer: %s\n", token);
                    break;
                case OPERATOR:
                    printf("Operator: %s\n", token);
                    break;
                case DELIMITER:
                    printf("Delimiter: %s\n", token);
                    break;
                case INVALID:
                    printf("Invalid Token: %s\n", token);
                    break;
            }
            memset(token, 0, sizeof(token));
            j = 0;
        } else {
            // Build the token character by character
            token[j++] = input[i];
        }
    }
}

int main() {
    char input[100];

    printf("Enter a C program: ");
    fgets(input, sizeof(input), stdin);

    printf("\nTokens:\n");
    tokenize(input);

    return 0;
}
```




2))   IMPLEMENTATION OF SYMBOL TABLE


#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct Symbol {
    char name[30];
    int address;
};

struct SymbolTable {
    struct Symbol *entries;
    int size;
    int capacity;
};

void initSymbolTable(struct SymbolTable *table, int capacity) {
    table->entries = (struct Symbol *)malloc(capacity * sizeof(struct Symbol));
    table->size = 0;
    table->capacity = capacity;
}

void addSymbol(struct SymbolTable *table, const char *name, int address) {
    if (table->size < table->capacity) {
        struct Symbol *entry = &table->entries[table->size++];
        strcpy(entry->name, name);
        entry->address = address;
    } else {
        printf("Symbol table is full. Unable to add symbol %s.\n", name);
    }
}

int searchSymbol(struct SymbolTable *table, const char *name) {
    for (int i = 0; i < table->size; ++i) {
        if (strcmp(table->entries[i].name, name) == 0) {
            return table->entries[i].address;
        }
    }
    return -1;
}

void displaySymbolTable(struct SymbolTable *table) {
    printf("\nSymbol Table:\nName\tAddress\n");
    for (int i = 0; i < table->size; ++i) {
        printf("%s\t%d\n", table->entries[i].name, table->entries[i].address);
    }
}

void freeSymbolTable(struct SymbolTable *table) {
    free(table->entries);
    table->size = 0;
    table->capacity = 0;
}

int main() {
    struct SymbolTable symbolTable;
    initSymbolTable(&symbolTable, 10);

    addSymbol(&symbolTable, "variable1", 1000);
    addSymbol(&symbolTable, "variable2", 2000);

    int address = searchSymbol(&symbolTable, "variable1");
    if (address != -1) {
        printf("Address of variable1: %d\n", address);
    } else {
        printf("Symbol not found.\n");
    }

    displaySymbolTable(&symbolTable);
    freeSymbolTable(&symbolTable);

    return 0;
}



3))Program to recognize a valid control structures syntax of C language (For loop, while loop, if-else, if-else-if, switch-case, etc.)

LEX
%{
#include "y.tab.h"
%}

%%

"for"                   { return FOR; }
"while"                 { return WHILE; }
"if"                    { return IF; }
"else"                  { return ELSE; }
"switch"                { return SWITCH; }
"case"                  { return CASE; }
"break"                 { return BREAK; }
"int|char|float|double" { return TYPE; }
[ \t\n]                 ; // Skip whitespace
.                       { return *yytext; }

%%

int yywrap() {
    return 1;
}

YACC

%{
#include <stdio.h>
%}

%token FOR WHILE IF ELSE SWITCH CASE BREAK TYPE

%%

program: /* empty */
    | program statement
    ;

statement:
    for_loop
    | while_loop
    | if_statement
    | switch_statement
    | BREAK ';'
    ;

for_loop:
    FOR '(' expression ';' expression ';' expression ')' statement
    ;

while_loop:
    WHILE '(' expression ')' statement
    ;

if_statement:
    IF '(' expression ')' statement
    | IF '(' expression ')' statement ELSE statement
    ;

switch_statement:
    SWITCH '(' expression ')' '{' case_list '}'
    ;

case_list:
    /* empty */
    | case_list CASE constant ':' statement
    ;

expression:
    /* Define your expression rules here */
    ;

constant:
    /* Define your constant rules here */
    ;

%%

int main() {
    yyparse();
    return 0;
}





4)THREE ADDRESS CODE GENERATION USING LEX AND YACC

#LEX
%{
#include "y.tab.h"
%}

%%

[0-9]+                  { yylval.num = atoi(yytext); return NUM; }
[-+*/()]                { return *yytext; }
[a-zA-Z][a-zA-Z0-9_]*    { yylval.id = strdup(yytext); return ID; }
[ \t\n]                 ; 
.                       { yyerror("Invalid character"); }

%%

int yywrap() {
    return 1;
}



#YACC

%{
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct TAC {
    char op;
    char arg1[30];
    char arg2[30];
    char result[30];
};

struct TAC tac[100];
int tacIndex = 0;

%}

%token NUM ID

%%

program: /* empty */
    | program statement
    ;

statement:
    expression ';' { printf("%s = %s %c %s\n", $$.result, $1.result, $2, $3.result); }
    ;

expression:
    ID              { $$ = $1; }
    | NUM           { $$ = $1; }
    | expression '+' expression   { $$ = createTemp(); generateTAC('+', $1, $3, $$); }
    | expression '-' expression   { $$ = createTemp(); generateTAC('-', $1, $3, $$); }
    | expression '*' expression   { $$ = createTemp(); generateTAC('*', $1, $3, $$); }
    | expression '/' expression   { $$ = createTemp(); generateTAC('/', $1, $3, $$); }
    | '(' expression ')'          { $$ = $2; }
    ;

%%

char* createTemp() {
    char temp[30];
    sprintf(temp, "t%d", tacIndex++);
    return strdup(temp);
}

#CD

1))  IMPLEMENTATION OF LEXICAL ANALYZER USING C

#include <stdio.h>
#include <ctype.h>
#include <string.h>
enum TokenType {
    KEYWORD,
    IDENTIFIER,
    INTEGER,
    OPERATOR,
    DELIMITER,
    INVALID
};
enum TokenType classifyToken(char token[]) {
    if (strcmp(token, "if") == 0 || strcmp(token, "else") == 0) {
        return KEYWORD;
    } else if (isdigit(token[0])) {
        return INTEGER;
    } else if (isalpha(token[0]) || token[0] == '_') {
        return IDENTIFIER;
    } else if (token[0] == '+' || token[0] == '-') {
        return OPERATOR;
    } else if (token[0] == ';' || token[0] == ',') {
        return DELIMITER;
    } else {
        return INVALID;
    }
}
void tokenize(char input[]) {
    char token[100];
    int i, j = 0;

    for (i = 0; i <= strlen(input); i++) {
        if (input[i] == ' ' || input[i] == '\0') {
            // Check the type of token and process accordingly
            token[j] = '\0';
            enum TokenType type = classifyToken(token);
            switch (type) {
                case KEYWORD:
                    printf("Keyword: %s\n", token);
                    break;
                case IDENTIFIER:
                    printf("Identifier: %s\n", token);
                    break;
                case INTEGER:
                    printf("Integer: %s\n", token);
                    break;
                case OPERATOR:
                    printf("Operator: %s\n", token);
                    break;
                case DELIMITER:
                    printf("Delimiter: %s\n", token);
                    break;
                case INVALID:
                    printf("Invalid Token: %s\n", token);
                    break;
            }
            memset(token, 0, sizeof(token));
            j = 0;
        } else {
            // Build the token character by character
            token[j++] = input[i];
        }
    }
}

int main() {
    char input[100];

    printf("Enter a C program: ");
    fgets(input, sizeof(input), stdin);

    printf("\nTokens:\n");
    tokenize(input);

    return 0;
}




2))   IMPLEMENTATION OF SYMBOL TABLE


#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct Symbol {
    char name[30];
    int address;
};

struct SymbolTable {
    struct Symbol *entries;
    int size;
    int capacity;
};

void initSymbolTable(struct SymbolTable *table, int capacity) {
    table->entries = (struct Symbol *)malloc(capacity * sizeof(struct Symbol));
    table->size = 0;
    table->capacity = capacity;
}

void addSymbol(struct SymbolTable *table, const char *name, int address) {
    if (table->size < table->capacity) {
        struct Symbol *entry = &table->entries[table->size++];
        strcpy(entry->name, name);
        entry->address = address;
    } else {
        printf("Symbol table is full. Unable to add symbol %s.\n", name);
    }
}

int searchSymbol(struct SymbolTable *table, const char *name) {
    for (int i = 0; i < table->size; ++i) {
        if (strcmp(table->entries[i].name, name) == 0) {
            return table->entries[i].address;
        }
    }
    return -1;
}

void displaySymbolTable(struct SymbolTable *table) {
    printf("\nSymbol Table:\nName\tAddress\n");
    for (int i = 0; i < table->size; ++i) {
        printf("%s\t%d\n", table->entries[i].name, table->entries[i].address);
    }
}

void freeSymbolTable(struct SymbolTable *table) {
    free(table->entries);
    table->size = 0;
    table->capacity = 0;
}

int main() {
    struct SymbolTable symbolTable;
    initSymbolTable(&symbolTable, 10);

    addSymbol(&symbolTable, "variable1", 1000);
    addSymbol(&symbolTable, "variable2", 2000);

    int address = searchSymbol(&symbolTable, "variable1");
    if (address != -1) {
        printf("Address of variable1: %d\n", address);
    } else {
        printf("Symbol not found.\n");
    }

    displaySymbolTable(&symbolTable);
    freeSymbolTable(&symbolTable);

    return 0;
}



3))Program to recognize a valid control structures syntax of C language (For loop, while loop, if-else, if-else-if, switch-case, etc.)

LEX
%{
#include "y.tab.h"
%}

%%

"for"                   { return FOR; }
"while"                 { return WHILE; }
"if"                    { return IF; }
"else"                  { return ELSE; }
"switch"                { return SWITCH; }
"case"                  { return CASE; }
"break"                 { return BREAK; }
"int|char|float|double" { return TYPE; }
[ \t\n]                 ; // Skip whitespace
.                       { return *yytext; }

%%

int yywrap() {
    return 1;
}

YACC

%{
#include <stdio.h>
%}

%token FOR WHILE IF ELSE SWITCH CASE BREAK TYPE

%%

program: /* empty */
    | program statement
    ;

statement:
    for_loop
    | while_loop
    | if_statement
    | switch_statement
    | BREAK ';'
    ;

for_loop:
    FOR '(' expression ';' expression ';' expression ')' statement
    ;

while_loop:
    WHILE '(' expression ')' statement
    ;

if_statement:
    IF '(' expression ')' statement
    | IF '(' expression ')' statement ELSE statement
    ;

switch_statement:
    SWITCH '(' expression ')' '{' case_list '}'
    ;

case_list:
    /* empty */
    | case_list CASE constant ':' statement
    ;

expression:
    /* Define your expression rules here */
    ;

constant:
    /* Define your constant rules here */
    ;

%%

int main() {
    yyparse();
    return 0;
}





4)THREE ADDRESS CODE GENERATION USING LEX AND YACC

#LEX
%{
#include "y.tab.h"
%}

%%

[0-9]+                  { yylval.num = atoi(yytext); return NUM; }
[-+*/()]                { return *yytext; }
[a-zA-Z][a-zA-Z0-9_]*    { yylval.id = strdup(yytext); return ID; }
[ \t\n]                 ; 
.                       { yyerror("Invalid character"); }

%%

int yywrap() {
    return 1;
}



#YACC

%{
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

struct TAC {
    char op;
    char arg1[30];
    char arg2[30];
    char result[30];
};

struct TAC tac[100];
int tacIndex = 0;

%}

%token NUM ID

%%

program: /* empty */
    | program statement
    ;

statement:
    expression ';' { printf("%s = %s %c %s\n", $$.result, $1.result, $2, $3.result); }
    ;

expression:
    ID              { $$ = $1; }
    | NUM           { $$ = $1; }
    | expression '+' expression   { $$ = createTemp(); generateTAC('+', $1, $3, $$); }
    | expression '-' expression   { $$ = createTemp(); generateTAC('-', $1, $3, $$); }
    | expression '*' expression   { $$ = createTemp(); generateTAC('*', $1, $3, $$); }
    | expression '/' expression   { $$ = createTemp(); generateTAC('/', $1, $3, $$); }
    | '(' expression ')'          { $$ = $2; }
    ;

%%

char* createTemp() {
    char temp[30];
    sprintf(temp, "t%d", tacIndex++);
    return strdup(temp);
}

void generateTAC(char op, char* arg1, char* arg2, char* result) {
    tac[tacIndex].op = op;
    strcpy(tac[tacIndex].arg1, arg1);
    strcpy(tac[tacIndex].arg2, arg2);
    strcpy(tac[tacIndex].result, result);
    tacIndex++;
}

int main() {
    yyparse();
    return 0;
}

#5))
IMPLEMENTATION OF TYPE CHECKING

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Enum for data types
typedef enum {
    INT_TYPE,
    FLOAT_TYPE,
    CHAR_TYPE,
    ERROR_TYPE
} DataType;

// Structure for symbol table entry
typedef struct {
    char name[30];
    DataType type;
} SymbolEntry;

// Symbol table (for simplicity, a fixed-size array is used)
SymbolEntry symbolTable[100];
int symbolIndex = 0;

// Function to add a symbol to the symbol table
void addSymbol(const char *name, DataType type) {
    SymbolEntry entry;
    strcpy(entry.name, name);
    entry.type = type;
    symbolTable[symbolIndex++] = entry;
}

// Function to perform type checking on an assignment
DataType typeCheckAssignment(const char *variable, DataType exprType) {
    for (int i = 0; i < symbolIndex; ++i) {
        if (strcmp(symbolTable[i].name, variable) == 0) {
            if (symbolTable[i].type == exprType) {
                printf("Assignment is type-correct.\n");
                return symbolTable[i].type;
            } else {
                printf("Error: Type mismatch in assignment.\n");
                return ERROR_TYPE;
            }
        }
    }

    printf("Error: Variable '%s' not declared.\n", variable);
    return ERROR_TYPE;
}

int main() {
    // Add symbols to the symbol table (for demonstration purposes)
    addSymbol("x", INT_TYPE);
    addSymbol("y", FLOAT_TYPE);

    // Example of type checking in an assignment
    DataType exprType = FLOAT_TYPE; // Assume the expression type is known
    DataType assignmentResult = typeCheckAssignment("x", exprType);

    // You can use the result for further processing or error handling

    return 0;
}

￼Entervoid generateTAC(char op, char* arg1, char* arg2, char* result) {
    tac[tacIndex].op = op;
    strcpy(tac[tacIndex].arg1, arg1);
    strcpy(tac[tacIndex].arg2, arg2);
    strcpy(tac[tacIndex].result, result);
    tacIndex++;
}

int main() {
    yyparse();
    return 0;
}

#5))
IMPLEMENTATION OF TYPE CHECKING

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// Enum for data types
typedef enum {
    INT_TYPE,
    FLOAT_TYPE,
    CHAR_TYPE,
    ERROR_TYPE
Type;

// Structure for symbol table entry
typedef struct {
    char name[30];
    DataType type;
} SymbolEntry;

// Symbol table (for simplicity, a fixed-size array is used)
SymbolEntry symbolTable[100];
int symbolIndex = 0;

// Function to add a symbol to the symbol table
void addSymbol(const char *name, DataType type) {
    SymbolEntry entry;
    strcpy(entry.name, name);
    entry.type = type;
    symbolTable[symbolIndex++] = entry;
}

// Function to perform type checking on an assignment
DataType typeCheckAssignment(const char *variable, DataType exprType) {
    for (int i = 0; i < symbolIndex; ++i) {
        if (strcmp(symbolTable[i].name, variable) == 0) {
            if (symbolTable[i].type == exprType) {
                printf("Assignment is type-correct.\n");
