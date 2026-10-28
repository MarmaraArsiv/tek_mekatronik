# PIC Mikrodenetleyici C Kod Çözümleri (LCD ve ADC)

Bu dosya, Hasan Hüseyin Çelik hocanın sınavlarında çıkan LCD ve ADC okuma sorusunun C programlama çözümlerini içerir.

```c
// LCD pin bağlantı tanımlamaları
sbit LCD_RS at RB4_bit;
sbit LCD_EN at RB5_bit;
sbit LCD_D7 at RB3_bit;
sbit LCD_D6 at RB2_bit;
sbit LCD_D5 at RB1_bit;
sbit LCD_D4 at RB0_bit;

// Pin yönlendirmeleri (TRIS)
sbit LCD_RS_Direction at TRISB4_bit;
sbit LCD_EN_Direction at TRISB5_bit;
sbit LCD_D7_Direction at TRISB3_bit;
sbit LCD_D6_Direction at TRISB2_bit;
sbit LCD_D5_Direction at TRISB1_bit;
sbit LCD_D4_Direction at TRISB0_bit;

#define led  PORTC.B0
#define led1 PORTC.B1

unsigned int pot;
unsigned char veri[6];

void main() {
    ADCON1 = 0x80;    // ADC konfigürasyonu
    TRISA  = 0xFF;    // Port A giriş (potansiyometre)
    TRISC  = 0;       // Port C çıkış (LED'ler)
    
    Lcd_Init();
    Lcd_Cmd(_LCD_CURSOR_OFF);
    Lcd_Cmd(_LCD_CLEAR);
    
    led  = 0;
    led1 = 0;
    
    do {
        pot = ADC_Read(3);              // Kanal 3'ten analog oku
        Lcd_Out(1, 1, "pot=");
        WordToStr(pot, veri);
        Lcd_Out(1, 5, veri);
        
        if (pot < 256) {
            RC0_bit = 1;
            RC1_bit = 0;
        }
        if (pot >= 256 && pot < 512) {
            led  = 0;
            led1 = 1;
        }
        if (pot >= 512 && pot < 1024) {
            led  = 1;
            led1 = 1;
        }
    } while (1);
}
```
