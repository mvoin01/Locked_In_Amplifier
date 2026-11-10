CD4053B behavioural LTspice model - quick start
===============================================

Files
  CD4053B.lib            Subcircuit model (.subckt CD4053B)
  CD4053B.asy            Schematic symbol (pins labelled as datasheet)
  CD4053B_test.asc       Demo, single supply: VDD=10V, VEE=0, 0..10V signals
  CD4053B_bipolar_test.asc  Demo, split supply: VDD=10V, VEE=-5V (edit .param VEEV), +/-4V signals
  CD4053B_Ron_sweep.cir  DC sweep of Ron vs input voltage (VDD=10V)

Install (simplest)
  Keep all the files in the same folder and open CD4053B_test.asc.
  The symbol's ModelFile attribute loads CD4053B.lib automatically.

Install (use in any schematic)
  1. Copy CD4053B.asy to  ...\Documents\LTspiceXVII\lib\sym\  (or LTspice\lib\sym\ for LTspice 24)
     - optionally in a subfolder such as "Custom".
  2. Copy CD4053B.lib to  ...\lib\sub\
  3. Place it with F2 (Component) and pick CD4053B.
  If LTspice reports "unknown subcircuit", add the directive  .lib CD4053B.lib
  (or the full path) to your schematic.

Connections (pin number in brackets)
  AX(12) AY(13) -> AOUT(14)   select by A(11)
  BX(2)  BY(1)  -> BOUT(15)   select by B(10)
  CX(5)  CY(3)  -> COUT(4)    select by C(9)
  INH(6) high = all switches open
  VDD(16), VSS(8) logic ground, VEE(7) negative analog supply (tie to VSS for 0..10V signals)
  Select low -> X input on; select high -> Y input on.

Tweaking parameters
  Ctrl-right-click the symbol, set SpiceLine, e.g.:   RON=400 TADDR=320n
  RON    peak on-resistance at VDD-VEE=10V (180 typ, 400 max @25C)
         Other supplies follow the datasheet table: 15V -> 125 typ (240 max), 5V -> 470 typ (1050 max)
         RON scales the whole table: RON=400 gives ~1040 (5V), 400 (10V), 278 (15V) - close to or slightly above the 25C max.
  RMOD   fractional Ron drop at the rails (0.35)
  TADDR  address->output delay (160n typ, 320n max @10V)
  TINH   inhibit->output delay (160n typ, 320n max @10V)
  ROFF   off-state leakage resistance (100G ~ 100pA at 10V)
  CIS, COS, CIOS, CIN   capacitances (5p, 9p, 0.2p, 5p)
  IDDQ   quiescent supply current (40n typ; 10u worst-case max)

Ron sweep: open CD4053B_Ron_sweep.cir, run, then plot  -0.05/I(VX)
  VEEV=0  : ~117 ohm near the rails, 180 ohm at mid-supply.
  VEEV=-5 : ~81 ohm near the rails, 125 ohm at mid-supply (2.5V).

Supply limits (not enforced by the model)
  VDD-VSS 3..18V, VDD-VEE <= 18V recommended (20V absolute max).
  Logic inputs swing VSS..VDD; analog signals VEE..VDD.
