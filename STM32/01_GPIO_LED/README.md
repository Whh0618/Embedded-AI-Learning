介绍：利用开发板上的按键：SW1(PB11)和SW2(PB1)控制LED灯PA7和PA6的点亮
步骤：
1. 新建工程和配置按键引脚：  打开CubeIDE(后面简称CIDE)，新建工程，选择芯片，进入CIDE图形界面，将PB11和PB1引脚选择GPIO_Input，在GPIO选项中选择Pull-up，因为我的按键电路是接GND，所以当按键松开时保持按键为高电位，按键按下时，电路连通，按键引脚输出低点位，控制LED点亮。如果不选择up或down，电路会受干扰。

2. 配置LED的输出引脚，将PA7和PA6的引脚设置为GPIO_Output，电位设置为High，我的LED为低电平点亮，在未按下按键时，让LED保持高电位熄灭状态
3. 生成代码、编译和下载： 通过CIDE生成基础的引脚配置代码
4. 编写按键控制LED代码： 在用户编写出编写代码，代码如下：
 while (1)
  {
    /* USER CODE END WHILE */

    /* USER CODE BEGIN 3 */       // 本行开始到   USER CODE END 3   结束， 之间的代码是保护区，不会在重新生成时抹掉

	      GPIO_PinState SW1_state;      // 用GPIO_PinState 定义变量，用来储存GPIO读取按键引脚的电平（0或1）
	  	  SW1_state = HAL_GPIO_ReadPin(GPIOB,  GPIO_PIN_11);
	  	  	{
	  	  		  if ( SW1_state == GPIO_PIN_RESET )
	  	  		  {
	  	  			  //点亮PA7 低电平
	  	  			          HAL_GPIO_WritePin(GPIOA, GPIO_PIN_7, GPIO_PIN_RESET);
	  	  		  }
	  	  		  else
	  	  		  {
	  	  		        //关闭PA7
	  	  		        HAL_GPIO_WritePin(GPIOA, GPIO_PIN_7, GPIO_PIN_SET);
	  	  		  } /* USER CODE END WHILE */
	  	  	 }   // GPIO读取结束

	  	      GPIO_PinState SW2_state;
	  	      SW2_state = HAL_GPIO_ReadPin(GPIOB, GPIO_PIN_1);
	  	      if(SW2_state == GPIO_PIN_RESET)
	  	         {
	  	  	       HAL_GPIO_WritePin( GPIOA, GPIO_PIN_6, GPIO_PIN_RESET );
	  	          }
	  	      else
	  	      {
	  	      	HAL_GPIO_WritePin( GPIOA, GPIO_PIN_6, GPIO_PIN_SET );
	  	      }
  /* USER CODE END 3 */
}

5. 代码中的一些库函数说明
   GPIO_PinState X  : 利用变量定义函GPIO_PinState定义X
   HAL_GPIO_ReadPin(x ,y);  :HAL库中的引脚读取函数，将第一个变量 X(GPIOB) 里的 y(GPIO_PIN_11) 引脚电平读取出来，x 一般指GPIO，可以是A、B、C口，y可以是这些口的具体引脚。
   HAL_GPIO_WritePin(x,y);  :Hal库中的写函数，将某个引脚设置电平，比如(GPIOA, GPIO_PIN_7, GPIO_PIN_RESET);  将A口的第7引脚设置低电平
   

