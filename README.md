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

## Algo 2-2020

# Base-62-AlphaNumeric

Converts numbers to base 63, compressing integers to alphanumeric strings

## Algorithm

For base 10, values include 0-9. When a value exceeding 9 increases a leading digit starting with 1. This implies a leading digit of 0 at all times, which is omitted for lacking significance. When surpasing 9, that leading 0 is incremented by 1, attributing to it signficance. With arithmatic in base 10, the number 10 is 10 remainder 0. If we were to add 5 and 6, we'd have 10 remainder 1, or 11 stacked.


For base 62, values include 0-9:A-Z:a-z, incrementing from 0 to lowercase z. Once z is surpased, a leading character is incremented to 1, and the preceding z character resets to 0. This implies leading characters of 0 at all times, omitted for lacking significance. Converting base 62 value `"10"` is value `62` in base 10. 