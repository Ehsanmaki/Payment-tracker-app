

---

# Main Features

## 1. Automatic Bank SMS Detection

The application should:

* Monitor incoming bank transaction SMS messages.
* Support multiple Iranian banks.
* Allow me to provide sample SMS messages so the system can learn and identify each bank's message format.
* Extract transaction information automatically.

Required extracted fields:

* Transaction date
* Transaction amount
* Bank name
* Any available transaction metadata

The extracted information should be prepared as a draft transaction awaiting confirmation.

---

## 2. Manual Transaction Completion

After detecting a payment:

The application should show a pending transaction card containing:

* Date
* Amount
* Bank

Then I should enter:

* Title (عنوان)
* Category (دسته بندی)

Once confirmed, the transaction should be saved.

---

## 3. Smart Category Suggestions

The application should learn from previous records.

Example:

If I previously recorded:

Title: Snapp
Category: Transportation

Then whenever a new transaction with title "Snapp" is entered, the application should automatically suggest the same category.

The user can always change the suggestion manually.

---

## 4. Expense Analytics

The application should provide:

* Monthly spending analysis
* Category breakdowns
* Budget usage
* Spending trends
* Interactive charts
* Remaining budget calculations
* Bank account allocation reports

Charts should be optimized for Persian users and support the Shamsi (Jalali) calendar.

---

## 5. Google Sign-In

I want to sign in using my Google account.

Preferred features:

* Google Sign-In
* Cloud backup
* Multi-device synchronization

---

## 6. Google Sheets Integration

If technically possible:

* Every transaction should automatically sync with a Google Sheet.
* Changes in the application should update the Google Sheet.
* The Google Sheet can act as a backup and reporting source.

Please suggest the best architecture for implementing this.

---

# Language and Localization

Requirements:

* Full Persian UI
* RTL layout
* Persian numbers where appropriate
* Shamsi (Jalali) calendar support
* Iranian Rial currency support

---

# Excel File Structure

The Excel file contains:

1. A sheet named "paid"
2. Monthly sheets such as:

   * Farvardin 1405
   * Ordibehesht 1405
   * Dey 1405
   * etc.

To reduce processing costs and token usage, only analyze:

* Sheet: "paid"
* Columns related to "فروردین 1405"
* Sheet: "Farvardin 1405"

Ignore all other monthly sheets.

The structure of other months follows the same pattern.

---

# Paid Sheet Structure

Under each month section (for example: "فروردین 1405") there are these columns:

## تاریخ

Represents the day number of the month.

Examples:

* 1
* 2
* 15

This value may be extracted from SMS messages.

---

## مبلغ ریال

Amount paid in Iranian Rial.


This value should be extracted automatically from SMS messages.

---

## عنوان

Title or description of the purchase.

This field is entered manually by the user.

---

## دسته بندی

Expense category.

Examples:

* روزمره
* تفریح
* حمل و نقل
* سلامت
* ماشین
* خرید لباس نو
* مسافرت
* سرمایه گزاری
* پس‌انداز
* قسط
* دیگر موارد



The application should:

* Read all existing categories.
* Show them as selectable options.
* Allow category management.(add-remove-rename-reorder, etc)
* Suggest categories automatically based on previous usage.

---

## بانک

Bank account used for the payment.

This should be extracted automatically from the SMS whenever possible.

---

# Monthly Sheet Structure (Example: Farvardin 1405)

The sheet contains budget planning and spending tracking.

## B4:B14 — Category List

Represents expense categories/subcategories.

The application should allow:

* Add category
* Edit category
* Remove category

---

## A4:A14 — Income Allocation Percentage

Represents the percentage of total income allocated to each category.

The application should allow management of these values.

---

## C4:C14 — Assigned Bank

Represents the bank account allocated to each category budget.

The application should allow management of these assignments.

---

## D4:D14 — Payable Percentage (درصد قابل پرداخت)

Represents the percentage of income assigned to the category.

This value can be:

* Static amount
* Percentage-based

The user should be able to choose either mode.

---

## F4:F14 — Payable Amount (مبلغ قابل پرداخت)

Represents the budget amount allocated to the category.

Calculated from income and allocation settings.

---

## G4:G14 — Spent Amount (مبلغ خرج شده)

Represents the total amount spent within the category.

This should be calculated automatically from transaction records stored in the Paid sheet.

---

## E4:E14 — Spent Percentage (درصد خرج شده)

Represents the percentage of allocated budget already consumed.

Calculated automatically.

---

## H4:H14 — Remaining Amount (مبلغ باقیمانده)

Represents the remaining budget available for spending.

Calculated automatically.

---

## I4:I14 — Remaining Percentage (درصد باقیمانده)

Represents the percentage of remaining budget.

Calculated automatically.

---

## Totals

There are summary cells at the bottom of each column.

The application should reproduce the same calculations and summaries automatically.

---

# Technical Expectations

Please recommend the best architecture for:

* Android development
* SMS parsing
* Local database
* Google Sign-In
* Google Sheets synchronization
* Cloud backup
* Offline-first support
* Data security
* Persian localization
* Future scalability

Before writing code, analyze the Excel file structure and ask any missing questions required for a robust implementation.
