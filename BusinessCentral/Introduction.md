# Gaining Familiarity

1. **Business Central Online Architecture**
    - [link here](https://learn.microsoft.com/en-us/dynamics365/business-central/dev-itpro/administration/tenant-environment-topology)    
2. **Pricing**
    - [link here](https://dynamics.microsoft.com/en-us/business-central/pricing/) ...  
    - Essentials is what is typically needed (Premium is when you also have manufacturing)    
3. **Managing User Permissions**
    - [link here](https://www.youtube.com/watch?v=tMXqcoBXwUk)    
4. **Creating New Companies / Deleting old ones** 
    - In Business Central search for Companies and then in that list add or delete companies. You can only do this if you are assigned the SUPER permission set. 
    - An alternative way of creating companies is ```My Settings->3 dot ellipsis next to Company->3 dot Ellipsis on Top-> New```    
5. **Setting up Number Series (for invoices etc.)** 
    - in Business Central search for `No. series`    
    - remember to check boxes that allow `Default No.s`, `Manual No.s` and `Allow Gaps in No.s`    
6. **VAT issues in posting AP/AR**    
    - Business Central expects each GL Account or Item on which a purchase or sales invoice is issued to have their own specific tax setup and VAT setup
    - So you need to setup VAT at the account/item level as well as the customer / vendor level    
    - After that yo need to do the Tax Setup (search for Tax Setup)
7. **Table sizes (Object specifications and limitations)**
    - [link here](https://learn.microsoft.com/en-us/dynamics365/business-central/dev-itpro/developer/devenv-object-specifications-limitations)
    - Tables have some restrictions on the fields, but table sizes can be large
8. **Considerations for when to create new environments / tenants**
    - [link here](https://learn.microsoft.com/en-us/power-platform/admin/multiple-online-environments-tenants)
9. **Video on APIs**
    - [how to get API authentication on Business Central](https://youtu.be/zxDa222uXeQ?feature=shared)
    - [how to call an API](https://youtu.be/r7MBwAnt4z0?feature=shared)
    - [Erik's video on API Authentication](https://www.youtube.com/watch?v=B1rxyqR2ZCY)
10. **Remember to login as the Role of Accountant**

# Intercompany
- **Why use Intercompany**    
    You can consolidate:
    - Across companies that have different charts of accounts. 
    - Companies that use different fiscal years and different currencies.
    - Either the full amount or a percentage of a company's financial information.
    - Using different currency exchange rates in individual G/L accounts.
    - Companies in other accounting and business management programs.
    - Companies in different Business Central environments.

- **Setting up intercompany**
    - [link here](https://learn.microsoft.com/en-us/dynamics365/business-central/intercompany-how-setup)    
- **Setting up Intercompany chart of accounts**
    - [link here](https://learn.microsoft.com/en-us/dynamics365/business-central/intercompany-how-setup#set-up-the-intercompany-charts-of-accounts)
- **Consolidating data from multiple companies**    
    - [link here](https://learn.microsoft.com/en-us/dynamics365/business-central/finance-consolidated-company-reporting)
- **Entering Transactions between companies**     
    - [link here](https://learn.microsoft.com/en-us/dynamics365/business-central/intercompany-manage)    
- A new GL account for incoming transactions from group company is needed
    - and you can then transfer it to the regular GL
    - as any of the existing Chart of Accounts cannot be input from what we can make out     



