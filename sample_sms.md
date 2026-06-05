# Iranian Banking SMS Parsing Guide

## General Instructions

When parsing Persian banking SMS messages:

1. Detect the bank name first.
2. Extract structured financial data.
3. Ignore decorative text and line breaks.
4. Amounts may contain commas and should be converted to integers.
5. Persian and English digits should both be supported.
6. If a field is not present, return null.
7. Determine transaction type from keywords.

---

# Bank Mellat

## Sample SMS

```
حساب1912993206
برداشت28,083,000
مانده21,571,808
05/03/14-20:20
```

## Expected Extraction

```json
{
  "bank": "Mellat",
  "transaction_type": "withdrawal",
  "account_number": "1912993206",
  "amount": 28083000,
  "balance": 21571808,
  "date": "1405/03/14",
  "time": "20:20"
}
```

## Detection Rules

### Account Number

Look for:

```
حساب
```

The numeric value immediately after it is the account number.

### Withdrawal Amount

Look for:

```
برداشت
```

The numeric value after this keyword is the transaction amount.

### Balance

Look for:

```
مانده
```

The numeric value after this keyword is the remaining balance.

### Date & Time

Pattern:

```
DD/MM/YY-HH:MM
```

Example:

```
05/03/14-20:20
```

Interpret as:

```
1405/03/14
20:20
```

### Transaction Type

Keyword mapping:

```
برداشت => withdrawal
واریز => deposit
```

---

# Bank Saman

## Sample SMS

```
بانك سامان
برداشت مبلغ 54,000,000 خريدکالا
از 807-700-2115860-1
مانده 2,928,921,349
1405/2/20
00:25:44
```

## Expected Extraction

```json
{
  "bank": "Saman",
  "transaction_type": "purchase",
  "amount": 54000000,
  "account_number": "807-700-2115860-1",
  "balance": 2928921349,
  "date": "1405/02/20",
  "time": "00:25:44",
  "description": "خریدکالا"
}
```

## Detection Rules

### Bank Detection

Look for:

```
بانک سامان
بانك سامان
```

### Amount

Pattern:

```
برداشت مبلغ <amount>
```

### Purchase Detection

If SMS contains:

```
خريدکالا
خریدکالا
```

then:

```json
"transaction_type": "purchase"
```

instead of generic withdrawal.

### Source Account

Look for:

```
از
```

and extract the following account number.

### Balance

Look for:

```
مانده
```

### Date

Pattern:

```
YYYY/MM/DD
```

### Time

Pattern:

```
HH:MM:SS
```

---

# Blu Bank

## Sample SMS

```
بلو
بفرمایید رمز پویا
انتقال به
610433*9199
مبلغ: 500,000 ریال
رمز: 374475
14:42
```

## Expected Extraction

```json
{
  "bank": "BluBank",
  "message_type": "otp",
  "operation": "transfer",
  "destination_card": "610433*9199",
  "amount": 500000,
  "otp_code": "374475",
  "time": "14:42"
}
```

## Detection Rules

### Bank Detection

Look for:

```
بلو
بلوبانک
بلو بانک
```

### Message Type

If SMS contains:

```
رمز پویا
رمز:
```

then:

```json
"message_type": "otp"
```

### Transfer Operation

If SMS contains:

```
انتقال به
```

then:

```json
"operation": "transfer"
```

### Destination Card

Extract the value after:

```
انتقال به
```

Example:

```
610433*9199
```

### Amount

Pattern:

```
مبلغ:
```

Amount may optionally end with:

```
ریال
تومان
```

### OTP

Pattern:

```
رمز:
```

Extract the numeric code.

### Time

Pattern:

```
HH:MM
```

---

# Universal Keyword Dictionary

## Transaction Types

```json
{
  "برداشت": "withdrawal",
  "واریز": "deposit",
  "خرید": "purchase",
  "خريد": "purchase",
  "انتقال": "transfer"
}
```

## Common Financial Fields

```json
{
  "مانده": "balance",
  "مبلغ": "amount",
  "حساب": "account_number",
  "رمز": "otp_code"
}
```

## Output Format

Always return:

```json
{
  "bank": null,
  "transaction_type": null,
  "account_number": null,
  "card_number": null,
  "destination_card": null,
  "amount": null,
  "balance": null,
  "date": null,
  "time": null,
  "description": null,
  "otp_code": null,
  "message_type": null
}
```

Only populate fields that exist in the SMS.

```
```
