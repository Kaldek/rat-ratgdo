## ratgdo powered from battery backup using Linear voltage regulator
It is possible to use a linear voltage regulator rather than a buck converter module to provide 5v for ESP boards that have an onboard 5v-3.3v voltage regulator.
<br>
<img src="/images/Breadboard-batterypower-Installed/BoardWired.png" width="50%" />


## Technical description
To provide 5v power to the ESP module a linear voltage regulator (L7805CV 5V regulator) was used, coupled with filtering capacitors on the input and output sides of the regulator (100uF cap on the input, 10uf electrolytic and .1uF ceramic on the output).  The 7805 can handle 7-36v and is suitable for use in droping the voltage down to 5v for the ESP modules.

The example shown uses vampire clips to connect to the 12v wiring of the battery backup circuit
<img src="/images/Breadboard-batterypower-Installed/BatteryBackupWires.png" width="50%" />

The full install is shown here.
<img src="/images/Breadboard-batterypower-Installed/CompleteInstall.png" width="50%" />
  
<br>
