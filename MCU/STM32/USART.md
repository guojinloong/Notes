简介
===
内容
===
## 重定向printf和scanf
```C
#if defined ( __GNUC__ )
#define PUTCHAR_PROTOTYPE int __io_putchar(int ch)
#define GETCHAR_PROTOTYPE int __io_getchar(void)
#elif defined ( __CC_ARM ) || defined(__ICCARM__)
#define PUTCHAR_PROTOTYPE int fputc(int ch, FILE *f)
#define GETCHAR_PROTOTYPE int fgetc(FILE *f)
#endif

/* Relocate the standard C lib function printf to USART, after relocation the function printf is availiable. */
PUTCHAR_PROTOTYPE
{
  HAL_UART_Transmit(&huart6, (uint8_t *)&ch, 1, 1);

  return (ch);
}

/* Relocate the standard C lib function scanf to USART, after relocation the function scanf, getchar is availiable. */
GETCHAR_PROTOTYPE
{
  int ch;

  HAL_UART_Receive(&huart6, (uint8_t *)&ch, 1, 1);

  return (ch);
}
```

另外，修改syscalls.c的\_read函数如下：
```C
int _read(int file, char *ptr, int len)
{
  int DataIdx;

  for (DataIdx = 0; DataIdx < len; DataIdx++)
  {
    *ptr = __io_getchar();
    if(0x7F == *ptr)
    {
        if(0 == DataIdx)
        {
          continue;
        }
    }
    __io_putchar(*ptr);
    if('\r' == *ptr)
    {
      *ptr = '\n';
      __io_putchar(*ptr);
      break;
    }
    if(0x7F == *ptr)
    {
      DataIdx--;
      ptr--;
    }
    else
    {
      DataIdx++;
      ptr++;
    }
  }
  len = DataIdx + 1;
  return len;
}
```

参考
===
* [STM32重定向printf()和scanf()到UART](https://my.oschina.net/igiantpanda/blog/1611419)
* [STM32 串口接收大量数据导致死机](https://www.cnblogs.com/qdrs/p/7701327.html)