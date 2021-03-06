# Binary_Counter
Avr Assembly ile atmega328p üzerinde ledler yardımıyla bir binary countera hayat verilmiştir.Detaylı açıklamasını youtube kanalımdan izleyebilirsiniz.
Youtube kanalım->https://www.youtube.com/channel/UCrHQlXOK2GSIDMV41BJtEaQ
----------------------------------Kodlar----------------------------------
```javascript
;Author:Halil Gundogdu
.ORG 0

init:
ldi r16, 0b11101111 ; 0b11101111 5.pin giriş diğer pinler çıkış pini
out DDRB, r16 
ldi r28,0
loop:

ldi r30,8
cbi PINB,4

wait:
sbis PINB,PINB4		
rjmp wait	

subi r28,-1 

sub r30,r28 ;r30=r30-r25
brne HERE ;if count=7 finish program
ldi r16,0x00
out PORTB,r16
rcall blink ;blink led

rjmp init ;restarts program

HERE:
	
OUT PORTB,r28 ; show counts

rcall DELAY_01
rcall DELAY_01
rcall DELAY_01

rjmp loop



blink:
	ldi r31,10
	again:
		sbi PORTB,3
		rcall DELAY_01
		cbi PORTB,3
		rcall DELAY_01
		dec r31
		brne again
	ret
DELAY_01:              ; the subroutine:
  ldi r16, 6          ; load r16 with 31
OUTER_LOOP:            ; outer loop label
  clr r24 
                      
  clr r25  
DELAY_LOOP:            ; "add immediate to word": r24:r25 are
                       ; incremented
  adiw r24, 1          ; if no overflow ("branch if not equal"), go
                       ; back to "delay_loop"
  brne DELAY_LOOP
  dec r16              ; decrement r16
  brne OUTER_LOOP      ; and loop if outer loop not finished
  ret                  ; return from subroutine

```
