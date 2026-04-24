## MIST 4610 Project 2 - Group A3
Catherine Brumbeloe- Group Leader  
Camryn Copeland- SQL Writer  
Joseph Halab- Database Wrangler   
Diya Mopur- Database Designer  

## Case Summary
Northline Outfitters is a small online retail company that sells student-focused lifestyle and technology accessories, including apparel, backpacks, water bottles, keyboards, phone cases, and other related products. The company purchases merchandise from outside vendors and sells directly to customers in the United States and Canada.
Because the business is still growing, much of its operational data is maintained in Excel spreadsheets instead of a structured database system. As a result, the provided data contains several organizational and quality challenges, including duplicated information, inconsistent formatting, mixed date and measurement formats, embedded customer details within text fields, inconsistent discount and tax values, and duplicate or unstandardized product records. These issues create difficulties in storing, managing, and analyzing data accurately. The purpose of this project is to transform the raw spreadsheet data into a structured database model that improves organization, consistency, data integrity, and overall usability.

## Conceptual Model 

The conceptual model organizes Northline Outfitters’ retail data into separate normalized entities to improve data organization, reduce redundancy, and maintain data integrity. Each entity contains attributes that describe the data being stored, and each entity includes a primary identifier used to uniquely distinguish each record.  

The Customer entity stores customer-related attributes such as customer name, email, and loyalty information. The primary identifier for this entity is Customer_ID. One customer may place multiple orders.  

The Order entity stores order-level attributes including sale date, shipping information, and payment details. The primary identifier is Order_ID. Each order belongs to one customer and is processed by one employee.  

The Order_Line entity stores line-item transaction details such as quantity, unit price, discount, tax, and line total. The primary identifier is Line_ID. This entity connects orders and products, allowing one order to contain multiple purchased items.  

The Employee entity stores employee information for individuals who process customer orders. The primary identifier is Employee_ID. Each employee may process many orders.  

The Manager entity stores manager information and uses Manager_ID as its primary identifier. One manager may supervise multiple employees.  

The Product entity stores product-related attributes including SKU, product description, pricing, cost, and inventory details. The primary identifier is Product_ID. A product may appear in multiple order lines.

The Vendor entity stores supplier information such as vendor name, phone number, and representative. The primary identifier is Vendor_ID. One vendor may supply many products.  

The Category entity stores product category classifications. The primary identifier is Category_ID. One category may include multiple products.  

The Payment_Method entity stores accepted payment types for transactions. The primary identifier is Payment_Method_ID. One payment method may be associated with multiple orders.  

The relationships between these entities create a structured database where customers place orders, employees process orders, managers supervise employees, orders contain multiple order lines, products appear in order lines, vendors supply products, and categories classify products.  

## Data Quality Assessment
### 1. Sales_Dump Dataset  

*line_id* Prefix “LN-” is unnecessary and should be removed.  

*order_id* Prefixes “UORD-” and “CORD-” should be removed. Country indicators (C/U) are stored elsewhere.  

*employee_ref* Prefixes “EMU-” and “EMC-” should be removed. Country indicators are redundant.   

*sale_date* Dates appear in multiple formats and must be standardized to a single format.   

*customer_info* Not in first normal form. Names need to be split and loyalty, guest, and student flags need to be separated.


*payment_method* Card names vary in case and include abbreviations.    

*sku* SKU values appear in inconsistent case.    

*category* Not in first normal form; some products contain multiple categories.    

*unit_price* Currency symbols must be separated into a dedicated currency column.   

*discount* Inconsistent formats (e.g., “10%” vs “promo5”).   

*tax* Mixed formats: some values are percentages, others are absolute amounts.  

*line_total* Some values include a dollar sign while others do not.  

*return_flag* Contains blank rows.  

*customer_email* Varying cases, some of which are stored as different values. This is redundant as Email addresses are not case sensitive.  

  

### Product_Supplier_Master Dataset   

*sku* SKU values not in consistent case.   

*category* Not in first normal form. Multiple categories listed in a single field.  

*vendor_phone* Phone numbers appear in multiple formats.  

*vendor_rep* Full name must be split into first name and last name.  

*cost* and *list_price* Currency symbols should be separated from numeric values.  

*pack_size* Inconsistent formatting; (e.g., 12-pack, case of 6, etc.).  

*weight* and *length* Units vary (e.g., “kilograms” vs “kg”).  

## Data Cleaning Process  
1. Sales_Dump Dataset  

*line_id* Removed the "LN-" prefix using a string‑replace function; retained only the numeric portion.  

*order_id* Stripped "UORD-" and "CORD-" prefixes; extracted only the order number since country codes are stored elsewhere.  

*employee_ref* Removed "EMU-" and "EMC-" prefixes; standardized the field to a clean employee ID.  

*sale_date* Parsed all date formats and converted them into a single ISO format (YYYY-MM-DD).  

*customer_info* Split full name into first name and last name columns. Extracted embedded flags (loyalty, guest, student) into separate columns.  


*payment_method* Standardized all values to consistent title case and expanded abbreviations (e.g., “MC” → “Mastercard”).  

*sku* Converted all SKU values to uppercase for consistency.  

*category* Split multi‑category entries into separate rows.  

*unit_price* Removed currency symbols and stored numeric values in unit_price; created a new currency column populated from the symbol.  

*discount* Standardized all discounts to a numeric percentage (e.g., "promo5" → 5%).  

*tax* Converted all tax values to a single format (tax rate).    

*line_total* Removed dollar signs and converted all values to numeric type.   

*return_flag* Replaced blank values with assumed ‘N’.  

  

2. Product_Supplier_Master Dataset  

*sku* Standardized all SKU values to uppercase.  

*category* Split multi‑category entries into separate rows.  

*vendor_phone* Reformatted all phone numbers into a single standard format.  

*vendor_rep* Split full name into first name and last name.  

*cost* and *list_price* Removed currency symbols and stored numeric values only; added a currency column.  

*pack_size* Standardized all pack size descriptions into a consistent format.  

*weight* and *length* Reformatted to ensure consistency.  

 

Data Cleaning Actions Recommended  

*weight* and *length* A standard unit of measurement should be agreed upon for data storage purposes. If necessary, a conversion unit entity can be created for quick conversions into the dominant currency of the data system.  

*size_or_weight* We have assumed this data is specifically extracted and stored for packaging purposes. In this case, we recommend standardized packaging size/weight parameters be established so that each product may be sorted into a ProductSizing classes (such as small, medium, large, etc.)  

*EmployeeFname, EmployeeLname, EmployeeEmail, ManagerFNname, ManagerLname, ManagerEmail* Attributes were created with the assumption that each Employee and Manager ID corresponds to an existing Manager and Employee with the mentioned characteristics. Storing this data would be sensible and relevant on performant tasks such as performance reviews.  

## Queries 
