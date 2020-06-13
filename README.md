# DS18B20-one-sensor
ногодрыг  взят с narodstrea_m	

использована на stm32f103c8t6  
пин подключения PB11  //переназначит в portinit()    
      
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
3.Работа:         
в main():  
port_init();  //переназначение параметров вывода PB11       
status=ds18b20_init(SKIP_ROM);  //инициализация датчика             
snprintf(str1,60,"_%d",status); //вывод статуса в буфер         
в While():         
ds18b20_MeasureTemperCmd(SKIP_ROM, 0);       //запрсо на измерение температуры               
HAL_Delay(800);                             //курим         
ds18b20_ReadStratcpad(SKIP_ROM, dt, 0);     //читаем        
snprintf(str1,60,"%02X %02X %02X %02X ",     сливаем в буфер            
dt[0], dt[1], dt[2], dt[3]);                     
raw_temper = ((uint16_t)dt[1]<<8)|dt[0];     // достаем байты с температурой        
if(ds18b20_GetSign(raw_temper)) c='-';        //определяем знак         
	  else c='+';           
 temper = ds18b20_Convert(raw_temper);        //достаем  занчение температуры из присланных байт            
 temrez=(uint8_t)(floor(temper*10));         //округляем для получения десятой  0,1  0,2        
 snprintf(str1,60,"%c%d.%d'C",c, ((uint8_t)(floor(temper))),temrez%10);  //сливаем в буфер +27.2'C          
