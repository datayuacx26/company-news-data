---
schema_version: "1.0.0"
document_id: "3baaacb0e2195bc7729de1a58f3661492251b488fe9152f5fd34fb763f12c7eb"
company_key: "yc-finta"
company: "Finta"
source_id: "yc-finta-news-import-2ae7bbbcba74"
canonical_url: "https://www.finta.io/blog/how-to-track-your-net-worth-in-notion"
published_at: null
first_seen_at: "2026-07-25T04:54:13.676178+00:00"
fetched_at: "2026-07-28T21:36:14.883471+00:00"
content_hash: "sha256:1d7d94f59a033901707f60ac2511ff0d93a91d82d889d0b798176372540e5ab4"
---

# How to Track your Net Worth in Notion

## What is Net Worth and Why Does it Matter?


Net worth is a measure of your financial health. Essentially, it's the difference between what you own (your assets) and what you owe (your liabilities). It's what you would have left over if you sold everything you own and paid off all your debts.


Knowing your net worth is important for several reasons. Most importantly, it gives you a comprehensive picture of your financial situation. This can help you make better decisions about how to manage your money and plan for your future.


## How to Calculate Your Net Worth in Notion


To calculate your net worth, you need to add up the value of all your assets and subtract the value of all your liabilities. Assets can include things like your home's value, car, investments, and savings accounts. Liabilities can include things like your mortgage, car loan, student loans, and credit card debt.


### Account Balance Data


You could manually check all of your bank accounts and enter their balances into Notion, or you can use Finta to sync this data automagically every day. To get started:


1. [Sign up](https://finta.link/signup) - try us out for 7 days no credit card needed
2. Connect your bank accounts - We partner with Plaid to securely connect to your accounts.
3. Create a destination - Copy one of our many Notion budgeting templates


### Account Table Formulas


Next, let's add two helpful formulas to the accounts table.


First, create a new formula column "Is Asset?" with the following formula:


```text
or(prop("Type") == "depository", prop("Type") == "investment")
```


This field will be checked for all asset accounts and unchecked for all liability accounts


Next, create another formula column "Net Worth" with the following formula:


```text
if(prop("Is Asset?"), prop("Current"), -prop("Current"))
```


This field will return the account balance as a positive number if the account is an asset and a negative number if the account is a liability.


Finally, sum the "Net Worth" column to calculate your total net worth!


## Visualize Your Net Worth in Notion


In addition to tracking your net worth, it can also be helpful to visualize your account balances. Although Notion doesn't have a built-in chart feature, you can use Finta to create and embed charts right within your doc. published


## Tracking Net Worth Over time


Once you have calculated your net worth, you can track it over time to see how your financial situation is changing. For example, during your monthly financial review, jot down the total value of your assets, your total liabilities, and your calculated net worth. After awhile, you’ll be able to see how these numbers change over time.


Ideally, you want your net worth to increase, which means you are building wealth and becoming more financially secure.


Overall, knowing your net worth is an important part of maintaining good financial health. By understanding your assets and liabilities and tracking your net worth over time, you can make informed decisions about your finances and work toward your long-term financial goals.
