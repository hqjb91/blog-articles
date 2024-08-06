Classless Inter-Domain Routing (CIDR) is a way to allocate IP address.
#### Classful addressing system (In the past)

- IPv4 address consists of 32 bits with each string of period separated numbers being 8 bits represented by 0 to 255 and organizations could purchase 3 classes
	- Class A
		- 8 network prefix bit e.g. 44.0.0.1
			- Where 44 is the network address pointing to network's unique identified
			- and 0.0.1 is the host address indicating the host/individual device identifier on the network
	- Class B
		- 16 network prefix bit e.g. 128.16.0.2
			- Where 128.16 is the network address
			- 0.2 is the host address
	- Class C
		- 24 network prefix bit e.g. 192.168.1.100
			- Where 192.168.1 is the network address
			- 100 is the host address
#### Classless address

- Uses variable address subnet masking (rather than supporting a fixed number of devices)
	- Subnet mask returns address's value from IP address by turning host address into zeroes
		- e.g. 192.0.2.0/24 is an IPv4 CIDR address where the first 24 bits e.g. 192.0.2 is the network address
- CIDR blocks
	- Collection of IP addresses that share the same network prefix and number of bits. A large block consists of more IP addresses and a small suffix.
		- Internet Assigned Numbers Authority (IANA) assigns large CIDR blocks to regional internet registries (RIR)
		- RIR assigns smaller blocks to local internet registries (LIR)
		- LIR assign them to organizations
		- Private users apply for CIDR blocks from their internet service providers
- CIDR notation
	- CIDR notation represents an IP address and a suffix that indicates network identifier bits in a specified format. For example, you could express 192.168.1.0 with a 22-bit network identifier as 192.168.1.0/22.
- CIDR in IPv6
	- IPv6 is a networking addressing system designed to replace IPv4. IPv6 uses a 128-bit unique identifier, which allows it to hold 1,028 times more IP addresses than IPv4.
	- An IPv6 address consists of 8 colon-separated hexadecimal values. IPv6 allows a much larger address space to accommodate the increasing number of devices that are connecting to the internet today.
	- Under Classless Inter-Domain Routing (CIDR), IPv6 addresses can be aggregated with prefixes of arbitrary bit length, similar to IPv4 addresses. For example, 2001:0db8:/32 is an IPv6 CIDR address, with the first 32 bits, or 2001:db8, as the network address.
- For example, `/8` corresponds to `11111111.00000000.00000000.00000000` and the more readable subnet mask `255.0.0.0`
- E.g. 
	- Household home network has a standard subnet mask of 255.255.255.0 or `/24`
		- Convert to bits : 11111111. 11111111. 11111111. 00000000 (we can count 24 numbers of 1)
		- 254 usable IP addresses within the defined network - can connect up to 254 internet-enabled devices
		- the 1s represent the unchanging bits of the network address and 0s represent the individual changing bits of the host address