# Configuring STM32 to work with float and double (converting values ​​to char type) under FreeRTOS.

If you use standard tools for converting float and double to char type such as printf, sprintf, snprintf, then when you run these conversions under FreeRTOS and using the GNU Arm Embedded Toolchain (https://developer.arm.com/tools-and-software/ open-source-software / developer-tools / gnu-toolchain / gnu-rm) the microcontroller hangs and a Hard Fault error occurs.
According to research on this topic http://www.nadler.com/embedded/newlibAndFreeRTOS.html, this error is associated with incorrect operation of dynamic memory allocation (malloc / malloc_r).

How is it all corrected.
1. Remove the heap_4.c file from the \ Middlewares \ Third_Party \ FreeRTOS \ Source \ portable \ MemMang \ directory of your project
2. Copy the heap_useNewlib.c file from the given repository (src) to the above directory;
3. Open the FreeRTOS configuration file (FreeRTOSConfig.h) and add an additional define to the section;
```
/* USER CODE BEGIN Defines */   	      
/* Section where parameter definitions can be added (for instance, to override default ones in FreeRTOS.h) */

#define configUSE_NEWLIB_REENTRANT 1

/* USER CODE END Defines */
```
### Remember to enable the extra option for the "-u _printf_float" compiler.

This completes the setup. Compilation and operation of the printf, sprintf, snprintf functions should occur without hovering and Hard Fault.

### Do not forget that when you re-generate the project in STM32CubeMX, the procedure will need to be repeated!

p.s. The version of the heap_useNewlib.c file provided in this repository is slightly different from the original (http://www.nadler.com/embedded/newlibAndFreeRTOS.html) in terms of fixing compilation errors. It was tested on the following versions of the STM32F103C8T6 and STM32F401CCU6 microcontrollers under Windows 10 and Ubuntu 19.10


# Настройка STM32 для работы с float и double (преобразования значений к типу char) под FreeRTOS.

Если использовать стандартные инструменты по преобразованию  float и double к типу char такие как printf, sprintf, snprintf, то при запуске этих преобразований под FreeRTOS и используя  GNU Arm Embedded Toolchain (https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm) наблюдается зависание микроконтроллера и появление ошибки Hard Fault.

Согласно исследованиям этой темы http://www.nadler.com/embedded/newlibAndFreeRTOS.html данная ошибка связанна с некорректной работой динамического выделения памяти (malloc/malloc_r).

Как это все правиться:
1. Удалите из директори \Middlewares\Third_Party\FreeRTOS\Source\portable\MemMang\ вашего проекта файл heap_4.c;
2. Скопируйте в вышеуказанную директорию файл heap_useNewlib.c из данного репозитария (src);
3. Откройте конфигурационный файл FreeRTOS (FreeRTOSConfig.h) и добавьте в секцию дополнительный define;
```
/* USER CODE BEGIN Defines */   	      
/* Section where parameter definitions can be added (for instance, to override default ones in FreeRTOS.h) */

#define configUSE_NEWLIB_REENTRANT 1

/* USER CODE END Defines */
```

### Не забудьте включить дополнительную опцию для компилятора "-u _printf_float".

На этом настройка закончена. Компиляция и работа функций printf, sprintf, snprintf должны происходить без зависания и Hard Fault.

### Не забывайте, что при пере генерации проекта в STM32CubeMX процедуру необходимо будет повторить!

p.s. Версия файла  heap_useNewlib.c приведенного в данном репозитарии немного отличается от исходной (http://www.nadler.com/embedded/newlibAndFreeRTOS.html) в части исправления ошибок компиляции. Проверялось на следующих версиях микроконтроллеров STM32F103C8T6 и STM32F401CCU6 под Windows 10 и Ubuntu 19.10