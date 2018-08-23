# esp8266-ssd1351-oled
esp8266通过arduinoIDE使用 `https://github.com/adafruit/Adafruit-SSD1351-library` 连接 1.5'' oled显示屏.

### 关于库的修改

1. 安装库
   * 在菜单 sketch->include library->manage libraries...搜索ssd1351 最高版本是1.0.1
   * [github](https://github.com/adafruit/Adafruit-SSD1351-library)最高版本1.0.3 ,自己看方便下载吧.



2. 编译example/test提示如下错误

```
error: cannot convert 'volatile uint32_t* {aka volatile unsigned int*}' to 'PortReg* {aka volatile unsigned char*}' in assignment
     csport      = portOutputRegister(digitalPinToPort(cs));
```

打开 xxx/Arduino/libraries/Adafruit-SSD1351-library/Adafruit_SSD1351.h

在 

```
#if defined (SAM3X8E) ...

    typedef volatile RwReg PortReg;

    typedef uint32_t PortMask;
```

代码附近

``` 
1.0.3中恢复注释
//couldn't get EP8266 to work for some reason.  R.Lesniak
//#elif defined (ESP8266)
//    typedef volatile uint32_t PortReg;
//    typedef uint32_t PortMask;

1.0.1中手动添加
#elif defined (ESP8266)
    typedef volatile uint32_t PortReg;
    typedef uint32_t PortMask;
```



### 关于example/test的修改

1. 打开example带的test示例 修改对应gpio pins

   ```
   #define sclk D5
   #define mosi D7
   #define cs   D8
   #define dc   D2
   #define rst  D1
   ```

2. 初始化tft 语句使用上面被注释掉的

   `Adafruit_SSD1351 tft = Adafruit_SSD1351(cs, dc, MOSI, sclk, rst); `

   同时注释下面的

   `// Adafruit_SSD1351 tft = Adafruit_SSD1351(cs, dc, rst);`



注: testlines(), testfillrects()中代码会让我的8266重启 并在串口输出 >>>stack>>> 不知道为啥,现在知识基础不够,以后还玩的话再看吧... 现在直接注释

```
// testlines()
delay(500); 
  
// testfillrects()
delay(1000);
```

