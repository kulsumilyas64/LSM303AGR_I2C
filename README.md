## Code for LSM303AGR Sensor (ACC + MAG)

```
#include "main.h"
#include<string.h>
#include<stdio.h>

/* Constants define for address*/

static const uint16_t Acc_ADDR  = 0011001 <<1;
static const uint16_t Mag_ADDR  = 0011110 <<1;
static const uint32_t OUT_X_L_A = 0101000 <<1;
static const uint32_t OUT_X_H_A = 0101001 <<1;
static const uint32_t OUT_Y_L_A = 0101010 <<1;
static const uint32_t OUT_Y_H_A = 0101011 <<1;
static const uint32_t OUT_Z_L_A = 0101100 <<1;
static const uint32_t OUT_Z_H_A = 0101101 <<1;
static const uint32_t OUTX_L_REG_M = 01101000;
static const uint32_t OUTX_H_REG_M = 01101001;
static const uint32_t OUTY_L_REG_M = 01101010;
static const uint32_t OUTY_H_REG_M = 01101010;
static const uint32_t OUTZ_L_REG_M = 01101100;
static const uint32_t OUTZ_H_REG_M = 01101101;
static const uint32_t CTRL_REG5_A     = 0x24;
static const uint32_t FIFO_CTRL_REG_A = 0x2E;
static const uint32_t FIFO_SRC_REG_A  = 0x2F;

int main(void)
{ 
HAL_StatusTypeDef ret;
uint8_t uart_buf[50];
int32_t val;

while (1)
  {
	  //for 1st register
	  uart_buf[0]= OUT_X_L_A;
	  ret = HAL_I2C_Master_Transmit(&hi2c1, Acc_ADDR, uart_buf,1, 6000);
	  if(ret !=HAL_OK){
		  strcpy((char*)uart_buf, "ERROR \r\n");
		  break;
	  }
	  else{
		  ret = HAL_I2C_Master_Receive(&hi2c1, Acc_ADDR, uart_buf,1,6000);
		  if(ret !=HAL_OK){
			  strcpy((char*)uart_buf,"ERROR\r\n");
		  }
		  else{
			  val = uart_buf[0];

			  printf((char*)uart_buf,val);
		  }
	  }

	  HAL_UART_Transmit(&huart2, uart_buf,strlen((char*)uart_buf),HAL_MAX_DELAY);
      HAL_Delay(500);
}
}
```
### Code is same for all 12 Registers (6 ACC_REG + 6 MAG_REG)
