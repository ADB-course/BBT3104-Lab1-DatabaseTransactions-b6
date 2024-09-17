![WhatsApp Image 2024-09-17 at 12 50 18_c2345af8](https://github.com/user-attachments/assets/07c70f51-4ef1-48d4-b984-284e6f3e2eab)![WhatsApp Image 2024-09-17 at 12 50 18_d23cbd04](https://github.com/user-attachments/assets/58f31faa-5b3a-4c8d-af17-95ba0f2817d8)[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/r-tQZu0l)
# BBT3104-Lab1of6-DatabaseTransactions


| **Key**                                                               | Value                                                                                                                                                                              |
|---------------|---------------------------------------------------------|
| **Group Name**                                                               | b6 |
| **Semester Duration**                                                 | 19<sup>th</sup> August - 25<sup>th</sup> November 2024                                                                                                                       |

## Flowchart
![WhatsApp Image 2024-09-17 at 12 50 18_d23cbd04](https://github.com/user-attachments/assets/696d5366-8ca1-4295-b477-e4ab736a84ad)

## Pseudocode
BEGIN TRANSACTION

// Step 1: Get the latest order number
SET orderNumber = SELECT MAX(orderNumber) + 1 FROM orders

// Step 2: Insert a new order
INSERT INTO orders (orderNumber, orderDate, requiredDate, shippedDate, status, customerNumber)
VALUES (orderNumber, CURRENT_DATE, CURRENT_DATE + 3 days, CURRENT_DATE + 2 days, 'In Process', 145)

// Step 3: Create SAVEPOINT before inserting the first product
SAVEPOINT before_product_1

// Step 4: Insert first product and update stock
INSERT INTO orderdetails (orderNumber, productCode, quantityOrdered, priceEach, orderLineNumber)
VALUES (orderNumber, 'S18_1749', 2724, 136, 1)

SET quantityInStock = SELECT quantityInStock FROM products WHERE productCode = 'S18_1749'

UPDATE products SET quantityInStock = quantityInStock - 2724 WHERE productCode = 'S18_1749'

// Step 5: Create SAVEPOINT before inserting the second product
SAVEPOINT before_product_2

// Step 6: Insert second product and update stock
INSERT INTO orderdetails (orderNumber, productCode, quantityOrdered, priceEach, orderLineNumber)
VALUES (orderNumber, 'S18_2248', 540, 55.09, 2)

SET quantityInStock = SELECT quantityInStock FROM products WHERE productCode = 'S18_2248'

UPDATE products SET quantityInStock = quantityInStock - 540 WHERE productCode = 'S18_2248'

// Step 7: Rollback to the SAVEPOINT before second product if an error occurs
ROLLBACK TO SAVEPOINT before_product_2

// Step 8: Create SAVEPOINT before inserting the third product
SAVEPOINT before_product_3

// Step 9: Insert third product and update stock
INSERT INTO orderdetails (orderNumber, productCode, quantityOrdered, priceEach, orderLineNumber)
VALUES (orderNumber, 'S12_1099', 68, 95.34, 3)

SET quantityInStock = SELECT quantityInStock FROM products WHERE productCode = 'S12_1099'

UPDATE products SET quantityInStock = quantityInStock - 68 WHERE productCode = 'S12_1099'

// Step 10: Receive payment for the order
INSERT INTO payments (customerNumber, checkNumber, paymentDate, amount)
VALUES (145, 'JM555210', CURRENT_DATE, 300000)

// Step 11: Commit the transaction
COMMIT

// Step 12: Verify transaction by selecting order line items
SELECT * FROM orderdetails WHERE orderNumber = orderNumber



## Support for the Sales Departments' Report
To enable customers to purchase products in installments the following entities can be used:

1. Order Entity: Order ID, Customer ID, Order Date, Order Total, Payment Status, Payment History.
2. Payment Entity: Payment ID, Order ID, Payment Amount, Payment Date, Payment Method.
3. Customer Entity: Customer ID, Customer Name, Contact Information.

To differentiate between a fully  paid order and those paid in installments:

1. Join Order and Payment entities on Order ID.
Filter for orders with Payment Status not "Paid in Full."
Calculate remaining balance.
Include customer information.
This design allows for easy generation of the required report.

The above will enable them to follow up with clients who are yet to pay the business the money
they owe.
