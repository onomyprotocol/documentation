---
description: BondingNOM.sol
---

# BCO API

_All function calls are currently implemented without side effects. Please refer to the "_[_Bonding Curve Offering \(BCO\)_](bonding-curve-offering.md)_" section for more details on the BCO and wNOM vs NOM._

## _getNOMAddr\(\)_ public view returns \(address NOMContAddr\)

| Input/Output | Data Type | Variable Name | Comment |
| :--- | :--- | :--- | :--- |
| output | address | NomContAddr | Return the wNOM Token Contract address |

## _getSupplyNOM\(\)_ public view returns \(uint256 supplyNOM\)

| Input/Output | Data Type | Variable Name | Comment |
| :--- | :--- | :--- | :--- |
| output | uint256 | supplyNOM | Return the wNOM Token circulate supply |

## _getBondPrice\(\)_ public view returns \(uint256 priceNOM\)

| Input/Output | Data Type | Variable Name | Comment |
| :--- | :--- | :--- | :--- |
| output | uint256 | priceNOM | Return the price based on the current wNOM supply |

## _tokToF64\(uint256 utoken\)_ public view returns\(int128 f64Token\)

### We will use`ABDKMath64x64.divu` module from `abdk-libraries-solidity` library

| Input/Output | Data Type | Variable Name | Comment |
| :--- | :--- | :--- | :--- |
| input | uint256 | utoken | uint256 token amount |
| output | int128 | f64Token | Return token amount to F64\(Fixed Point 64\) format |

## _f64ToTok\(int128 fixed64\)_ public view returns\(uint256 uToken\)

### We will use `ABDKMath64x64.mulu` module from `abdk-libraries-solidity` library

| Input/Output | Data Type | Variable Name | Comment |
| :--- | :--- | :--- | :--- |
| input | int128 | f64Token | f64 token amount |
| output | uint256 | uToken | Return F64\(Fixed Point 64\) token amount to uint256 |

## _priceAtSupply\(uint256 supplyNOM\)_ public view returns\(uint256 priceNOM\)

### By consuming this function, everyone can predict the exact price base on the NOM token supply

### Formula: `ETH/NOM = pow(_supplyNOM/a, 2)`

| Input/Output | Data Type | Variable Name | Comment |
| :--- | :--- | :--- | :--- |
| input | uint256 | supplyNOM | token supply amount in uint256 |
| output | uint256 | priceNOM | Return the token price base on the input supply amount |

## _supplyAtPrice\(uint256 priceNOM\)_ public view returns \(uint256 suppliedNOM\)

### Using this function, everyone can predict the exact token supply base on token price

### Formula: `SuppliedNom = sqrt(ETH/NOM) * a`

| Input/Output | Data Type | Variable Name | Comment |
| :--- | :--- | :--- | :--- |
| input | uint256 | priceNOM | F64\(Fixed Point 64\) formated amount |
| output | uint256 | suppliedNOM | Return token supply amount for input price |

## _NOMSupToETH\(uint256 supplyTop, uint256 supplyBottom\)_ public view returns\(uint256 amountETH\)

### Integrate over a curve to get the amount of ETH needed to buy the amount of NOM

### Formula: `ETH = a/3((SupplyNOM_Top/a)^3 - (SupplyNOM_Bottom/a)^3)`

| Input/Output | Data Type | Variable Name | Comment |
| :--- | :--- | :--- | :--- |
| input | uint256 | supplyTop | The amount of the wNOM supply top |
| input | uint256 | supplyBottom | The amount of the wNOM supply bottom |
| output | uint256 | amountETH | Return wNOM supply range to ETH |

## _buyQuoteNOM\(uint256 amountNOM\)_ public view returns\(uint256 amountETH\)

* **Determine supply range based on the spread and current curve price based on supplyNOM** 
* **Integrate over the curve to get the amount of ETH needed to buy the amount of NOM**

| Input/Output | Data Type | Variable Name | Comment |
| :--- | :--- | :--- | :--- |
| input | uint256 | amountNOM | amount of wNOM to be purchased in 18 decimal |
| output | uint256 | amountETH | Return quote for a particular amount of wNOM \(Dec 18\) in ETH \(Dec 18\) |

## _cubrt_ _\(int128 x\)_ public pure returns \(int128 cX\)

| Input/Output | Data Type | Variable Name | Comment |
| :--- | :--- | :--- | :--- |
| input | int128 | x | signed 64.64-bit fixed-point number |
| output | int128 | cX | Return cubrt\(x\) rounding down, where x is int128 integer |

## _cubrtu_ _\(uint256 x\)_ public pure returns \(uint256\)

| Input/Output | Data Type | Variable Name | Comment |
| :--- | :--- | :--- | :--- |
| input | uint256 | x | unsigned 256-bit integer number |
| output | uint256c | cX | Return cubrtu\(x\) rounding down, where x is unsigned 256-bit integer |

## _buyQuoteETH\(uint256 amountETH\)_ public view returns\(uint256 amountNOM\)

* **Determine supply bottom**
* **Integrate over the curve, and solve for supply top `supplyNOM_Top = a(3ETH/a + (supplyNOM_Bot/a)^3)^(1/3)`**
* **Subtract supply bottom from top to get NOM for ETH**

| Input/Output | Data Type | Variable Name | Comment |
| :--- | :--- | :--- | :--- |
| input | uint256 | amountETH | amount of ETH to buy wNOM |
| output | uint256 | amountNOM | Buy Quote for the purchase of wNOM based on the amount of ETH \(Dec 18\) |

## _buyNOM_\(\) public payable

### **Buy NOM token with ETH**

## _sellQuoteNOM\(uint256 amountNOM\)_ public view returns\(uint256 amountETH\)

* **Determine supply top: `priceBondCurve - 1% = Top Sale Price`**
* **Integrate over the curve to find ETH: `ETH = a/3((supplyNOM_Top/a)^3 - (supplyNOM_Bot/a)^3)`**
* **Subtract supply bottom from top to get NOM for ETH**

| Input/Output | Data Type | Variable Name | Comment |
| :--- | :--- | :--- | :--- |
| input | uint256 | amountNOM | amount of wNOM |
| output | uint256 | amountETH | Return Sell Quote: wNOM for ETH \(Dec 18\) |

## _sellNOM\(uint256 amountNOM\)_ public

* **Determine supply top: `priceBondCurve - 1% = Top Sale Price`**
* **Integrate over the curve to find ETH: `ETH = a/3((supplyNOM_Top/a)^3 - (supplyNOM_Bot/a)^3)`**
* **Subtract supply bottom from top to get NOM for ETH**

| Input/Output | Data Type | Variable Name | Comment |
| :--- | :--- | :--- | :--- |
| input | uint256 | amountNOM | amount of wNOM |

## _teamBalance\(\)_ public returns\(uint256 teamBalance\)

* **Calculate the amount of ETH to cover all current NOM outstanding based on bonding curve integration**
* **Subtraction locked ETH from Contract Balance to get amount available for withdrawal**

| Input/Output | Data Type | Variable Name | Comment |
| :--- | :--- | :--- | :--- |
| output | uint256 | teamBalance | ETH balance of the team |

## _withdraw\(\)_ public onlyOwner returns\(bool success\)

* **Calculate the amount of ETH to cover all current NOM outstanding based on bonding curve integration**
* **Subtraction lockedETH from Contract Balance to get amount available for withdrawal.**

| Input/Output | Data Type | Variable Name | Comment |
| :--- | :--- | :--- | :--- |
| output | bool | success | Transfer ETH to the Owner |

