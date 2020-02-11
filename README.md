---
author: Edson Ayllon
category: algorithm
tags: 
- mathematics
- cryptography
- compression
status: started
twitter: https://twitter.com/relativeread
---

## Algo 3-2020

# Base-62-AlphaNumeric

Converts numbers to base 63, compressing integers to alphanumeric strings

## Contents

- [Section 1—Significance](#1-significance)
- [Section 2—Algorithm](#2-algorithm)

## 1. Significance

Significantly reduces the required storage for very large unsigned integers.

## 2. Algorithm

### 2.1 Definition

For base 10, values include 0-9. When a value exceeding 9 increases a leading digit starting with 1. This implies a leading digit of 0 at all times, which is omitted for lacking significance. When surpasing 9, that leading 0 is incremented by 1, attributing to it signficance. With arithmatic in base 10, the number 10 is 10 remainder 0. If we were to add 5 and 6, we'd have 10 remainder 1, or 11 stacked.

For base 62, values include 0-9:A-Z:a-z, incrementing from 0 to lowercase z. Once z is surpased, a leading character is incremented to 1, and the preceding z character resets to 0. This implies leading characters of 0 at all times, omitted for lacking significance. Converting base 62 value `"10"` is value `62` in base 10, and base 62 value `"z"` is `61` in base 10. 

### 2.2 Optimization

To keep the time complexity low, to keep speed of execution high, the algorithm was targeted to O(1) or O(n) complexity. 

To keep time complexity within O(n), one for-loop was created per block of execution, with one iteration per character in the resulting base 62 string. 

### 2.3 Encoding

For base 10, possible numbers for 1 digit are 0-9, or 10^1. For 2 digits in base 10, each leading number from 0-9 has a possible trailing number of 0-9, creating a 2 dimensional array of possible numbers. Base 10 possible numbers for 2 digits are 0-99, or 10^2. As digits increase, so does the dimensions of the array containing the total possible values. For n digits, the total possible values are 10^n for base 10. 

For base 62, possible values for 1 character are 0-z, or 62^1. For 2 characters, total values goes from 0-zz, or 62^2 values. For n digits, total possible values are 62^n. So for a given input value in base 10, the digits in base 62 are calculated as follows:

> ![](https://latex2image.joeraut.com/output/img-75e0ba78e2e22ac2.png)

> ![](https://latex2image.joeraut.com/output/img-14e648b0e17eb6d4.png)

> ![](https://latex2image.joeraut.com/output/img-4fdc95749f970741.png)

- where `m` is the number in base 10
- where `n` is the number of digits in base 62

That number is truncated, reduced to the lowest whole integer, and incremented by 1. For numbers below 62, there should be at least 1 iteration, and should increment to 2 iterations passing a magnituded of 62 in base 62, etc. 

```
totalDigits = int(math.log(base10)/math.log(62))+1
```

Values for a 62 digit string are held in an array from 0-z.

```
alphanum = [
  "0","1","2","3","4","5","6","7","8","9",
  "A","B","C","D","E","F","G","H","I","J","K","L","M","N","O","P","Q","R","S","T","U","V","W","X","Y","Z",
  "a","b","c","d","e","f","g","h","i","j","k","l","m","n","o","p","q","r","s","t","u","v","w","x","y","z"
]
```

For the lowest remaining digit, the furthest remaining digit from the leading digit, the result will be the remainder from division of 62, or modulus 62. This remainder maps to a base 62 value when used as the index to the array `alphanum`. This base 62 digit is saved to our collection of base 62 digits for the given encoding. The qoutient, by Euclidean division, is saved as the new number to be divided by 62 in the next iteration. In the next iteration, the previous quoutient is divided by 62, creating a new remainder to be mapped to a base 62 value, and a new qoutient to be passed to the next iteration. This continues until the total number of digits for a given base 62 value conversion are done.

```
base62digits = []
num = base10
for x in range(totalDigits):
  base62digits.append(alphanum[num%62])
  num, remainder = divmod(num, 62)
  base62digits.reverse()
```
