# Salesforce Regex Cheat Sheet

I believe that RegEx functions are one of the most undervalued features in Salesforce. While common amongst developers, it is less popular for functional folks. One reason for that could be the cryptic nature of RegEx formulas, which might scare people away. I am hoping that this cheat sheet will help to use RegEx formulas in your day to day activies and allow you to build more robust validations.

# What is RegEx

A regular expresion (RegEx in short), is a sequence of characters that helps search engines to find certain string patterns. In Salesforce, admins can take advantage of these string patterns for input validations. A common use case here is to only allow postal codes to be entered in a certain pattern like 12-311 (##-###). There are many other use cases like credit card or phone number validations for example.

# RegEx Testing

You can test your RegEx formulas via websites for example: [RegExTesting](https://www.regextester.com)

# <img src="img/postalCode.png" alt="calendar icon" width="50"/> Postal Code Validation

## Country Validations

[Source](https://stackoverflow.com/questions/7926687/regular-expression-german-zip-codes)

<details>
  <summary>Germany</summary>

```javascript
\b((?:0[1-46-9]\d{3})|(?:[1-357-9]\d{4})|(?:[4][0-24-9]\d{3})|(?:[6][013-9]\d{3}))\b
```

</details>

<details>
  <summary>United Kingdom</summary>

```javascript
^(GIR|[A-Z]\d[A-Z\d]??|[A-Z]{2}\d[A-Z\d]??)[ ]??(\d[A-Z]{2})$
```

</details>

<details>
  <summary>United States</summary>

```javascript
^\d{5}([\-]?\d{4})?$
```

</details>

<details>
  <summary>Canada</summary>

```javascript
^([ABCEGHJKLMNPRSTVXY]\d[ABCEGHJKLMNPRSTVWXYZ])\ {0,1}(\d[ABCEGHJKLMNPRSTVWXYZ]\d)$
```

</details>

<details>
  <summary>France</summary>

```javascript
^(F-)?((2[A|B])|[0-9]{2})[0-9]{3}$
```

</details>

<details>
  <summary>Italy</summary>

```javascript
^(V-|I-)?[0-9]{5}$
```

</details>

<details>
  <summary>Austria</summary>

```javascript
^(0[289][0-9]{2})|([1345689][0-9]{3})|(2[0-8][0-9]{2})|(290[0-9])|(291[0-4])|(7[0-4][0-9]{2})|(7[8-9][0-9]{2})$
```

</details>

<details>
  <summary>Netherlands</summary>

```javascript
^[1-9][0-9]{3}\s?([a-zA-Z]{2})?$
```

</details>

<details>
  <summary>Spain</summary>

```javascript
^([1-9]{2}|[0-9][1-9]|[1-9][0-9])[0-9]{3}$
```

</details>

<details>
  <summary>Denmark</summary>

```javascript
^([D-d][K-k])?( |-)?[1-9]{1}[0-9]{3}$
```

</details>

<details>
  <summary>Sweden</summary>

```javascript
^(s-|S-){0,1}[0-9]{3}\s?[0-9]{2}$
```

</details>

<details>
  <summary>Belgium</summary>

```javascript
^[1-9]{1}[0-9]{3}$
```

</details>

## General Zip Code Validations

[Source](http://regexlib.com/Search.aspx?k=zip&c=-1&m=-1&ps=20)

The below expression checks for 5 numeric values, followed by a hyphen and 4 additional numeric values. Example: 22223-3443.

<details>
  <summary>Zip+4</summary>

```javascript
^\d{5}-\d{4}$
```

</details>

The below expression works the same way as the Zip+4 function above but disallows zeros. Example of a disallowed value: 00000-0000.

<details>
  <summary>Zip+4 without 0 </summary>

```javascript
^(?(^00000(|-0000))|(\d{5}(|-\d{4})))$
```

</details>

# <img src="img/creditCard.png" alt="calendar icon" width="50"/> Credit Card Validations

[Source](https://www.validcreditcardnumber.com/)

-   Visa : 13 or 16 digits, starting with 4.
-   MasterCard : 16 digits, starting with 51 through 55.
-   American Express : 15 digits, starting with 34 or 37.
-   Discover : 16 digits, starting with 6011 or 65.
-   JCB : 15 digits, starting with 2131 or 1800, or 16 digits starting with 35.

---

**Note**

The below regex formulas assume the credit card numbers without any special characters like hyphens (-).
Example: 4111 1111 1111 1111 instead of 4111-1111-1111-1111

---

<details>
  <summary>Visa</summary>

```javascript
^4[0-9]{12}(?:[0-9]{3})?$
```

</details>

<details>
  <summary>MasterCard</summary>

```javascript
^(?:5[1-5][0-9]{2}|222[1-9]|22[3-9][0-9]|2[3-6][0-9]{2}|27[01][0-9]|2720)[0-9]{12}$
```

</details>

<details>
  <summary>American Express</summary>

```javascript
^3[47][0-9]{13}$
```

</details>

<details>
  <summary>Discover</summary>

```javascript
^6(?:011|5[0-9]{2})[0-9]{12}$
```

</details>

<details>
  <summary>JCB</summary>

```javascript
^(?:2131|1800|35\d{3})\d{11}$
```

</details>

# <img src="img/calendar.png" alt="calendar icon" width="50"/> Date Validations

<details>
  <summary>EU Date Notation dd/mm/yy or d/m/y or dd/mm/yyy </summary>
	
This expression validates dates in the ITALIAN d/m/y format from 1/1/1600 - 31/12/9999. The days are validated for the given month and year. Leap years are validated for all 4 digits years from 1600-9999, and all 2 digits years except 00 since it could be any century (1900, 2000, 2100). Days and months must be 1 or 2 digits and may have leading zeros. Years must be 2 or 4 digit years. 4 digit years must be between 1600 and 9999. Date separator may be a slash (/), dash (-), or period (.)

```diff
+ Allowed Values: 29/02/1972 | 5-9-98 | 10-11-2002
- Non-Allowed Values: 29/02/2003 (non leap year) | 12/13/2002 | 1-1-1500
```

```javascript
^(?:(?:31(\/|-|\.)(?:0?[13578]|1[02]))\1|(?:(?:29|30)(\/|-|\.)(?:0?[1,3-9]|1[0-2])\2))(?:(?:1[6-9]|[2-9]\d)?\d{2})$|^(?:29(\/|-|\.)0?2\3(?:(?:(?:1[6-9]|[2-9]\d)?(?:0[48]|[2468][048]|[13579][26])|(?:(?:16|[2468][048]|[3579][26])00))))$|^(?:0?[1-9]|1\d|2[0-8])(\/|-|\.)(?:(?:0?[1-9])|(?:1[0-2]))\4(?:(?:1[6-9]|[2-9]\d)?\d{2})$
```

</details>

<details>
  <summary>US Date Notation mm/dd/yy or m/d/y or dd/mm/yyy </summary>
	
MM/dd/yyyy with leap years. Valid since year 1900. MM and DD could have 1 or 2 digits : M/d/yyyy or MM/d/yyyy or M/dd/yyy

```diff
+ Allowed Values: 01/31/1905 | 1/9/1900 | 2/29/1904
- Non-Allowed Values: 31/01/2005 | 02/29/2005 | 2/29/2005
```

```javascript
^(?:(?:31(\/|-|\.)(?:0?[13578]|1[02]))\1|(?:(?:29|30)(\/|-|\.)(?:0?[1,3-9]|1[0-2])\2))(?:(?:1[6-9]|[2-9]\d)?\d{2})$|^(?:29(\/|-|\.)0?2\3(?:(?:(?:1[6-9]|[2-9]\d)?(?:0[48]|[2468][048]|[13579][26])|(?:(?:16|[2468][048]|[3579][26])00))))$|^(?:0?[1-9]|1\d|2[0-8])(\/|-|\.)(?:(?:0?[1-9])|(?:1[0-2]))\4(?:(?:1[6-9]|[2-9]\d)?\d{2})$
```

</details>
