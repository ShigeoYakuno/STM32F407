# STM32F407

# MCUDEV DevEBox STM32F407VET6 ハードウェア仕様（一次情報のみ）

---

# Specifications

* STM32F407VET6 ARM Cortex M4
* 168MHz, 210 DMIPS / 1.25 DMIPS / MHz
* 1.8V - 3.6V operating voltage
* 8MHz system crystal
* 32.768KHz RTC crystal
* 2.54mm pitch pins
* SWD header
* 512 KByte Flash, 192 + 4 KByte SRAM
* 3x SPI, 3x USART, 2x UART, 2x I2S, 3x I2C
* 1x FSMC, 1x SDIO, 2x CAN
* 1x USB 2.0 FS / HS controller (with dedicated DMA)
* 1x USB HS ULPI (for external USB HS PHY)
* Micro SD
* Winbond W25Q16 16Mbit SPI Flash
* 1x 10/100 Ethernet MAC
* 1x 8 to 12-bit Parallel Camera interface
* 3x ADC (12-bit / 16-channel)
* 2x DAC (12-bit)
* 12x general timers, 2x advanced timers
* AMS1117-3.3V: 3.3V LDO voltage regulator, max current 800mA
* Micro USB for power and comms
* Power LED D1
* User LED D2 (PA1) active low
* Reset button, 1x user buttons K0
* 2x22 side header
* SPI TFT/OLED header (3V3, GND, MOSI, SCK, CS, MISO, RST, BL)
* RTC battery header B1 beside SD card
* M3 mounting holes
* Dimensions: 40.89mm x 68.59mm

---

# Exposed Port Pins

* PA0-PA15
* PB0-PB3, PB5-PB15
  （PB4はSPI1_MISOとしてSPI Flash専用）
* PC0-PC13
  （PC14 OSC32_IN, PC15 OSC32_OUT は未引き出し）
* PD0-PD15
* PE0-PE15

---

# Peripherals

## TFT / OLED (J4)

| Pin | Signal    |
| --- | --------- |
| 1   | 3V3       |
| 2   | GND       |
| 3   | PB15 MOSI |
| 4   | PB13 SCK  |
| 5   | PB12 CS   |
| 6   | PB14 MISO |
| 7   | PC5 RS    |
| 8   | PB1 BLK   |

---

## SPI Flash W25Q16 (U3)

| Pin | Signal        |
| --- | ------------- |
| 1   | PA15 F_CS     |
| 2   | PB4 SPI1_MISO |
| 3   | WP 3V3        |
| 4   | GND           |
| 5   | PB5 SPI1_MOSI |
| 6   | PB3 SPI1_SCK  |
| 7   | HOLD 3V3      |
| 8   | VCC 3V3       |

---

## SWD debug (J1)

| Pin | Signal     |
| --- | ---------- |
| 1   | Boot0      |
| 2   | 3V3        |
| 3   | GND        |
| 4   | PA13 SWDIO |
| 5   | PA14 SWCLK |

---

## USB (J5)

| Pin | Signal      |
| --- | ----------- |
| 1   | VCC 5V      |
| 2   | PA11 USB_DM |
| 3   | PA12 USB_DP |
| 4   | NC (ID)     |
| 5   | GND         |

---

## Micro SD (U5)

| Pin | Signal        |
| --- | ------------- |
| 1   | PC10 SDIO_D2  |
| 2   | PC11 SDIO_D3  |
| 3   | PD2 SDIO_CMD  |
| 4   | 3V3           |
| 5   | PC12 SDIO_SCK |
| 6   | GND           |
| 7   | PC8 SDIO_D0   |
| 8   | PC9 SDIO_D1   |
| 9   | NC SD_NC      |

---

# User Interface

## User Button

* PA0 WK_UP

## User LED

* PA1（active low）

---

# Battery (B1)

| Pin | Signal |
| --- | ------ |
| 1   | BAT54C |
| 2   | GND    |

---

# Notes

* PB4はSPI Flash専用ピン
* PC14およびPC15は未接続

---

# 追記：最小タスク構成（ThreadX動作確認用）

---

# 目的

* ThreadX起動確認
* LED点滅確認
* UARTデバッグ出力確認

---

# 使用リソース

## GPIO

* PA1：User LED（active low）

## UART

* USART1

  * TX：PA9
  * RX：PA10

---

# ソフトウェア構成

* ThreadX使用
* スレッド数：1

---

# スレッド構成

## LED/UARTスレッド

処理内容：

* LEDトグル（PA1）
* UARTへデバッグ文字列送信
* 一定時間スリープ

---

# 動作仕様

ループ周期：

* 約500ms

動作：

1. LED反転
2. UARTへ文字列送信
3. スリープ

---

# 制約

* 他ペリフェラル未使用
* 割込みはThreadX標準のみ使用
* ビジーループ禁止（tx_thread_sleep使用）

---

# 成功条件

* LEDが一定周期で点滅する
* UARTに周期的なログが出力される

---

