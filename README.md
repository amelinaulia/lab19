# Домашнее задание к работе 19
## Условие задачи
Задание 3. Разработайте программу, которая записывает в файл поля структуры задания 3 работы 18 Запись «Сотрудник»:
Фамилия Имя Отчество Пол Должность Дата рождения Вывести данные об инженерах, пенсионного возраста (мужчинам больше 65-ти лет, женщинам 60).

### Алгоритм
1. Начало программы.
2. Объявление константы SIZE = 3.
3. Создание структуры employee с полями:
   - char lastname[20];
   - char name[20];
   - char surname[20];
   - char gender[10];
   - char position[20];
   - int year;
4. Объявление массива employee p1[SIZE].
5. Установка локали setlocale(LC_ALL, "RUS").
6. Ввод данных сотрудников (цикл от i = 0 до SIZE-1):
   - ввод фамилии p1[i].lastname;
   - ввод имени p1[i].name;
   - ввод отчества p1[i].surname;
   - ввод пола p1[i].gender;
   - ввод должности p1[i].position;
   - ввод года рождения p1[i].year.
7. Определение и вывод сотрудников пенсионного возраста (мужчины старше 65, женщины старше 60, должность engineer):
   - для каждого сотрудника вычислить age = 2025 - p1[i].year;
   - если сотрудник соответствует критериям, вывести его данные на экран.
8. Вызов функции writefile для записи данных в файл "employees.txt":
   - открытие файла для записи;
   - запись всех данных сотрудников;
   - запись списка сотрудников пенсионного возраста (если есть);
   - закрытие файла.
9. Конец программы.
### Блок-схема

![Блок схема алгоритма](poto9.bmp)

## 2. Реализация программы:
#define _CRT_SECURE_NO_WARNINGS
#define _USE_MATH_DEFINES
#include <locale.h>
#include <stdio.h>
#include <stdlib.h>
#include <string.h> 
#include <conio.h>
#include <math.h>

struct employee {
    char lastname[20];
    char name[20];
    char surname[20];
    char gender[10];
    char position[20];
    int year;
};

typedef struct employee employee;
int writefile(char* fname, employee* emp, int size);
#define SIZE 3

int main()
{
    setlocale(LC_ALL, "RUS");
    employee p1[SIZE];
    char filename[] = "employees.txt";
    printf("=== Ввод данных о сотрудниках ===\n\n");
    for (int i = 0; i < SIZE; i++) {
        printf("--- Сотрудник %d ---\n", i + 1);

        printf("Фамилия: ");
        scanf("%19s", p1[i].lastname);

        printf("Имя: ");
        scanf("%19s", p1[i].name);

        printf("Отчество: ");
        scanf("%19s", p1[i].surname);

        printf("Пол (m/f): ");
        scanf("%9s", p1[i].gender);

        printf("Должность: ");
        scanf("%19s", p1[i].position);

        printf("Год рождения: ");
        scanf("%d", &p1[i].year);

        printf("\n");
    }

   
    printf("\n=== Сотрудники, подлежащие выводу (пенсионного возраста) ===\n");

    for (int i = 0; i < SIZE; i++) {
        int age = 2025 - p1[i].year;

        if ((strcmp(p1[i].gender, "m") == 0 && age > 65 && strcmp(p1[i].position, "engineer") == 0) ||
            (strcmp(p1[i].gender, "f") == 0 && age > 60 && strcmp(p1[i].position, "engineer") == 0))
        {
            printf("\n--- Сотрудник %d ---\n", i + 1);
            printf("Фамилия: %s\n", p1[i].lastname);
            printf("Имя: %s\n", p1[i].name);
            printf("Отчество: %s\n", p1[i].surname);
            printf("Пол: %s\n", p1[i].gender);
            printf("Должность: %s\n", p1[i].position);
            printf("Год рождения: %d\n", p1[i].year);
            printf("Возраст: %d\n", age);
        }
    }

 
    printf("\n=== Запись данных в файл ===\n");
    if (writefile(filename, p1, SIZE)) {
        printf("Данные успешно записаны в файл '%s'\n", filename);
    }
    else {
        printf("Ошибка записи в файл!\n");
    }

    return 0;
}

int writefile(char* fname, employee* emp, int size)
{
    FILE* out;

    if ((out = fopen(fname, "wt")) == NULL)
    {
        printf("Ошибка открытия файла для записи\n");
        return 0;
    }

    fprintf(out, "=== Данные о сотрудниках ===\n");
    fprintf(out, "Всего сотрудников: %d\n\n", size);

    for (int i = 0; i < size; i++)
    {
        fprintf(out, "--- Сотрудник %d ---\n", i + 1);
        fprintf(out, "Фамилия: %s\n", emp[i].lastname);
        fprintf(out, "Имя: %s\n", emp[i].name);
        fprintf(out, "Отчество: %s\n", emp[i].surname);
        fprintf(out, "Пол: %s\n", emp[i].gender);
        fprintf(out, "Должность: %s\n", emp[i].position);
        fprintf(out, "Год рождения: %d\n", emp[i].year);
        fprintf(out, "Возраст: %d\n", 2025 - emp[i].year);
        fprintf(out, "\n"); 
    }
    fprintf(out, "=== Сотрудники пенсионного возраста (инженеры) ===\n");
    int pension_count = 0;

    for (int i = 0; i < size; i++)
    {
        int age = 2025 - emp[i].year;

        if ((strcmp(emp[i].gender, "m") == 0 && age > 65 && strcmp(emp[i].position, "engineer") == 0) ||
            (strcmp(emp[i].gender, "f") == 0 && age > 60 && strcmp(emp[i].position, "engineer") == 0))
        {
            pension_count++;
            fprintf(out, "%d. %s %s %s, %s, возраст: %d\n",
                pension_count,
                emp[i].lastname,
                emp[i].name,
                emp[i].surname,
                emp[i].position,
                age);
        }
    }

    if (pension_count == 0) {
        fprintf(out, "Нет сотрудников пенсионного возраста среди инженеров.\n");
    }
    fclose(out);
    return 1; 
}

## 3. Результат работы программы
![Результат работы программы](image.png)
## 4. Информация о разработчике
Амелина Юлия, бИПТ-252
