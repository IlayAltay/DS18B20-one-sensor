# DS18B20-one-sensor
ногодрыг  
использована на stm32f103c8t6 
пин подключения PB11  //переназнАчит в portinit() 
  
Использование библиотеки:   
1.подключение в main.c          #include "ds18b20.h"       
2.Список переменных:  
          глобальная буфер  char str1[60];          
          в main():
            uint8_t status;             //статус возврата функции инициализации датчика
            uint8_t dt[8];          // буфер чтения с датчика байты
            uint16_t raw_temper;    //достаем значение температуры из присланных байтов
            float temper;          // вычисляем занечние температуры
            char c;					// для хранеия знака температуры  + или -
            uint8_t temrez;	      // для промежуточных вычислений связано с float И sprintf   
3.           
