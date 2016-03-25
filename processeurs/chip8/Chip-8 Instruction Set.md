# Chip-8 Instruction Set

<P>(all values in Hexadecimal unless stated) 
<HR>

<P>The Chip-8 instruction set runs in 4k of memory (addresses 000 - FFF). 
Programs start at 200, memory before that containing the chip-8 interpreter on a 
real 1802 based machine. The screen is 64 x 32 (128 x 64 on superchip) and is 
monochrome. Their is a sound buzzer 
<P>There are 16 primary registers, called v0 - vf. vf is used for carries and 
borrows and shouldn't really be used as a general purpose register. There is a 
12 bit index register called I. There is a program counter and stack pointer, 
but neither of these are accessible from program code. 
<P>There are 2 counters, the sound timer and the delay timer. Both count down at 
about 60Hz (on Chip8 they count down in threes using the PC's 18.2Hz Clock). 
When the sound timer is non-zero the buzzer sounds. 
<P>This is the Chip-8 Instruction set as I understand it.... 
<P>
<TABLE cellPadding=2 border=1>
  <TBODY>
  <TR>
    <TD>Code </TD>
    <TD>Assembler </TD>
    <TD>Description </TD>
    <TD>Notes </TD></TR>
  <TR>
    <TD>00Cx </TD>
    <TD>scdown x </TD>
    <TD>Scroll the screen down x&nbsp;lines </TD>
    <TD>Super only, not implemented </TD></TR>
  <TR>
    <TD>00E0 </TD>
    <TD>cls </TD>
    <TD>Clear the screen </TD>
    <TD></TD></TR>
  <TR>
    <TD>00EE </TD>
    <TD>rts </TD>
    <TD>return from subroutine call </TD>
    <TD></TD></TR>
  <TR>
    <TD>00FB </TD>
    <TD>scright </TD>
    <TD>scroll screen 4 pixels right </TD>
    <TD>Super only,not implemented </TD></TR>
  <TR>
    <TD>00FC </TD>
    <TD>scleft </TD>
    <TD>scroll screen&nbsp;4 pixels left </TD>
    <TD>Super only,not implemented </TD></TR>
  <TR>
    <TD>00FE </TD>
    <TD>low </TD>
    <TD>disable extended screen mode </TD>
    <TD>Super only </TD></TR>
  <TR>
    <TD>00FF </TD>
    <TD>high </TD>
    <TD>enable extended screen mode (128 x 64) </TD>
    <TD>Super only&nbsp; </TD></TR>
  <TR>
    <TD>1xxx </TD>
    <TD>jmp xxx </TD>
    <TD>jump to address xxx </TD>
    <TD></TD></TR>
  <TR>
    <TD>2xxx </TD>
    <TD>jsr xxx </TD>
    <TD>jump to subroutine at address xxx </TD>
    <TD>16 levels maximum </TD></TR>
  <TR>
    <TD>3rxx </TD>
    <TD>skeq vr,xx </TD>
    <TD>skip if register r =&nbsp;constant </TD>
    <TD></TD></TR>
  <TR>
    <TD>4rxx </TD>
    <TD>skne vr,xx </TD>
    <TD>skip if register r&nbsp;&lt;&gt; constant </TD>
    <TD></TD></TR>
  <TR>
    <TD>5ry0 </TD>
    <TD>skeq vr,vy </TD>
    <TD>skip f register r = register y </TD>
    <TD></TD></TR>
  <TR>
    <TD>6rxx </TD>
    <TD>mov vr,xx </TD>
    <TD>move constant to register r </TD>
    <TD></TD></TR>
  <TR>
    <TD>7rxx </TD>
    <TD>add vr,vx </TD>
    <TD>add constant to register r </TD>
    <TD>No carry generated </TD></TR>
  <TR>
    <TD>8ry0 </TD>
    <TD>mov vr,vy </TD>
    <TD>move register vy into vr </TD>
    <TD></TD></TR>
  <TR>
    <TD>8ry1 </TD>
    <TD>or rx,ry </TD>
    <TD>or register vy into register vx </TD>
    <TD></TD></TR>
  <TR>
    <TD>8ry2 </TD>
    <TD>and rx,ry </TD>
    <TD>and register vy into register vx </TD>
    <TD></TD></TR>
  <TR>
    <TD>8ry3 </TD>
    <TD>xor rx,ry </TD>
    <TD>exclusive or register ry into register rx </TD>
    <TD></TD></TR>
  <TR>
    <TD>8ry4 </TD>
    <TD>add vr,vy </TD>
    <TD>add register vy to vr,carry in&nbsp;vf </TD>
    <TD></TD></TR>
  <TR>
    <TD>8ry5 </TD>
    <TD>sub vr,vy </TD>
    <TD>subtract register vy from vr,borrow in&nbsp;vf </TD>
    <TD>vf set to 1 if borroesws </TD></TR>
  <TR>
    <TD>8r06 </TD>
    <TD>shr vr </TD>
    <TD>shift register vy right, bit 0 goes into&nbsp;register vf </TD>
    <TD></TD></TR>
  <TR>
    <TD>8ry7 </TD>
    <TD>rsb vr,vy </TD>
    <TD>subtract register vr from register vy, result in vr </TD>
    <TD>vf set to 1 if borrows </TD></TR>
  <TR>
    <TD>8r0e </TD>
    <TD>shl vr </TD>
    <TD>shift register vr left,bit 7 goes into register&nbsp;vf </TD>
    <TD></TD></TR>
  <TR>
    <TD>9ry0 </TD>
    <TD>skne rx,ry </TD>
    <TD>skip if register rx &lt;&gt; register ry </TD>
    <TD></TD></TR>
  <TR>
    <TD>axxx </TD>
    <TD>mvi xxx </TD>
    <TD>Load index register with constant xxx </TD>
    <TD></TD></TR>
  <TR>
    <TD>bxxx </TD>
    <TD>jmi&nbsp;xxx </TD>
    <TD>Jump to address xxx+register v0 </TD>
    <TD></TD></TR>
  <TR>
    <TD>crxx </TD>
    <TD>rand vr,xxx &nbsp;&nbsp; </TD>
    <TD>vr = random number less than or&nbsp;equal to xxx </TD>
    <TD></TD></TR>
  <TR>
    <TD>drys </TD>
    <TD>sprite rx,ry,s </TD>
    <TD>Draw sprite at screen location rx,ry height s </TD>
    <TD>Sprites stored in memory at location in index register, maximum 
      8&nbsp;bits wide. Wraps around the screen. If when drawn, clears a pixel, 
      vf is&nbsp;set to 1 otherwise it is zero. All drawing is xor&nbsp;drawing 
      (e.g. it toggles the screen pixels </TD></TR>
  <TR>
    <TD>dry0 </TD>
    <TD>xsprite rx,ry </TD>
    <TD>Draws extended sprite at screen location rx,ry </TD>
    <TD>As above,but sprite is always 16 x&nbsp;16. Superchip only, not 
      yet&nbsp;implemented </TD></TR>
  <TR>
    <TD>ek9e </TD>
    <TD>skpr k </TD>
    <TD>skip if key (register rk) pressed </TD>
    <TD>The key is a key number, see the chip-8 documentation </TD></TR>
  <TR>
    <TD>eka1 </TD>
    <TD>skup k </TD>
    <TD>skip if key (register rk) not pressed </TD>
    <TD></TD></TR>
  <TR>
    <TD>fr07 </TD>
    <TD>gdelay vr </TD>
    <TD>get delay timer into&nbsp;vr </TD>
    <TD></TD></TR>
  <TR>
    <TD>fr0a </TD>
    <TD>key vr </TD>
    <TD>wait for for keypress,put key in register vr </TD>
    <TD></TD></TR>
  <TR>
    <TD>fr15 </TD>
    <TD>sdelay vr </TD>
    <TD>set the delay timer to vr </TD>
    <TD></TD></TR>
  <TR>
    <TD>fr18 </TD>
    <TD>ssound vr </TD>
    <TD>set the sound timer to vr </TD>
    <TD></TD></TR>
  <TR>
    <TD>fr1e </TD>
    <TD>adi vr </TD>
    <TD>add register vr to the index register </TD>
    <TD></TD></TR>
  <TR>
    <TD>fr29 </TD>
    <TD>font vr </TD>
    <TD>point I to the sprite for hexadecimal character in vr </TD>
    <TD>Sprite is 5 bytes high </TD></TR>
  <TR>
    <TD>fr30 </TD>
    <TD>xfont vr </TD>
    <TD>point I to the sprite for hexadecimal character in vr </TD>
    <TD>Sprite is 10 bytes high,Super only </TD></TR>
  <TR>
    <TD>fr33 </TD>
    <TD>bcd vr </TD>
    <TD>store the bcd representation of register vr at location I,I+1,I+2 </TD>
    <TD>Doesn't change I </TD></TR>
  <TR>
    <TD>fr55 </TD>
    <TD>str v0-vr </TD>
    <TD>store registers v0-vr at location I onwards </TD>
    <TD>I&nbsp;is incremented to point to&nbsp;the next location on. e.g. 
      I&nbsp;= I + r + 1 </TD></TR>
  <TR>
    <TD>fx65 </TD>
    <TD>ldr v0-vr </TD>
    <TD>load registers v0-vr from location I onwards </TD>
    <TD>as above. </TD></TR></TBODY></TABLE>
<P>
<HR>


</BODY></HTML>
