# Лабораторная работа №1: Структуры данных
## Задание:
![image](https://github.com/user-attachments/assets/dc439e25-e0f0-48ea-939b-7e928fecdc06)
## Реализация:
### Листинг программы:
``` c++
#include <iostream>
#include <chrono>
#ifdef _WIN32
    #include <windows.h>
    #include <psapi.h>
#else
    #include <sys/resource.h>
#endif

using namespace std;
using namespace chrono;

// Расширенный алгоритм Евклида
long long gcd_extended(long long a, long long b, long long &x, long long &y) {
    if (a == 0) {
        x = 0, y = 1;
        return b;
    }
    long long x1, y1;
    long long g = gcd_extended(b % a, a, x1, y1);
    x = y1 - (b / a) * x1;
    y = x1;
    return g;
}

// Получение использования памяти
long long getMemoryUsage() {
#ifdef _WIN32
    PROCESS_MEMORY_COUNTERS pmc;
    GetProcessMemoryInfo(GetCurrentProcess(), &pmc, sizeof(pmc));
    return pmc.WorkingSetSize / 1024; // В КБ
#else
    struct rusage usage;
    getrusage(RUSAGE_SELF, &usage);
    return usage.ru_maxrss; // В КБ
#endif
}

int main() {
    cout << "Автор: Мареев-Королев Алексей Сергеевич, группа: ПОВа-024" << endl;
    cout << "Введите три натуральных числа: ";
    long long a, b, c;
    cin >> a >> b >> c;

    auto start_time = high_resolution_clock::now();

    long long x0, y0;
    long long g = gcd_extended(a, b, x0, y0);

    cout << "Результат: ";
    if (c % g != 0) {
        cout << "Impossible" << endl;
    } else {
        // Базовое решение
        x0 *= c / g;
        y0 *= c / g;

        // Шаг изменения решений
        long long dx = b / g;
        long long dy = a / g;

        // Ищем наименьшее неотрицательное x
        long long k = (-x0) / dx;
        if (x0 + k * dx < 0) ++k;

        long long x = x0 + k * dx;
        long long y = y0 - k * dy;

        cout << x << " " << y << endl;
    }

    auto end_time = high_resolution_clock::now();
    duration<double> elapsed_time = end_time - start_time;
    cout << "Время выполнения: " << elapsed_time.count() << " сек" << endl;
    cout << "Используемая память: " << getMemoryUsage() << " КБ" << endl;

    cout << "Автор: Мареев-Королев Алексей Сергеевич, группа: ПОВа-024" << endl;
    return 0;
}

```
## Результаты выполнения программы:
![image](https://github.com/user-attachments/assets/8a3a0fb0-1abc-4dbc-983a-f0bd44b8b294)



