# Secure Network Access Project

This repository contains the configuration for a secure network access project implemented using Cisco Packet Tracer.

## Project Overview

The project involves the configuration of a network with multiple VLANs, distributed switches, a wireless LAN controller (WLC), DHCP configuration, Layer 3 routing, and firewall configuration. The network is designed to provide secure access across different departments in an organization.

## Network Configuration

The network configuration involves setting up VLANs, trunk and access ports, EtherChannel, and inter-VLAN routing. The VLANs created include:

- VLAN 10: Production
- VLAN 20: Admin
- VLAN 30: Finance
- VLAN 40: WLAN
- VLAN 50: Voice

Each VLAN is assigned to specific interfaces on the switches. EtherChannel is configured to provide link aggregation for increased bandwidth and redundancy.

## DHCP Configuration

DHCP pools are configured for each VLAN to provide dynamic IP address allocation. The DHCP configuration includes the network address, default router, and excluded addresses for each pool.

## Layer 3 Configuration

The Layer 3 configuration involves setting up Hot Standby Router Protocol (HSRP) for redundancy and inter-VLAN routing on the distributed switches. OSPF is used as the dynamic routing protocol.

## Firewall Configuration

The firewall configuration includes setting up NAT for the different VLANs and implementing access control lists (ACLs) to control traffic flow.

## Voice VLAN Configuration

A Voice VLAN is configured for VoIP services. The configuration includes setting up a DHCP pool for the Voice VLAN and configuring ephone-dns for different extensions.

Please refer to the configuration files in this repository for detailed configuration commands.

## Disclaimer

This project is for educational purposes only. Please use caution when implementing these configurations in a production environment.

