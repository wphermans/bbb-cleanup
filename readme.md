<dl>
	<dt><h4>USB gadget modules</h4></dt>
	<dd>Renaming the file <em>am335x_evm.sh</em> will stop g_multi and related files from loading at boot.</dd>
</dl>

---
```
debian@beaglebone:~$ cd /opt/scripts/boot/ 
debian@beaglebone:/opt/scripts/boot$ mv am335x_evm.sh am335x_evm.sh.bak  
```
---

<dl>
	<dt><h4>Unwanted kernel modules</h4></dt>
	<dd>The following script, and additional commands illustrates how to remove unwanted kernel modules. Take care to remove 'blacklist' files for modules you wish to run. After this script has finished.</dd>
</dl>

---
```
debian@beaglebone:~$ nano remove-modules.sh`
```
---
```sh
#!/bin/sh

modules=$(lsmod|cut -d ' ' -f 1)
modpath="/etc/modprobe.d"
for module in  $modules
do
        if [ $module != 'Module' ] ; then
                touch $modpath/$module.conf
                echo 'install ' $module ' /bin/true' > $modpath/$module.conf
        fi
done
```
---

```
debian@beaglebone:~$ chmod +x ./remove-modules.sh
debian@beaglebone:~$ sudo ./remove-modules.sh
debian@beaglebone:~$ cd /etc/modprobe.d/
debian@beaglebone:/etc/modprobe.d$ ls
c_can.conf            omap_aes_driver.conf  pwm_tiecap.conf         snd_soc_core.conf           spi_omap2_mcspi.conf
c_can_platform.conf   omap_rng.conf         pwm_tiehrpwm.conf       snd_soc_davinci_mcasp.conf  spidev.conf.bak
can_dev.conf          omap_sham.conf        rng_core.conf           snd_soc_edma.conf           tieqep.conf
evdev.conf            pru_rproc.conf        snd.conf                snd_soc_omap.conf           uio.conf
fbdev-blacklist.conf  pruss.conf            snd_pcm.conf            snd_timer.conf              uio_pdrv_genirq.conf
g_ether.conf          pruss_intc.conf       snd_pcm_dmaengine.conf  soundcore.conf
```
```
/* Remove the conf file(s) that represent the modules you need loaded. Then reboot.*/  
debian@beaglebone:/etc/modprobe.d$ sudo reboot  
```
---

```
debian@beaglebone:~$ lsmod  
Module                  Size  Used by
nfsd                  261377  2
debian@beaglebone:~$ sudo depmod -ae 			/* Ignore warning */  
debian@beaglebone:~$ sudo update-initramfs -u 	/* Changes will take effect next boot */  
```
---
