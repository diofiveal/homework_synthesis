Напишем функцию add. Типы используемых инструкций: R, I
```c
int add(int a, int b){
    return a + b;
}
```
Код на ассемблере:
```asm
#a0 = a, a1 = b
add a0, a0, a1
jalr x0, ra
```

Напишем функцию addi_const. Типы используемых инструкций: I, U
```c
int addi_const(int a){
    int c = 0xABCDE123;
    return a + c;
}
```
Код на ассемблере:
```asm
#s3 = c
lui s3, 0xABCDE
addi s3, s3, 0x123
jalr ra
```
1) Я с помощью lui загрузил в s2 старшие 20 бит числа 0xABCDE123
2) С помощью addi я загрузил в s2 младшие 12 бит числа 0xABCDE123

Напишем функцию store_10. Типы используемых инструкций: I, S
```c
void store_number(int *arr, int size){
    arr[1] = 10;
}
```
Код на ассемблере
```asm
addi a1, 0, 10
sw   a1, 4 # 4 - адрес для записи
```
Напишем функцию add_with_condition, которая складывает одно из двух чисел с числом 4, если они равны, иначе складывает подаваемые числа. Функция реализована с помощью функций add и addi_const.
Типы используемых инструкций: R, I, B(beq), J(jal)
```c
int add_with_condition(int reg1, int reg2){
    if(reg1 == reg2){
        int reg_res = addi_const(reg1);
        return reg_res;
    }

    int reg_res = add(reg1, reg2);
    return reg_res;
```
Код на ассемблере:
```asm
s0 = reg_res, s1 = reg1, s2 = reg2
beq s1, s2, L1
jal addi_const
add s0, s3, 0 # в s3 хранится return функции addi_const
jalr ra
L1: jal add
    add s0, a0, 0 #в a0 хранится reurn функции add
    jalr ra
```

