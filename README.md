# PEAP-TOTP<!-- omit in toc -->

- [Abstract](#Abstract)
- [Problem](#Problem)
- [Solution](#Solution)
  - [Pros](#Pros)
  - [Cons](#Cons)
- [Technical Implementation Details](#Technical-Implementation-Details)

## Abstract

The PEAP+TOTP solution addresses the problem of WiFi PSK sharing or leaking by assigning every device (MAC address) a set of username and password at a lower cost as compared to a full fledged WPA2 Enterprise. This solution is faster and easier to deploy, has a lower cost to maintain, and is thus a good option for small businesses or homes with higher security demands.

## Problem

WPA2 PSK, or WPA2 Personal, as the name suggests, is originally intended for personal use. It is not very secure of a solution for businesses or homes with higher security requirements due to the shareability of the single pre-shared key. The key can be extracted by users or malicious apps and shared with unintended parties. This can even be done on iOS platforms.

WPA2 Enterprise is a safer solution, as each user needs to have an account on the RADIUS server. However, it might not feasible in all scenarios for the host to create a set of credentials for every guest, especially at businesses with high-flow of guests.

## Solution

TBD

### Pros

- WPA2 Enterprise provides significantly better security than PSK or plain portal
- WiFi key share app typically do not support WPA2 Enterprise
- Low cost, only a Raspberry Pi and several APs that support RADIUS are needed
- No manual user creation process for guests
- No shared accounts
- Provides the ability to log or invalidate individual devices
- Different usernames can be used to separate devices into different VLANs if supported by AP
- Credentials change over time automatically
  - There's no need to rotate the passwords manually
  - Already-connected devices will remain connected
- A kiosk/screen/pad can be used to display guest credentials to guests, or even use a TOTP authenticator app on the phone
  - This also implies that the guest must have physical access to the kiosk to obtain valid credentials

### Cons

- TBD

## Technical Implementation Details

The main program will be a RADIUS server providing auth for the AP. An admin creates a username/key pair (e.g., 'guest' and a random key), and all users will do auth to the AP using that username ('guest') and the 6-digit TOTP code derived from that random key. Once auth is finished, the auth information (device NAS number aka MAC address, username, TOTP code at that time) will be kept in database. When next time that device connects, it would automatically login using cached credential, eliminating the need to input password again.
