#include"LPC11XX.h"
int i=0;
int j=0;
//#define LED  (1ul<<i) 
//#define LEDOFF() LPC_GPIO2->DATA |= (1<<i) 
//#define LEDON() LPC_GPIO2->DATA &= (1<<i) 
void LedInit()
{
   LPC_SYSCON->SYSAHBCLKCTRL |= (1<<16);  
	 LPC_IOCON->PIO2_0 &= ~0x07;	
	 LPC_IOCON->PIO2_0 |=0x00;
	 LPC_IOCON->PIO2_1 &= ~0x07;	
	 LPC_IOCON->PIO2_1 |=0x00;
	 LPC_IOCON->PIO2_2 &= ~0x07;	
	 LPC_IOCON->PIO2_2 |=0x00;
	 LPC_IOCON->PIO2_3 &= ~0x07;	
	 LPC_IOCON->PIO2_3 |=0x00;
	 LPC_IOCON->PIO2_4 &= ~0x07;	
	 LPC_IOCON->PIO2_4 |=0x00;
	 LPC_IOCON->PIO2_5 &= ~0x07;	
	 LPC_IOCON->PIO2_5 |=0x00;
	 LPC_IOCON->PIO2_6 &= ~0x07;	
	 LPC_IOCON->PIO2_6 |=0x00;
	 LPC_IOCON->PIO2_7 &= ~0x07;	
	 LPC_IOCON->PIO2_7 |=0x00;
	 LPC_SYSCON->SYSAHBCLKCTRL &= ~(1<<16); 
	 LPC_SYSCON->SYSAHBCLKCTRL |= (1<<6);
	 for(j=0;j<=7;j++)
	{
	 LPC_GPIO2->DIR |= (1<<j);	
	 LPC_GPIO2->DATA |= (1<<j);
	}


}
void TIMER32_0_IRQHandler(void)
{
	
  
	LPC_TMR32B0->IR = 0x01;
	LPC_GPIO2->DATA |= 0xff;
	LPC_GPIO2->DATA &= ~(1<<i);
	i++;
  if(i==8)
	i=0;		

}
void Timer0Init(void)
{
  LPC_SYSCON->SYSAHBCLKCTRL |=(1<<9);
	LPC_TMR32B0->IR=0x01;
	LPC_TMR32B0->PR=0;
  LPC_TMR32B0->MCR=0x03;
	LPC_TMR32B0->MR0=SystemCoreClock/10;	
	LPC_TMR32B0->TCR=0x01;
	NVIC_EnableIRQ(TIMER_32_0_IRQn);
}

int main (void)
{
  LedInit();
	Timer0Init();
  while(1);	
}
