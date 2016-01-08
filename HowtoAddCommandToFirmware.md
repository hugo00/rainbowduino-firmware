# How to add a new command to Rainbowduino Firmware 3 #

## Introduction ##

In order to add a new functionality to the firmware, after writing the code of your procedure, you must create the `#define`, giving it a name and a byte-code, and you must register the number of parameter that your function requires.


## Firmware side ##

Edit **Rainbowduino\_Firmware\_v3\_0.pde** and write the code of your procedure:
```
void myProc(byte r, byte g, byte b) {
  [code of proc]
}
```

Edit **WireCommands.h** on firmware side and add a `#define` for your procedure:
```
#define CMD_MY_PROC     0x55
```

Now, edit again **Rainbowduino\_Firmware\_v3\_0.pde** and add the Wire command to the processing procedure. Find the function `processWireCommand()` and add a new `case` in the `switch` check:
```
void processWireCommand() {
  [...]
  switch(RainbowCMD[1]) {
    [...]
    case CMD_MY_PROC:
      CMD_ACKNOWLEDGED;
      r = RainbowCMD[2]; g = RainbowCMD[3]; b = RainbowCMD[4];
      myProc(r, g, b);
      break;
    [...]
  }
}
```

The macro `CMD_ACKNOWLEDGED` set the current command to `CMD_NOP`. It is required to avoid multiple execution of the same command.


## Master side ##

Edit **WireCommands.h** on the master side and add the `#define` for the procedure with the same name of the one you've defined on the firmware side:
```
#define CMD_MY_PROC     0x55
```

Edit **WireCommunication.c** and set the current number of parameters required by your procedure on the aguments array (the comments will help you to find the right location):
```
unsigned char CMD_totalArgs[MAX_WIRE_CMD] PROGMEM = {
//  0 - 1 - 2 - 3 - 4 - 5 - 6 - 7 - 8 - 9 - A - B - C - D - E - F 
    [...]
    0,  0,  0,  0,  0,  3,  0,  0,  0,  0,  0,  0,  0,  0,  0,  0,    // 5 - 0x50 -> 0x5F
    [...]
};
```

Now you can edit **Rainbowduino\_v3\_0\_Master.pde** and test your new command:
```
void loop() {
  [...]
  sendCMD(0x10, CMD_MY_PROC, 0xF, 0xA, 0x0);
  [...]
}
```


## Note ##

This architecture is currently under development.
Every issue is subject to change, including the name of the files, the function, the variable and their position in the code.