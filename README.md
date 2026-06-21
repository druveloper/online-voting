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

In each of these areas, the plan uses information entropy so that any nefarious activity encounters maximum difficulty, receives minimal reward, and risks potential exposure. The plan also assumes a universal data contract used by servers so there are no incompatibilties with communication, authentication, or sharing of data.

## I. Manufacturing
The devices should be manufactured in various securely controlled facilities across the nation. Once made, they are shipped to nearby Registration locations, where voters can go to receive a device. Voters do not need to return to the Registration location to cast a vote, only to receive a new device.

When manufactured, the facility will generate the following for each device:
1. **Device ID** - uniquely identifies the device.
2. **Data encryption public/private key pair** - for privacy of data going to the device.
3. **Data signature public/private key pair** - for authentication of data going to the device.
4. **Registration encryption public/private key pair** - for privacy when registering the device with a voter.
5. **Registration signature public/private key pair** - for authentication when registering the device with a voter.

Each device will be hard-wired with the Device ID and either the private or public portion of the Data keys and Registration keys. For each public/private key, the remaining counter-part, which does not go into the device, will go into a database within the facility along with the Device ID for association. The database counter-parts are used for server-side communication between the device and servers. For brevity, we can refer to these keys as merely the "Data key" or "Registration key" and infer which ones are used based their given purpose and location (device or server). The aforementioned database will be called the "Manufacturer Database."

## II. Registration
Each Registration location will have its own "Registration Database" for the voters it registers. In order to securely and efficiently register devices, an additional type of device, which we'll call a "Registration Machine," has to be made and distributed to Registration locations, one machine for each line of people expected. Each machine will have its own client-side certificate and encryption key for communication with the Registration Database. Each machine will also have only enough memory for one voter to be registered, and an idle timer that clears the memory after a certain time limit.

### How to Register a Device
To register a device, it must be plugged into a Registration Machine. A registration staff person can populate the voter's personal information via PC connected to the machine. Then the following can happen:
1. The machine reads the Device ID from the device.
2. The machine requests a new "unconfirmed" registration for the device from the Registration Database.
3. The Registration Database generates a new record with a public/private key pair -- the "Voter Key."
4. The private portion of the Voter key is encrypted and signed using the Data Key from the Manufacturer Database.
5. The following is then sent back to the Registration Machine.
    -  Voter key, encrypted and signed by Data Key
    -  A randomly generated Salt code (for use with thumb print hash)
6. The Registration Machine hashes the voter's personal information and generates a random AES key.
7. The Registration Machine sends the following to the device in a request for a new registration:
    -  hash of voter's personal information
    -  AES key
    -  Voter key (received from Registration Database)
    -  Salt code (received from Registration Database)
8. The device validates the Voter key and prompts the voter to scan their thumb print.
9. Once scanned, the thumb print is hashed into a "thumb-hash." The thumb-hash and Voter key are recorded in the device's persistent memory.
10. The device returns the following:
    - hash of the Salt code combined with the thumb-hash, signed by Registration Key
    - signature of voter's personal information, made via Registration Key
    - AES key, encrypted by Registration key
11. The Registration Machine then sends the following to the Registration Database to "confirm" the registration:
    - AES key, encrypted by Registration key
    - the voter data, encrypted by the AES key and signed by Registration Key
    - salted hash of thumb-hash, signed by Registration Key
12. The machine receives confirmation the registration was successful.

## III. Casting a Vote

## IV. Counting the Votes
