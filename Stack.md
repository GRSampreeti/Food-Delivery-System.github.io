# Stack

A stack is a data type that serves as a collection of elements with two main operations:
- **Push**, which adds an element to the collection.
- **Pop**, which removes the most recently added element.

<img src="https://miro.medium.com/v2/resize:fit:800/1*kkK3EZNOzBsuwkDNvSVR9g.gif" alt="Bloom Filter GIF" width="400">

## C Code

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define SIZE 1000

typedef struct FoodItem {
    char name[50];
    float price;
} FoodItem;

typedef struct STACK {
    FoodItem items[SIZE];
    int top;
} s;

void push(s *stack);
FoodItem pop(s *stack);
void total(s *stack, int num);
void display(s *stack);

int main() {
    s *stack;
    s s1;
    stack = &s1;
    stack->top = -1;
    int i, num;
    printf("Enter number of food items: ");
    scanf("%d", &num);
    for (i = 0; i < num; i++) {
        push(stack);
    }
    total(stack, num);
    display(stack);
    return 0;
}

void push(s *stack) {
    if (stack->top == SIZE - 1) {
        printf("Stack is full\n");
    } else {
        stack->top++;
        printf("Enter the name of the food item: ");
        scanf("%s", stack->items[stack->top].name);
        printf("Enter the price of the food item: ");
        scanf("%f", &stack->items[stack->top].price);
    }
}

FoodItem pop(s *stack) {
    FoodItem item;
    if (stack->top == -1) {
        printf("Stack is empty\n");
    } else {
        item = stack->items[stack->top];
        stack->top--;
    }
    return item;
}

void total(s *stack, int num) {
    int i;
    float max = 0, sum = 0;
    for (i = 0; i <= stack->top; i++) {
        if (stack->items[i].price > max) {
            max = stack->items[i].price;
        }
        sum += stack->items[i].price;
    }
    printf("Maximum price is: %.2f\n", max);
    printf("Total amount is: %.2f\n", sum);
}

void display(s *stack) {
    int i;
    if (stack->top == -1) {
        printf("Stack is empty\n");
    } else {
        printf("Food items in the cart:\n");
        for (i = stack->top; i >= 0; i--) {
            printf("Name: %s, Price: %.2f\n", stack->items[i].name, stack->items[i].price);
        }
    }
}
