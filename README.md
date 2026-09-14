# swiftupi

**offline upi payment via bluetooth mesh**

swiftupi is a proof-of-concept exploring how a upi payment instruction could be transferred between nearby devices using bluetooth mesh when internet connectivity is unavailable.

## problem

in areas with poor or no internet connectivity, users may not be able to initiate or receive digital payments reliably.

## idea

swiftupi explores a **store-and-forward** approach:

**sender → nearby devices → recipient → upi processing**

payment instructions can move through nearby devices until they reach the intended recipient or a device with internet connectivity.

## status

🚧 **pre-development / mvp in progress**

this project is an experimental proof-of-concept, not a production-ready payment system.

## goals

- explore offline payment instruction delivery
- build a bluetooth mesh communication layer
- handle delayed payment delivery
- maintain basic transaction state and integrity
- understand the limitations of offline digital payments

## tech stack

- java
- spring boot
- bluetooth mesh
- sqlite / local storage
- git & github

## author

swiftupi does **not** replace upi or process real money. it is being developed for learning, experimentation, and proof-of-concept purposes.

see you in the next build 
— aps