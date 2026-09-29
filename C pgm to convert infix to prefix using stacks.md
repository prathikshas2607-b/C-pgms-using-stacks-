# C pgm to convert infix to prefix using stacks   
  
#include <stdio.h>  
#include <stdlib.h>  
#include <string.h>  
#include <ctype.h>  
#define MAX 100  
struct Stack {  
    int top;  
    char arr[MAX];  
};  
void push(struct Stack* stack, char ch) {  
    if (stack->top == MAX - 1) return;  
    stack->arr[++(stack->top)] = ch;  
}  
char pop(struct Stack* stack) {  
    if (stack->top == -1) return '\0';  
    return stack->arr[(stack->top)--];  
}  
char peek(struct Stack* stack) {  
    if (stack->top == -1) return '\0';  
    return stack->arr[stack->top];  
}  
int isEmpty(struct Stack* stack) {  
    return stack->top == -1;  
}  
int precedence(char ch) {  
    if (ch == '^') return 3;  
    if (ch == '*' || ch == '/') return 2;  
    if (ch == '+' || ch == '-') return 1;  
    return -1;  
}  
void reverseAndSwap(char* exp) {  
    int len = strlen(exp);  
    for (int i = 0; i < len / 2; i++) {  
        char temp = exp[i];  
        exp[i] = exp[len - 1 - i];  
        exp[len - 1 - i] = temp;  
    }  
    for (int i = 0; i < len; i++) {  
        if (exp[i] == '(') {  
            exp[i] = ')';  
        } else if (exp[i] == ')') {  
            exp[i] = '(';  
        }  
    }  
}  
void reverse(char* exp) {  
    int len = strlen(exp);  
    for (int i = 0; i < len / 2; i++) {  
        char temp = exp[i];  
        exp[i] = exp[len - 1 - i];  
        exp[len - 1 - i] = temp;  
    }  
}  
void infixToPostfix(char* infix, char* postfix) {  
    struct Stack stack;  
    stack.top = -1;  
    int j = 0;  
    for (int i = 0; infix[i] != '\0'; i++) {  
        char ch = infix[i];  
        if (isalnum(ch)) {  
            postfix[j++] = ch;  
        }  
        else if (ch == '(') {  
            push(&stack, ch);  
        }  
        else if (ch == ')') {  
            while (!isEmpty(&stack) && peek(&stack) != '(') {  
                postfix[j++] = pop(&stack);  
            }  
            pop(&stack);  
        }  
        else {  
            while (!isEmpty(&stack) && precedence(peek(&stack)) >= precedence(ch)) {  
                if (ch == '^' && peek(&stack) == '^') {  
                    break;  
                }  
                postfix[j++] = pop(&stack);  
            }  
            push(&stack, ch);  
        }  
    }  
    while (!isEmpty(&stack)) {  
        postfix[j++] = pop(&stack);  
    }  
    postfix[j] = '\0';  
}  
void infixToPrefix(char* infix, char* prefix) {  
    reverseAndSwap(infix);  
    char tempPostfix[MAX];  
    infixToPostfix(infix, tempPostfix);  
    strcpy(prefix, tempPostfix);  
    reverse(prefix);  
}  
int main() {  
    char infix[MAX] = "((A+B)*C-D)*E";  
    char prefix[MAX];  
    printf("Infix expression: %s\n", infix);  
    infixToPrefix(infix, prefix)  
    printf("Prefix expression: %s\n", prefix);  
    return 0;  
}  
