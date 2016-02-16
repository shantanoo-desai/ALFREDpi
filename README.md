## A.L.F.R.E.D for Raspberry Pi 2

* For __Raspberry Pi-2 Model B with 1 GB RAM__

* __Debian Wheezy version 7.8__


### Dependencies
If need be for applications download the following:

    $ apt-get update
    $ apt-get upgrade
    $ apt-get install libcap-dev libgps-dev  

### WARNING
even with `libcap-dev` there are compilation errors which can be solved by changing __Makefile__
accordingly.

Use this [Query](http://raspberrypi.stackexchange.com/questions/42640/cannot-install-a-l-f-r-e-d-on-raspberry-pi-2/42642#42642)
or follow the steps below:

1. In `Makefile`:

<pre>
   $ nano Makefile

	# find Line 28 (use CTRL+C)
	LDLIBS += -lrt # add these lines -lpcap -lcap
	
	# find Line 31
	export CONFIG_ALFRED_CAPABILITIES=n # change 'y' to 'n'
	
	# comment out Lines 84 - 91 which cause problems for libcap errors

</pre>

2. use `make` and `make install` accordingly:

<pre>
	make CONFIG_ALFRED_VIS=n CONFIG_ALFRED_GPSD=n # when you don't want to use batadv-vis and gpsd
	make CONFIG_ALFRED_VIS=n CONFIG_ALFRED_GPSD=n install # install without batadv-vis and gpsd
</pre>


### Scenario
- __Ubuntu Laptop__ with `batman-adv` connected to __Raspberry Pi 2__ over `wlan0` using Mesh Networking

- on Ubuntu Laptop for *master* mode: `alfred -i wlan0 -m`

- on Raspberry Pi 2 : `alfred -i wlan0 -d` (use '-d' for verbose)

* On a new Terminal of Raspberry Pi-2: 
    
    $ cat /etc/hostname | alfred -s 100

here `-s` is for send-data over the UNIX Socket

* On Ubuntu Laptop in a new Terminal: 

    $ alfred -r 100

here `-r` is to receive data over the UNIX Socket

* Data recorded will be shown in __JSON Format__ in the following way:

`{"MAC Address of PI", "hostnameOfPi\x0a"}`


### Pi into Slave Mode

- In a new Terminal of Pi: `alfred -i wlan0 -M slave`
