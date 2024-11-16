# Classless Inter-Domain Routing (CIDR)

Classless Inter-Domain Routing (CIDR) is a method for allocating IP addresses and managing routing by introducing flexibility in defining network addresses and subnetting.

---

## Classful Addressing System

In the past, IP address allocation followed a **classful addressing system** with fixed prefix lengths. An IPv4 address consists of 32 bits, divided into four 8-bit segments (octets) separated by periods, each ranging from 0 to 255.

### Classes of IPv4 Addresses:
1. **Class A**
   - **Network Prefix**: 8 bits (e.g., `44.0.0.1`)
     - `44`: Network address (unique identifier for the network).
     - `0.0.1`: Host address (identifier for a device within the network).

2. **Class B**
   - **Network Prefix**: 16 bits (e.g., `128.16.0.2`)
     - `128.16`: Network address.
     - `0.2`: Host address.

3. **Class C**
   - **Network Prefix**: 24 bits (e.g., `192.168.1.100`)
     - `192.168.1`: Network address.
     - `100`: Host address.

This system, while straightforward, was inefficient due to rigid prefix lengths and wasteful allocation.

---

## Classless Addressing System

CIDR introduced **variable-length subnet masking (VLSM)**, which allows more efficient allocation of IP addresses.

### Key Features:
1. **Subnet Mask**:
   - Defines the network and host portions of an IP address by converting host bits to zero.
   - Example: `192.0.2.0/24`
     - **Network Address**: First 24 bits (`192.0.2`).
     - **Host Portion**: Remaining bits set to zero.

2. **CIDR Blocks**:
   - Represents a range of IP addresses with a shared network prefix.
   - **Hierarchy**:
     1. **Internet Assigned Numbers Authority (IANA)** assigns large CIDR blocks to **Regional Internet Registries (RIR)**.
     2. **RIRs** delegate smaller blocks to **Local Internet Registries (LIR)**.
     3. **LIRs** assign addresses to organizations, and ISPs provide CIDR blocks to private users.

3. **CIDR Notation**:
   - Combines an IP address with a suffix indicating the number of network prefix bits.
   - Example: `192.168.1.0/22`
     - **22-bit Prefix**: Represents the network portion of the address.

---

## CIDR in IPv6

IPv6 is the next-generation addressing system, designed to replace IPv4 with significantly more address space. It uses **128-bit unique identifiers**, expressed as colon-separated hexadecimal values.

### IPv6 Features
- Vastly larger address space to accommodate growing internet-connected devices.
- Supports CIDR, enabling aggregation with variable-length prefixes like IPv4.
  - Example: `2001:0db8:/32`
    - **Network Address**: First 32 bits (`2001:db8`).

---

## Examples and Practical Applications

### Subnet Masks
- A subnet mask defines the network and host portion of an IP address.
- Example:
  - `/8` corresponds to:
    - Binary: `11111111.00000000.00000000.00000000`.
    - Decimal: `255.0.0.0`.

### Home Network Example
- A typical household network uses a subnet mask of `255.255.255.0` or `/24`.
  - Binary: `11111111.11111111.11111111.00000000`.
  - Details:
    - **Usable IP Addresses**: 254 (devices can connect within the network).
    - **Subnet Mask**:
      - `1s`: Fixed network address bits.
      - `0s`: Variable host address bits.