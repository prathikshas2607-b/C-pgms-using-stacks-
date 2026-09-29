# C pgm to convert decimal to binary using stacks  
#include <stdio.h>  
#include <stdlib.h>  
#define MAX 64    
struct Stack {  
    int top;  
    int items[MAX];  
};  
void initStack(struct Stack* s) {  
    s->top = -1;  
}  
int isFull(struct Stack* s) {  
    return s->top == MAX - 1;  
}  
int isEmpty(struct Stack* s) {  
    return s->top == -1;  
}  
void push(struct Stack* s, int value) {  
    if (isFull(s)) {  
        printf("Stack Overflow!\n");  
        return;  
    }  
    s->items[++(s->top)] = value;  
}  
int pop(struct Stack* s) {  
    if (isEmpty(s)) {  
        printf("Stack Underflow!\n");  
        return -1;  
    }  
    return s->items[(s->top)--];  
}  
void decimalToBinary(int num) {  
    struct Stack s;  
    initStack(&s);  
    if (num == 0) {  
        printf("Binary equivalent: 0\n");  
        return;  
    }  
    int originalNum = num;  
    while (num > 0) {  
        push(&s, num % 2);  
        num = num / 2;  
    }  
    printf("Binary equivalent of %d: ", originalNum);  
    while (!isEmpty(&s)) {  
        printf("%d", pop(&s));  
    }  
    printf("\n");  
}  
int main() {  
    int decimalNumber;  
    printf("Enter a decimal number: ");  
    if (scanf("%d", &decimalNumber) != 1) {  
        printf("Invalid input.\n");  
        return 1;  
    }  
    if (decimalNumber < 0) {  
        printf("Please enter a non-negative integer.\n");  
        return 1;  
    }  
    decimalToBinary(decimalNumber);  
    return 0;  
}  
