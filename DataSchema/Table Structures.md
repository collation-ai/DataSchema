A typical setup in Collation (in order of hierarchy) is as follows:

## Home Office
This is needed in cases where the client is a firm that has acquired other wealth managers over time. This table is redundant for SFO/MFO use cases.    

#### table name: ```ct_home_office```
|Column |Data Type |Settings |References |Nullable |Sample value |Description |
|----|----|----|----|----|----|----|
|home_office_uuid|uuid |PK | |N |06exxx256-e1f0-3456-b001-86abe27cecc8 |Unique identification for each record in the table |
|home_office_code|text |Unique | |N |WealthAggregators| |
|home_office_name|text | | |Y |Wealth Aggregators Inc.| |
|email |text | | |Y |billing@example.com | |
|is_active |Boolean | | |Y |True |This can be True/False |
|created_at |timestamp | | |N |2023-07-27 13:44:16.594 |This is the UTC time |
|Updated_at |timestamp | | |N |2023-07-27 13:44:16.594 |This is the UTC time |


## Advisory Firms
Typically, this is a Wealth Manager, RIA, MFO or a SFO    
#### table name: ```ct_advisory_firms```
|Column |Data Type |Settings |References |Nullable |Sample value |Description |
|:----|:----|:----|:----|:----|:----|:----|
|advisory_firm_uuid |uuid |PK | |N |06e35256-e1f0-4533-b001-86abe27cecc8 |Unique identification for each record in the table |
|advisory_firm_code|text |Unique | |N |Acme | |
|advisory_firm_name|text | | |Y |Acme Group | |
|email |text | | |Y |billing@example.com | |
|is_active |Boolean | | |Y |True |This can be True/False |
|created_at |timestamp | | |N |2023-07-27 13:44:16.594 |This is the UTC time |
|Updated_at |timestamp | | |N |2023-07-27 13:44:16.594 |This is the UTC time |


## Households
- These are the clients of the advisory_firm or family office
- Typically a client having multiple accounts (but a single decision making process). Example multiple families in case of a MFO
- Somewhat redundant in the case of a Single Family Office

#### Table name: ```ct_households```

|Column |Data Type |Settings |References |Nullable |Sample Value |Description |
|:----|:----|:----|:----|:----|:----|:----|
|organization_uuid |uuid |PK | |N |83bea7fe-44aa-4038-895c-2acda1df8135 | |
|Organization_code |text |Unique | |Y |Acme-Howa | |
|Organization_name |text | | |Y |Howard Family | |
|Is_active |Boolean | | |Y |True |This can be True/False |
|Created_at |timestamp | | |N |2023-07-27 13:56:40.957 |This is the UTC time |
|Updated_at |timestamp | | |N |2023-07-27 13:56:40.957 |This is the UTC time |
|Subscriber_uuid |uuid |FK |ct_subscribers.subscriber_uuid |N |06e35256-e1f0-4533-b001-86abe27cecc8 | |
| | | | | | | |


## Entities

- Each Household can have multiple entities
- Entities can be persons, trusts, corporates etc.
- Entities can own all or part of other Entities
- Entities can also own all or part of Portfolios
- The % ownership changes over time

#### Table name: ```ct_entities```

|Column |Data Type |Settings |References |Nullable |Sample Value |Description |
|:----|:----|:----|:----|:----|:----|:----|
|entity_uuid |uuid |PK | |N |2fe62ec0-4609-4608-96d8-0ee9426b79a8 | |
|entity_code |text |Unique | |N |DAN-RB12 | |
|entity_type |text | | |N |Limited Liability Company |This can be of following types: |
| | | | | | |Limited Liability Company/Trust/Individual |
|Display_name |text | | |N |Daniel Reuben |This is the name of the Entity |
|Is_active |Boolean | | |Y |True |This can be True/False |
|Reporting_currency |text | | |N |USD | |
|tax_status |text | | |N |Non-taxable |Can be  Taxable or  Non-taxable |
|sub_type |text | | |Y | | |
|Deactivation_date |timestamp | | |Y | | |
|Created_at |timestamp | | |N |2023-10-18 09:32:38.573 |This is the UTC time |
|Updated_at |timestamp | | |N |2023-10-18 09:32:38.573 |This is the UTC time |
|organization_uuid |uuid |FK |Ct_organizations.organization_uuid |N |773ee112-2a2c-4e9d-a4c2-44a294dafd3d |This is the unique identifier referenced in ct_organizations table |

#### Table name: ```ct_users_master```

| Column       | Data Type | Settings | References | Nullable | Sample Value               | Description                                |
| ------------ | --------- | -------- | ---------- | -------- | -------------------------- | ------------------------------------------ |
| User_id      | serial4   | PK       |            | N        | 1                          | This is a sequential auto generated series |
| username     | text      | Unique   |            | Y        | acme_admin_ab@kurtosys.org |                                            |
| Display_name | text      |          |            | Y        | Acme Admin - AB            |                                            |
| Is_active    | Boolean   |          |            | Y        | TRUE                       | This can be True/False                     |
| Created_at   | timestamp |          |            | N        | 49:29.1                    | This is the UTC time                       |
| Updated_at   | timestamp |          |            | N        | 49:29.1                    | This is the UTC time                       |

## Portfolios ##
•	All assets are stored here (inside the relevant Currency Account)

•	Each Portfolio is owned by at least one Entity
### Table name: ```ct_portfolios```

| **Column**            | **Data Type** | **Settings** | **References**               | **Nullable** | **Sample Value**                     | **Description**                                                |
| --------------------- | ------------- | ------------ | ---------------------------- | ------------ | ------------------------------------ | -------------------------------------------------------------- |
| portfolio_uuid        | uuid          | PK           |                              | N            | f86377a4-f916-4d9d-8049-b029ebad8e1e |                                                                |
| portfolio_code        | text          | Unique       |                              | Y            | acme-ab-aab-xy_05irr                 |                                                                |
| Display_name          | text          |              |                              | Y            | ABCD - Fund                          |                                                                |
| Reporting_currency    | text          |              |                              | N            | USD                                  |                                                                |
| status                | text          |              |                              | N            | active                               | This is can be active/deleted                                  |
| Created_at            | timestamp     |              |                              | N            | 2023-12-01 13:40:55.770              | This is the UTC time                                           |
| Updated_at            | timestamp     |              |                              | N            | 2023-12-01 13:40:55.770              | This is the UTC time                                           |
| custodian_uuid        | uuid          | FK           | ct_custodians.custodian_uuid | Y            | 7f18e883-22aa-4eae-a873-200ab25cb037 | This is the unique idenyifier referenced from Custodians table |
| account_title         | text          |              |                              | Y            | ABC Irrevocable Trust                |                                                                |
| account_title_display | text          |              |                              | Y            | ABC Irrevocable Trust                |                                                                |
| tax_status            | text          |              |                              | Y            | Taxable                              | This can be Taxable/Non-Taxable                                |
| title_group           | text          |              |                              | Y            | ABC Irrevocable Trust                |                                                                |

## Currency Accounts ##

- A Currency (CCY) Account is where transactions in a specified currency are booked.  
- For example, a USD denominated transaction can only be booked into a USD CCY account and not into a EUR or any other CCY Account. 
-	Each Portfolio can have any number of Currency Accounts
### Table name : ```ct_currency_accounts```

| **Column**            | **Data Type** | **Settings** | **References**               | **Nullable** | **Sample Value**                     | **Description**      |
| --------------------- | ------------- | ------------ | ---------------------------- | ------------ | ------------------------------------ | -------------------- |
| Currency_account_uuid | uuid          | PK           |                              | N            | 7e1adb8e-542f-4c60-b063-58f0b21c1b0c |                      |
| Currency_account_code | text          | Unique       |                              | Y            | fortis-jhp-dwrius3a-863216207-usd-01 |                      |
| currency              | text          |              |                              | N            | USD                                  |                      |
| account_number        | text          |              |                              | Y            |                                      |                      |
| status                | text          |              |                              | N            | active                               |                      |
| Created_at            | timestamp     |              |                              | N            | 2023-10-18 05:54:24.171              | This is the UTC time |
| Updated_at            | timestamp     |              |                              | N            | 2023-10-18 05:54:24.171              | This is the UTC time |
| Display_name          | text          |              |                              | Y            | acme-abc-dwrius3a-863216207-usd-01   |                      |
| Portfolio_uuid        | text          | FK           | Ct_portfolios.portfolio_uuid | Y            | 6c4838db-2674-46e9-bad1-3bdac3c33c6b |                      |

## Custodian ##
-	A custodian is the institution that is holding the Account and its Currency Accounts
### Table name : ```ct_custodians```

| **Column**     | **Data Type** | **Settings** | **References** | **Nullable** | **Sample Value**                     | **Description**      |
| -------------- | ------------- | ------------ | -------------- | ------------ | ------------------------------------ | -------------------- |
| custodian_uuid | uuid          | PK           |                | N            | 10644be7-3903-42bc-bfd7-e626fd4d4073 |                      |
| custodian_code | varchar       | Unique       |                | N            | BLFSLBBX                             |                      |
| custodian_name | text          |              |                | N            | Libano Francaise Finance             |                      |
| created_at     | timestamp     |              |                | N            | 2023-08-08 14:29:49.548              | This is the UTC time |
| updated_at     | timestamp     |              |                | N            | 2023-08-08 14:29:49.548              | This is the UTC time |

## Entity Relationships ##
-	Its ownership relation between entity to entity and entity to its portfolios with ownership percentages split over time
### Table name : ```ct_entity_relationships```

| Column                       | Data Type | Settings | References                 | Nullable | Sample Value                         | Description                 |
| ---------------------------- | --------- | -------- | -------------------------- | -------- | ------------------------------------ | --------------------------- |
| id                           | Int4      | PK       |                            | N        | 121                                  |                             |
| as_of_date                   | date      |          |                            | N        | 1/1/1900                             |                             |
| Relationship_type            | text      |          |                            | N        | entity-portfolio                     | Entity-portfolio            |
| Ownership_percentage         | numeric   |          |                            | Y        | 1                                    | As of historical dates 100% |
| created_at                   | timestamp |          |                            | N        | 38:33.5                              | This is the UTC time        |
| updated_at                   | timestamp |          |                            | N        | 38:33.5                              | This is the UTC time        |
| Parent_entity_uuid           | uuid      | FK       | ct_entities.entity_uuid    | N        | 0edf23f9-c202-43ba-84e6-62381dc0c980 |                             |
| Child_node_uuid              | uuid      | FK       | ct_entities.entity_uuid OR ct_portfolios.portfolio_uuid | N        | 148c089a-b66a-454f-9f90-c9b8bc2d2db7 |                             |

## Securities ##
-	All public and private securities
### Table name : `ct_secutities_master`

| Column                    | Data Type | Settings | References         | Nullable | Sample Value                         | Description                                       |
| ------------------------- | --------- | -------- | ------------------ | -------- | ------------------------------------ | ------------------------------------------------- |
| security_uuid             | uuid      | PK       |                    | N        | 0058a9ba-1953-4749-8f1d-32a84e0f4aa5 |                                                   |
| security_name             | text      |          |                    | Y        | LITHIUM AMERICAS CORP                |                                                   |
| is_public                 | text      |          |                    | Y        | TRUE                                 |                                                   |
| source_type_id            | int4      | FK       | Ct_source_types.id | N        |                                      |                                                   |
| collation_asset_class     | text      |          |                    | N        | Equity                               | These are the Collation default asset classes     |
| collation_sub_asset_class | text      |          |                    | Y        | Agriculture                          | These are the Collation default sub-asset classes |
| collation_geography       | text      |          |                    | Y        | USA                                  | These are the Collation default                   |
| isin                      | text      |          |                    | Y        |                                      |                                                   |
| cusip                     | text      |          |                    | Y        | 53680Q207                            |                                                   |
| sedol                     | text      |          |                    | Y        |                                      |                                                   |
| wkn                       | text      |          |                    | Y        |                                      |                                                   |
| valoren                   | text      |          |                    | Y        |                                      |                                                   |
| id_bb_global              | text      |          |                    | Y        |                                      |                                                   |
| bloomberg_ticker          | text      |          |                    | Y        | LAC UB Equity                        |                                                   |
| id_stock_exchange         | text      |          |                    | Y        |                                      |                                                   |
| fs_company_id             | text      |          |                    | Y        |                                      |                                                   |
| fs_updated_at             | timestamp |          |                    | Y        |                                      |                                                   |
| more_info                 | jsonb     |          |                    | Y        |                                      |                                                   |
| created_by                | int4      |          |                    | Y        |                                      |                                                   |
| created_at                | timestamp |          |                    | N        | 32:42.3                              | This is the UTC time                              |
| updated_at                | timestamp |          |                    | N        | 32:42.3                              | This is the UTC time                              |
| exchange_code             | text      |          |                    | Y        | UB                                   |                                                   |
| exchange_ticker           | text      |          |                    | Y        | LAC                                  |                                                   |
| collation_ticker          | text      | Unique   |                    | Y        | BBG000BGM8D5                         |                                                   |
| contract_currency         | text      |          |                    | Y        | USD                                  |                                                   |
|  |

## Security Prices ##
- Hold security prices from multiple sources
### Table name: `ct_secutity_prices`

| Column            | Data Type | Settings | References                       | Nullable | Sample Value                         | Description |
| ----------------- | --------- | -------- | -------------------------------- | -------- | ------------------------------------ | ----------- |
| Price_uuid        | uuid      | PK       |                                  | N        | 000df0ee-1a4e-48ba-a973-a558fdfb6c3c |             |
| security_uuid     | uuid      | FK       | Ct_security_master.security_uuid | N        | 6a65cfff-b739-489c-bec1-f16974f8c8da |             |
| price_source_uuid | uuid      | FK       | Ct_price_sources.uuid            | N        | 5a7c3c28-3f01-42b4-ac06-8f0915608788 |             |
| history_date      | date      |          |                                  | N        | 12/26/2018                           |             |
| close             | numeric   |          |                                  | N        | 19.44                                |             |
| open              | numeric   |          |                                  | N        | 18.63                                |             |
| high              | numeric   |          |                                  | Y        | 19.49                                |             |
| low               | numeric   |          |                                  | Y        | 18.17                                |             |
| clean             | numeric   |          |                                  | Y        | 19.44                                |             |
| created_by        | int4      |          |                                  | Y        |                                      |             |
| created_at        | timestamp |          |                                  | N        | 36:18.7                              |             |
| updated_at        | timestamp |          |                                  | N        | 36:18.7                              |             |

## Price Sources ##
- A holding can use prices from any of the available price sources like
  - Custodian
  - Portfolio
  - Collation Default

### Table name: `ct_price_sources`

| Column         | Data Type    | Settings | References                        | Nullable | Sample Value                         | Description          |
| -------------- | ------------ | -------- | --------------------------------- | -------- | ------------------------------------ | -------------------- |
| uuid           | uuid         | PK       |                                   | N        | 64b79917-9e4a-480b-87b1-30529e9b8c31 |                      |
| description    | Varchar(255) |          |                                   | Y        | fortis-jhp-chasus33-92001257         |                      |
| source_type_id | int4         | FK       | Ct_security_master.Source_type_id | N        | 3                                    |                      |
| custodian_uuid | uuid         | FK       | Ct_custodians.custodian_uuid      | Y        |                                      |                      |
| portfolio_uuid | uuid         | FK       | Ct_portfolios.portfolio_uuid      | Y        | 5fa0b0bf-a2e4-4d19-ac6d-372f277d5b81 |                      |
| created_at     | timestamp    |          |                                   | N        | 22:00.4                              | This is the UTC time |
| updated_at     | timestamp    |          |                                   | N        | 22:00.4                              | This is the UTC time |


## Private Security ##
- Security not listed on any exchange like real estate, painting etc

### Table name : `ct_private_securities`

| Column            | Data Type | Settings | References                         | Nullable | Sample Value                         | Description          |
| ----------------- | --------- | -------- | ---------------------------------- | -------- | ------------------------------------ | -------------------- |
| id                | serial4   | PK       |                                    | N        | 45                                   |                      |
| created_by        | int4      |          |                                    | Y        |                                      |                      |
| created_at        | timestamp |          |                                    | N        | 57:59.6                              | This is the UTC time |
| updated_at        | timestamp |          |                                    | N        | 57:59.6                              | This is the UTC time |
| security_uuid     | uuid      | FK       | Ct_security_master.security_uuid   | N        | 59644b6e-d02c-4b6d-a449-cc0529c28c23 |                      |
| organization_uuid | Uuid      | FK       | Ct_organizations.organization_uuid | N        | 83bea7fe-44aa-4038-895c-2acda1df8135 |                      |


## Classification Overrides ##
- We can override the default classifications of any security at multiple levels like subscriber/organization/entity/portfolio
  -	asset_class
  - sub_asset_class
  - geography
  - category
  
  ### Table name : `Ct_classification_overrides`

 
| Column                                | Data Type              | Settings | References                        | Nullable | Sample Value                         | Description                                                                   |
| ------------------------------------- | ---------------------- | -------- | --------------------------------- | -------- | ------------------------------------ | ----------------------------------------------------------------------------- |
| id                                    | int4                   | PK       |                                   | N        | 840                                  |                                                                               |
| resource_id                           | uuid                   | FK       | Ct_security_master.security_uuid  | N        | c3a0c97b-75ec-4244-8948-4f946908f556 | This can be security_uuid or currency_account_uuid depending on resource_type |
| tag_type                              | Public.”tag_type_enum” |          |                                   | N        | asset_class                          | Can be asset_class, industry, category                                        |
| target_id                             | uuid                   | FK       | Ct_subscribers.subscriber_uuid OR Ct_organizations.organization_uuid OR  Ct_entities.entity_uuid OR Ct_portfolios.portfolio_uuid    |  N        | 773ee112-2a2c-4e9d-a4c2-44a294dafd3d | This varies as per target_type  |  |          ||          
| target_type                           | Public.”tag_type_enum” |          |                                   | N        | organization                         |                                                                               |
| created_at                            | timestamp              |          |                                   | N        | 02:13.6                              | This is the UTC time                                                          |
| tag_value                             | Varchar(255)           |          |                                   | N        | Alternatives                         |                                                                               |
| updated_at                            | timestamp              |          |                                   | N        | 02:13.6                              | This is the UTC time                                                          |
| last_updated_by                       | int4                   |          |                                   | Y        |                                      |                                                                               |
| resource_type                         | text                   |          |                                   | Y        | security                             |                                                                               |

## Transactions ##
- Transactions are made from a selection of information that allows the user to create & update the holdings in their Collation Account.
- A Transaction is made up of several parts. Some transactions contain more parts than others dependent on their purpose, which are the following: 
  - Entities
  - Custodian
  - Child Account
  - Currency Account
  - Amount
  - Ticker
  -	Quantity
  - Prices
  - Transaction Types
  - Transaction Dates

### Table name : `ct_transactions`

| Column                                 | Data Type                   | Settings | References                                  | Nullable | Sample Value                             | Description                 |
| -------------------------------------- | --------------------------- | -------- | ------------------------------------------- | -------- | ---------------------------------------- | --------------------------- |
| transaction_id                         | uuid                        | PK       |                                             | N        | 67023172-b633-40bf-aaef-4dd64ff906d2     |                             |
| version                                | int4                        |          |                                             | N        |                                          |                             |
| trade_date                             | date                        |          |                                             | N        | 7/28/2008                                |                             |
| settlement_date                        | date                        |          |                                             | N        | 7/28/2008                                |                             |
| accounting_date                        | date                        |          |                                             | Y        | 7/28/2008                                |                             |
| security_uuid                         | uuid                        | FK       | Ct_security_master. security_uuid           | Y        | fcf33df4-32e2-49a5-b6c4-7111baeecf81     |                             |
| related_security_uuid                  | uuid                        | FK       | Ct_security_master. security_uuid           | Y        |                                          |                             |
| currency_account_uuid                  | uuid                        | FK       | Ct_currency_accounts. currency_account_uuid | N        | 61f5bef4-121e-461a-a7da-380fa228cfe5     |                             |
| transfer_account_id                    | uuid                        |          |                                             | Y        |                                          |                             |
| related_security_currency_account_uuid | uuid                        | FK       | Ct_currency_accounts. currency_account_uuid | Y        |                                          |                             |
| vendor                                 | text                        |          |                                             | Y        |                                          |                             |
| transaction_type                       | text                        |          |                                             | Y        | Money Out                                |                             |
| trade_type                             | text                        |          |                                             | Y        | Outflow                                  |                             |
| client_transaction_type_override       | text                        |          |                                             | Y        | Contribution                             |                             |
| trade_sub_type                         | text                        |          |                                             | Y        |                                          |                             |
| trade_price                            | numeric                     |          |                                             | Y        | 1                                        |                             |
| effective_price                        | numeric                     |          |                                             | Y        | 1                                        |                             |
| gross_amount                           | numeric                     |          |                                             | Y        | 176553.71                                |                             |
| transaction_description                | text                        |          |                                             | Y        | Contribution to XYZ Group, LLC#Money Out |                             |
| accrued_interest                       | numeric                     |          |                                             | Y        |                                          |                             |
| fee_tax                                | numeric                     |          |                                             | Y        | 0                                        |                             |
| fee_tax_breakdown                      | numeric                     |          |                                             | Y        | 0                                        |                             |
| settlement_amount                      | numeric                     |          |                                             | Y        | 165976                                   |                             |
| quantity                               | numeric                     |          |                                             | Y        | 92590                                    |                             
| outstanding_quantity                   | numeric                     |          |                                             | Y        | 0                                        |                             |
| closed_quantity                        | numeric                     |          |                                             | Y        | 0                                        |                             |
| Cost_basis                             | numeric                     |          |                                             | Y        | 0                                        |                             |
| cost_basis_methodology                 | text                        |          |                                             | Y        | Average                                  |                             |
| realized_pnl                           | numeric                     |          |                                             | Y        | 0                                        |                             |
| day_1_notional                         | numeric                     |          |                                             | Y        | 92590                                    |                             |
| day_1_mtm                              | numeric                     |          |                                             | Y        | 20000                                    |                             |
| day_1_price_close                      | numeric                     |          |                                             | Y        | 20000                                    |                             |
| day_1_price_clean                      | numeric                     |          |                                             | Y        | 1                                        |                             |
| day_1_price_id                         | uuid                        |          |                                             | Y        |                                          |                             |
| day_1_price_source_id                  | uuid                        |          |                                             | Y        |                                          |                             |
| day_1_fxrate                           | numeric                     |          |                                             | Y        | 1                                        |                             |
| source_transaction_type                | text                        |          |                                             | Y        |                                          |                             |
| source_reference_id                    | text                        |          |                                             | Y        |                                          |                             |
| source_security_identifier             | text                        |          |                                             | Y        |                                          |                             |
| client_more_info                       | jsonb                       |          |                                             | Y        |                                          |                             |
| provider_details                       | jsonb                       |          |                                             | Y        |                                          |                             |
| tax_lot_id                             | text                        |          |                                             | Y        |                                          |                             |
| tax_lot_status                         | text                        |          |                                             | Y        |                                          |                             |
| status                                 | Oublic.”transaction_status” |          |                                             | N        | active                                   | This can be active/archived |
| created_at                             | timestamp                   |          |                                             | N        | 43:22.1                                  | This is the UTC time        |
| updated_at                             | timestamp                   |          |                                             | N        | 32:10.2                                  | This is the UTC time        |
| commitment_amount                      | numeric                     |          |                                             | Y        | 0                                        |                             |
| source_type_id                         | int4                        | FK       | Ct_source_types.id                          | N        | 6                                        |                             |


## Daily Position ##
-	This table contains a daily snapshot of all holdings of a particular customer or customers
- These are the Historical holdings

### Table name : `ct_daily_positions`

| Column                           | Data Type                 | Settings | References                                | Nullable | Sample Value                         | Description          |
| -------------------------------- | ------------------------- | -------- | ----------------------------------------- | -------- | ------------------------------------ | -------------------- |
| uuid_key                         | uuid                      | PK       |                                           | N        | cbdda6f9-a379-43ba-8af8-f39150ad455f |                      |
| history_date                     | date                      |          |                                           | N        | 4/2/2018                             |                      |
| quantity                         | numeric                   |          |                                           | Y        | 3                                    |                      |
| latest_price_date                | date                      |          |                                           | Y        | 4/2/2018                             |                      |
| latest_price_close               | numeric                   |          |                                           | Y        | 155.51                               |                      |
| latest_price_clean               | numeric                   |          |                                           | Y        |                                      |                      |
| latest_price_id                  | int4                      |          |                                           | Y        |                                      |                      |
| latest_price_source_id           | int4                      |          |                                           | Y        |                                      |                      |
| market_value_usd                 | numeric                   |          |                                           | Y        | 100000                               |                      |
| mtm_total_usd                    | numeric                   |          |                                           | Y        | 100000                               |                      |
| mtm_price_usd                    | numeric                   |          |                                           | Y        | 100000                               |                      |
| mtm_fx_usd                       | numeric                   |          |                                           | Y        | 0                                    |                      |
| mtm_accrued_usd                  | numeric                   |          |                                           | Y        | 0                                    |                      |
| market_value_child_account_ccy   | numeric                   |          |                                           | Y        | 100000                               |                      |
| mtm_total_child_account_ccy      | numeric                   |          |                                           | Y        | 100000                               |                      |
| mtm_price_child_account_ccy      | numeric                   |          |                                           | Y        | 100000                               |                      |
| mtm_fx_child_account_ccy         | numeric                   |          |                                           | Y        | 0                                    |                      |
| mtm_accrued_child_account_ccy    | numeric                   |          |                                           | Y        | 0                                    |                      |
| client_more_info                 | jsonb                     |          |                                           | Y        |                                      |                      |
| market_value_base_ccy            | numeric                   |          |                                           | Y        | 100000                               |                      |
| mtm_total_base_ccy               | numeric                   |          |                                           | Y        | 943028                               |                      |
| mtm_price_base_ccy               | numeric                   |          |                                           | Y        | 943028                               |                      |
| mtm_fx_base_ccy                  | numeric                   |          |                                           | Y        | \-566.8                              |                      |
| mtm_accrued_base_ccy             | numeric                   |          |                                           | Y        | 566.8                                |                      |
| avg_price                        | numeric                   |          |                                           | Y        | 90.68                                |                      |
| last_avg_price_usd               | numeric                   |          |                                           | Y        | 90.68                                |                      |
| last_avg_price_base_ccy          | text                      |          |                                           | Y        | 90.68                                |                      |
| last_avg_price_child_account_ccy | numeric                   |          |                                           | Y        | 90.68                                |                      |
| contract_fx_rate                 | numeric                   |          |                                           | Y        | 1                                    |                      |
| market_value_ccy                 | numeric                   |          |                                           | Y        | 62297.95                             |                      |
| basefx_rate                      | numeric                   |          |                                           | Y        | 1                                    |                      |
| child_account_fx_rate            | numeric                   |          |                                           | Y        | 1                                    |                      |
| status                           | Public.”positions_status” |          |                                           | N        | active                               |                      |
| Provider_details                 | jsonb                     |          |                                           | Y        |                                      |                      |
| created_at                       | timestamp                 |          |                                           | N        | 40:58.2                              | This is the UTC time |
| updated_at                       | timestamp                 |          |                                           | N        | 40:58.2                              | This is the UTC time |
| currency_account_uuid            | uuid                      | FK       | Ct_currency_account.currency_account_uuid | Y        | 06e4be5e-733b-4a1e-bd81-d8948bb62fe8 |                      |
| security_uuid                    | uuid                      | FK       | Ct_security_master.security_uuid          | Y        | fcf33df4-32e2-49a5-b6c4-7111baeecf81 |                      |


## Custom Tags ##
- Can add custom tags to any Transaction

### Tale name : `Ct_transactions_custom_tags`

| Column                           | Data Type | Settings | References | Nullable | Sample Value                         | Description                      |
| -------------------------------- | --------- | -------- | ---------- | -------- | ------------------------------------ | -------------------------------- |
| tag_id                           | uuid      | PK       |            | N        | 9a30aab9-2555-43b6-bfda-0620c7c57b28 |                                  |
| transaction_id                   | uuid      | Unique   |            | N        | 0f4de84d-232d-428b-baa5-7eda0cdd3101 |                                  |
| related_asset_liability          | text      |          |            | Y        | WHEELOCK_STREET_GROUP_LLC            |                                  |
| income_expense_type              | text      |          |            | Y        | 55310 Employee:  Gross Pay           |                                  |
| transfer_account                 | text      |          |            | Y        | fortis-jhp-abcgroupllc-zxy-usd-01    |                                  |
| return_of_capital                | numeric   |          |            | Y        | 21686                                |                                  |
| ordinary_income                  | numeric   |          |            | Y        | 99000                                |                                  |
| realized_income_st               | numeric   |          |            | Y        | \-22948.65                           |                                  |
| realized_income_lt               | numeric   |          |            | Y        | 17.95                                |                                  |
| recallable_distribution          | numeric   |          |            | Y        |                                      |                                  |
| commitment_impact                | numeric   |          |            | Y        |                                      |                                  |
| created_at                       | timestamp |          |            | N        | 58:50.0                              | This is the UTC time             |
| updated_at                       | timestamp |          |            | N        | 50:20.2                              | This is the UTC time             |
| family                           | text      |          |            | Y        |                                      |                                  |
| vendor                           | text      |          |            | Y        | Peter Attia Podcast                  |                                  |
| chart_of_account                 | text      |          |            | Y        | 56280 Subscriptions                  |                                  |
| client_transaction_type_override | text      |          |            | Y        | Distribution                         | Can be contribution/distribution |



Image

Database schema![Alt text](Untitled%20Diagram.drawio.png)

Editable diagram :

[https://viewer.diagrams.net/?tags=%7B%7D&highlight=0000ff&edit=\_blank&layers=1&nav=1&title=ERDiagram#Uhttps%3A%2F%2Fdrive.g\[...\]f\_%26export%3Ddownload](https://viewer.diagrams.net/?tags=%7B%7D&highlight=0000ff&edit=_blank&layers=1&nav=1&title=ERDiagram#Uhttps%3A%2F%2Fdrive.google.com%2Fuc%3Fid%3D1Tb30tyco4NSF9o9bgCphVL3Wua0Lp2f_%26export%3Ddownload)
