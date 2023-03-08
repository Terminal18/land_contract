# land_contract
Ethereum smart contract for Terminal18 Lands

## Overview

This is a smart contract for Terminal18 Lands. 
It is a simple ERC721 contract with a few extra features.
The ERC721 contract features are based on the OpenZeppelin implementation (which has been audited).

## Features

### ERC721

The contract implements the ERC721 standard, for which you can find the specification here: https://eips.ethereum.org/EIPS/eip-721

### Additional features

The contract has a few additional features:

- strong security layer (see below)
- public land buying in ETH for the initial land sale (see below)
- flash sales (see below)
- batch minting of lands (for contract owners)
- kyc check from another contract (for potential future use)
- external minting from another contract (for future minting contract accepting other crypto-currencies)

## Security

The contract has a strong security layer : all owner functions are protected by a 2-step or 3-step lock process.

For all owner functions, the contract lock must first be disengaged by one or the two other owners.

To properly disengage the lock, the following data needs to be provided:
- id of the function to be executed
- additional key (chosen and shared between the owners just before the lock disengagement)
- function target (wallet or contract address for the functions that needs a target)

Once the lock is disengaged (by one or two owners depending on the function), the function can be executed by another owner.

If one of the following conditions are met, the lock is automatically re-engaged:
- the lock is disengaged by one owner and the same owner tries to execute the function
- the data provided for the lock disengagement does not match the data sent for the function execution
- the lock was disengaged for a too long time before the function execution

All this means that the contract is protected against:
- if an owner key is stolen, the attacker cannot execute any function without the other owners
- if two owner keys are stolen, the attacker cannot execute most of the functions
- a single owner trying to execute a function without the other owners knowing

All owners functions are protected by the 3-way process, except for the following functions:
- toggleZonePublicSale (2-way process, this function is used to enable/disable the public sale of lands for a given zone)
- setTokenURI (any owner can set the tokenURI)

Good luck trying to hack this contract ;)

## Public sale

The contract has a public sale feature, which allows anyone to buy a land in ETH.

Owners can enable/disable the public sale for a given zone and price.

The price is set in ETH and is the same for all lands in a given zone but different for each zone.

## Flash sales

The contract has a flash sale feature, which allows anyone to buy a land in ETH at a discounted price for a limited time.

Owners can enable/disable the flash sale for a given zone and price.

The price is set in ETH and is the same for all lands in a given zone but different for each zone.

A flash sale has a limited number of lands that can be sold.