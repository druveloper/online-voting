# online-voting
A description of a plan to implement secure, nation-wide voting for any candidate, referendum, or ballot initiative, including a citizen's state and local elections.

## Introduction
Online voting, if done securely, holds great potential to enable a greater and more highly participatory democracy. However, in the most powerful nation in the world, compromising it contains so much reward that nearly any effort, no matter how great, to rig or influence an election substatially would most certainly be worth the effort and cost. Because of this, measures must be taken so that citizens can be sure the count is accurate and fair, and even that votes are cast by real human beings, let alone eligible citizens.

To solve this issue, I propose a kind of USB device which a citizen can plug into their phone/tablet/PC when casting a vote.

![Picture of device](USB-device.png)

Security considerations have been divided into 4 areas:
1. Manufacturing of the devices.
2. Registration of each device with a voter.
3. Casting a vote via one of these devices.
4. Counting the votes from these devices.

In each of these areas, the plan uses information entropy so that any nefarious activity encounters maximum difficulty, receives minimal reward, and risks potential exposure.

## I. Manufacturing
The devices should be manufactured in various securely controlled facilities across the nation. Then shipped to nearby Registration locations, where voters can go to receive a device. Voters do not need to return to the Registration location to cast a vote, only to receive a new device.

When manufactured, the facility will generate the following for each device:
1. **Device ID** - uniquely identifies the device.
2. **Data encryption public/private key pair** - for privacy of data going to the device.
3. **Data signature public/private key pair** - for authentication of data going to the device.
4. **Registration encryption public/private key pair** - for privacy when registering the device with a voter.
5. **Registration signature public/private key pair** - for authentication when registering the device with a voter.

Each device will be hard-wired with the Device ID and either the private or public portion of the Data keys and Registration keys. For each public/private key, the remaining counter-part, which does not go into the device, will go into a database within the facility along with the Device ID for association. The database counter-parts are used for server-side communication between the device and servers. For brevity, we can refer to these keys as merely the "Data key" or "Registration key" and infer which ones are used based their given purpose and location (device or server).

## II. Registration

## III. Casting a Vote

### IV. Counting the Votes
