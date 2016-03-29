homebrew-msp430
===============

##Deprecated
MSP430 support has been added to mainline [GCC](http://gcc.gnu.org/git/?p=gcc.git;a=commit;h=e4a25868849c6594513a795a26be9da85b8b6ceb).

MSP430 GCC homebrew formulas

`brew tap tduehr/msp430`

___only tested on snow leopard currently___

based on [larsimmisch/avr](https://github.com/larsimmisch/homebrew-avr)

___Tested on El Captain___

# Running Cooja on MAC OS X (El Captain 10.11.4)

> Tested on java -version 7 and 8

``` bash
java version "1.7.0_60"
Java(TM) SE Runtime Environment (build 1.7.0_60-b19)
Java HotSpot(TM) 64-Bit Server VM (build 24.60-b09, mixed mode)


java version "1.8.0_77"
Java(TM) SE Runtime Environment (build 1.8.0_77-b03)
Java HotSpot(TM) 64-Bit Server VM (build 25.77-b03, mixed mode)
```

## Install Contiki
Download Contiki (version 3) or clone it. [Contiki](https://github.com/contiki-os/contiki)

Unpack it.

Download [mspsim](https://github.com/contiki-os/mspsim)

Unpack it in mspsim's contiki folder (contiki/tools/mspsim)

Run cooja ($ contiki/tools/cooja: ant run)

Try to open a simulation, for instance contiki/example/collect/example-collect-view.csc. It'll generate an error because of missing msp430-gcc.

## Install GCC

Install msp430-gcc using brew and this formulas [tduehr/homebrew-msp430](https://github.com/tduehr/homebrew-msp430)

>TODO: This is quite old version (GCC 4.6.3), version used by instant-contiki is 4.7.0, for future version update formula and add libc.a, libfp.a, libm.a compilation.

After install GCC, try to open a simulation again, now the error will be like below. This is because of missing libraries libc.a, libfp.a, and libm.a

``` shell
> make hello-world.sky TARGET=sky 
fatal: Not a git repository: '../../.git'
msp430-gcc -DCONTIKI=1 -DCONTIKI_TARGET_SKY=1 -DNETSTACK_CONF_WITH_IPV6=1 -DUIP_CONF_IPV6_RPL=1 -gstabs+ -Os -fno-strict-aliasing -ffunction-sections -Wall -mmcu=msp430f1611   -I. -I../../platform/sky/. -I../../platform/sky/dev -I../../platform/sky/apps -I../../platform/sky/net -I../../cpu/msp430/f1xxx -I../../cpu/msp430/. -I../../cpu/msp430/dev -I../../core/dev -I../../core/lib -I../../core/net -I../../core/net/llsec -I../../core/net/mac -I../../core/net/rime -I../../core/net/rpl -I../../core/sys -I../../core/cfs -I../../core/ctk -I../../core/lib/ctk -I../../core/loader -I../../core/. -I../../core/sys -I../../core/dev -I../../core/lib -I../../core/net/ipv6 -I../../core/net/ip -I../../core/net/rpl -I../../core/net/mac -I../../core/net -I../../core/net/mac/contikimac -I../../core/net/mac/cxmac -I../../core/net/llsec -I../../core/net/llsec/noncoresec -I../../dev/cc2420 -I../../dev/sht11 -I../../dev/ds2411 -I../../platform/sky/ -I../.. -DAUTOSTART_ENABLE -c hello-world.c -o hello-world.co
msp430-gcc -DCONTIKI=1 -DCONTIKI_TARGET_SKY=1 -DNETSTACK_CONF_WITH_IPV6=1 -DUIP_CONF_IPV6_RPL=1 -gstabs+ -Os -fno-strict-aliasing -ffunction-sections -Wall -mmcu=msp430f1611   -I. -I../../platform/sky/. -I../../platform/sky/dev -I../../platform/sky/apps -I../../platform/sky/net -I../../cpu/msp430/f1xxx -I../../cpu/msp430/. -I../../cpu/msp430/dev -I../../core/dev -I../../core/lib -I../../core/net -I../../core/net/llsec -I../../core/net/mac -I../../core/net/rime -I../../core/net/rpl -I../../core/sys -I../../core/cfs -I../../core/ctk -I../../core/lib/ctk -I../../core/loader -I../../core/. -I../../core/sys -I../../core/dev -I../../core/lib -I../../core/net/ipv6 -I../../core/net/ip -I../../core/net/rpl -I../../core/net/mac -I../../core/net -I../../core/net/mac/contikimac -I../../core/net/mac/cxmac -I../../core/net/llsec -I../../core/net/llsec/noncoresec -I../../dev/cc2420 -I../../dev/sht11 -I../../dev/ds2411 -I../../platform/sky/ -I../.. -MMD -c ../../platform/sky/./contiki-sky-main.c -o obj_sky/contiki-sky-main.o
msp430-gcc -mmcu=msp430f1611 -Wl,-Map=contiki-sky.map -Wl,--gc-sections,--undefined=_reset_vector__,--undefined=InterruptVectors,--undefined=_copy_data_init__,--undefined=_clear_bss_init__,--undefined=_end_of_init__  hello-world.co obj_sky/contiki-sky-main.o contiki-sky.a  -o hello-world.sky
/usr/local/Cellar/msp430-gcc/20120406/lib/gcc/msp430/4.6.3/../../../../msp430/bin/ld: cannot find -lc
collect2: ld returned 1 exit status
make: *** [hello-world.sky] Error 1
rm hello-world.co obj_sky/contiki-sky-main.o
Process returned error code 2
```

Add those libraries at those directories.

``` sh
lib/gcc/msp430/4.6.3
libc.a
libfp.a
libm.a

lib/gcc/msp430/4.6.3/mcpu-430x
libc.a
libfp.a
libm.a
```

To compile those libraries, follow README intructions at git://mspgcc.git.sourceforge.net/gitroot/mspgcc/msp430-libc (version 20120406)

## Note
After those proccedures, everything look fine, but simulator runs very slow, at least 10x slow. Need more investigation.
> Try use last version of GCC with msp430 native support.
