# Data

The original dataset is **not included** in this repository (provided as part of a
data challenge). This file documents its schema so the analysis can be understood
and, in principle, reproduced on a comparable dataset.

## Overview

- **Rows:** 98,645 checkout events
- **Target:** binary fraud flag — `fraud`
- **Fraud rate:** 0.56%

## Schema

| column                        | type      | description                                                        |
|-------------------------------|-----------|--------------------------------------------------------------------|
| `payment_id`                  | string    | unique identifier of the checkout / payment event                  |
| `created_date`                | datetime  | timestamp of the checkout event                                    |
| `week`                        | date      | week bucket derived from `created_date`                            |
| `amount_eur`                  | float     | transaction amount, in EUR                                         |
| `shop_name`                   | string    | merchant / shop identifier                                         |
| `merchant_category_code`      | string    | merchant category code (MCC)                                       |
| `customer_id`                 | string    | unique customer identifier                                         |
| `residence_region`            | string    | customer residence region                                          |
| `residence_country`           | string    | customer residence country                                         |
| `national_bank_code`          | string    | national bank code of the customer's bank                          |
| `bank_name`                   | string    | name of the customer's bank                                        |
| `iban_country_code`           | string    | country code of the customer's IBAN                                |
| `device_os`                   | string    | operating system of the device used (e.g. iOS, Android)           |
| `first_top_up_date`           | datetime  | date of the customer's first wallet top-up                         |
| `days_since_first_payment`    | int       | days between first payment and current event                       |
| `o_business_account_lifetime` | int       | account lifetime / age of the account (days)                       |
| `nr_to_business_count_90d`    | int       | number of transactions in the last 90 days                         |
| `last_payment_date`           | datetime  | date of the customer's most recent payment                         |
| `fraud`                       | int       | fraud flag (1 = fraud, 0 = legitimate)                             |
| `ys_since_wallet_activation`  | float     | years since wallet activation                                      |
| `payment_status`              | string    | status of the payment (e.g. DONE)                                  |

## Notes

- Column names are reproduced as they appear in the source data; some are truncated
  or abbreviated.
- Account age (`o_business_account_lifetime`) is the single strongest signal in the
  analysis: median ~31 days for fraud vs. ~1,265 days for legitimate events.
- To run the notebook, place the dataset under `data/` and update the load path if
  required.
