# Standby low power mode on STM32WB55 MCU

Standby is a low power mode available on STM32 MCUs, in this repository we share a STM32WB BLE code example on a standby use case.

You can find more information on our [WIKI page](https://wiki.st.com/stm32mcu/index.php?title=Connectivity:STM32WB_standby_low_power_mode)  

## Setup
This application is running on a **NUCLEO-WB55RG board**.   
Wake up pin 5 (WKUP5) on pin PC5 and present on CN7.3 is used to wake up the system from standby when grounded.  
A smartphone is needed to connect to STM32WB device with ST BLE Sensor application. You can get it from your platform application store.  
After flashing your board you have to unplug jumpers present on JP5 in order to reach best current measurements. These ones connect STM32WB to ST-LINK debugger.  
To measure current consumption of the MCU you can use our X-NUCLEO-LPM01A board and power Nucleo board from JP2 connector.

## Application description
This application is based on P2P server from CubeWB package v1.14.0.  
Some changes conduct to the following behavior:
- At power up device is booting, and starts BLE advertising.
- Once connection with ST BLE Sensor smartphone application is established you can turn on and off the LED for instance.
- At BLE disconnect, advertising is not restarted, and a timer will trigger the task called Go_standby() that will:
  - Clear standby flag if set
  - Disable WKUP5 and clear wake up flags.
  - Enable a pull-up resistor on WKUP5 pin.
  - Set wake up polarity to low on WKUP5 pin.
  - Enable WKUP5 pin.
  - Allow low power manager (LPM) to go into standby, UTIL_LPM_SetOffMode().
- Then low power manager will put the device in standby mode, current consumption around 1uA.
- System can be restarted by connecting to the ground WKUP5 pin.
- At reboot, WKUP5 and pull-up are disabled, wake up flags are cleared.
- Then application restart BLE adverting waiting for a connection.

## Troubleshooting

**Caution** : Issues and the pull-requests are **not supported** to submit problems or suggestions related to the software delivered in this repository. The STM32WB-BLE-standby example is being delivered as-is, and not necessarily supported by ST.

**For any other question** related to the product, the hardware performance or characteristics, the tools, the environment, you can submit it to the **ST Community** on the STM32 MCUs related [page](https://community.st.com/s/topic/0TO0X000000BSqSWAW/stm32-mcus).
