# Setting Up Pi-hole on Raspberry Pi 4

## What is a Raspberry Pi?
A Raspberry Pi is a small, affordable single-board computer that can be used for a variety of projects including home automation, media centers, retro gaming, and network security tools.

## Whats is a Pi-hole?
Pi-hole is a free, open-source application that turns your Raspberry Pi into a network-wide DNS server and ad blocke. It filters all DNS requests on your network, blocking ads, trackers and malicious domains before they reach your devices.

## Why Pi-hole?
Pi-hole was chosen for this home lab project because it:

- Provides real world DNS monitoring experience
- Blocks ads and trackers at the network level affecting all devices on the network
- Gives visibility into all DNS queries on the network
- Is directly relevant to SOC analyst work
- Runs efficiently on a raspberry Pi 4

## Hardware Requirements

The following hardware was used in this project:

- Raspberry Pi 4 Model B (4GB RAM)
- CanaKit USB-C Power Supply
- CanaKit Black Case
- Cooling Fan
- 32GB Samsung MicroSD Card
- USB MicroSD Card Reader
- HDMI Cable
- GPIO Reference Card
- PiSwitch (On/Off Switch)

<img src="Screenshots/pihole-setup/pi-box.jpeg" width="600" alt="Raspberry Pi 4 Box">
<p><em>Raspberry Pi 4 Model B 4GB RAM box</em></p>

<img src="Screenshots/pihole-setup/pi-board.jpeg" width="600" alt="Raspberry Pi 4 Board">
<p><em>Raspberry Pi 4 Model B board showing all ports and GPIO pins</em></p>

<img src="Screenshots/pihole-setup/pi-powersupply.jpeg" width="600" alt="Power Supply">
<p><em>CanaKit USB-C power supply used to power the Raspberry Pi 4</em></p>

<img src="Screenshots/pihole-setup/pi-case.jpeg" width="600" alt="Black Case">
<p><em>CanaKit black case compatible with Raspberry Pi 4</em></p>

<img src="Screenshots/pihole-setup/pi-fan.jpeg" width="600" alt="Cooling Fan">
<p><em>CanaKit cooling fan connected to GPIO pins to prevent overheating</em></p>

<img src="Screenshots/pihole-setup/pi-microsd.jpeg" width="600" alt="MicroSD Card">
<p><em>32GB Samsung MicroSD card used as the Raspberry Pi storage</em></p>

<img src="Screenshots/pihole-setup/pi-card-reader.jpeg" width="600" alt="Card Reader">
<p><em>USB MicroSD card reader used to flash the OS from laptop</em></p>

<img src="Screenshots/pihole-setup/pi-hdmi.jpeg" width="600" alt="HDMI Cable">
<p><em>HDMI cable used for initial Pi setup and display connection</em></p>

<img src="Screenshots/pihole-setup/pi-gpio-card.jpeg" width="600" alt="GPIO Reference Card">
<p><em>CanaKit GPIO reference card used to identify correct pins for fan connection</em></p>