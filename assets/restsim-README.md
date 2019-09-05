# RESTAURANT SOFTWARE (Phase 2)

This program is a tool for restaurants to track: customer orders, the bill at each table inventory levels,
and inventory shipments.

**Contents**  

<details>
<summary><a href="#pre-requisite-files">Pre-requisite Files</a></summary>

  - [menu.txt](#menutxt)
  - [inventory.txt](#inventorytxt)
  - [workers.txt](#workerstxt)
  - [inventory-receipts.txt](#inventory-receiptstxt)
  - [System Files](#system-files)
</details>
<p>

&nbsp;&nbsp;&nbsp;&nbsp;[How To Run](#how-to-run)
<details>
<summary><a href="#features">Features</a></summary>

  - [Manager](#manager-view)
  - [Cook](#cook-view)
  - [Server](#server-view)
  - [Inventory](#inventory-view)
  - [Additional Features](#additional-features)
</details>
<p>

&nbsp;&nbsp;&nbsp;&nbsp;[Additional Information](#additional-information)
<p>

&nbsp;&nbsp;&nbsp;&nbsp;[Group 0217](#group-0217)

## Pre-requisite Files

These are the text files you will need prior to running this program, and will be placed in the project folder. Make sure you use these filenames, otherwise the program may not run properly.

#### menu.txt
A text file detailing all menu items. Use the format:  
```json
[{
  "itemName": "caesar salad",
  "ingredients": {
    "lettuce": 1,
    "croutons": 1,
    ...
    "dressing": 1
  },
  "price": 5.00,
  "numAdditions": 1
  },
  ...
]
```
Element | Description | Type
--- | --- | ---
`itemName` | The name of the menu item. | string
`ingredients` | The ingredients needed for the menu item. | ingredients list object
`ingredients/[ingredientName]` | The quantity of the ingredient needed for the menu item. | integer
`price` | The price of the menu item. | float
`numAdditions` | The number of add-ons that can be included with the menu item. | integer

*All fields are required. At least one ingredient must be defined.*

#### inventory.txt
A text file detailing opening inventory levels at the restaurant and the type of ingredients held
in stock. Make sure to include all ingredients from [menu.txt](#menutxt). Use the format:  
```json
[{
  "ingredientName": "lettuce",
  "minQuantity": 10,
  "currentQuantity": 50
  },
  ...
]
```
Element | Description | Type
--- | --- | ---
`ingredientName` | The name of the ingredient to add to the inventory. | string
`minQuantity` | The minimum amount, in units, of the ingredient the inventory can hold. | integer
`currentQuantity` | The current amount, in units, of the ingredient in the inventory. | integer

*All fields required.*

#### workers.txt
A text file detailing the number of each type of employees at the restaurant as well as tables available. Currently the program supports: Server, Cook, Manager, and Table. Use the format:
```json
{
  "server": 5,
  "cook": 3,
  "manager": 2,
  "table": 10
}
```
Element | Description | Type
--- | --- | ---
`server` | The number of servers working at your restaurant. | integer
`cook` | The number of cooks working in your restaurant's kitchen. | integer
`manager` | The number of managers overseeing your restaurant. | integer
`table` | The number of tables available at your restaurant. | integer

*All fields required.*

#### inventory-receipts.txt
An **optional** text file that is used to update inventory items with the specified amount, in units. Use the format:
```json
[{
  "lettuce": 40,
  "chicken": 20,
  ...
  "cheese": 10
  },
  ...
]
```
Each list item can be viewed as a "batch order" of ingredients.

Element | Description | Type
--- | --- | ---
| A batch order of ingredients for the kitchen inventory. | ingredients list object
`/[ingredientName]` | The quantity of the specified ingredient to add to the inventory. | integer

### System Files

While running, the program may create or update files containing various pieces of information.

**requests.txt**: A text file that is automatically created and updated whenever an ingredient level is below 10 quantities.  
**inventory-listing.txt**: A text file that details the inventory levels at a point in time for the manager to review.  
**open-orders.txt**: A text file that records all open-orders at time of request.  
**same-day-payments**: A folder that stores a summary of all payments by date.

## How To Run

1. Create and populate all of your [pre-requisite files](#pre-requisite-files).
2. Place these files in your project folder.
2. Compile and run `GUISimulation.java`.

## Features
The GUI handles the following actions:

#### Manager View
A manager can:
- View the inventory list
- View a list of all inventory requests
- View a list of all open orders
- Look-up the current quantity of stock for a particular inventory item

#### Cook View
A cook can:
- Accept an order
- Notify servers that an order is ready to deliver to customers

#### Server View
A server can:
- Make a new order:
    - With an option to make adjustments to an order
    - But an order cannot be made if inventory is too low
- Deliver an order
    - Acknowledge that an order has been delivered
    - With option to give the customer a refund if the order was unsatisfactory
- Bill a customer
    - With an option to bill by customer (separate checks)
    - Or, an option to bill by table

#### Inventory View
Regarding the inventory, you can:
- Automatically upload inventory shipment receipts from [inventory-receipts.txt](#inventory-receiptstxt)
- Or, manually enter new inventory items

#### Additional Features
Special features are indicated in bold.

- No standard output
- GUI handles invalid inputs using:
    - Exception handling
    - **Regex notation**
    - Use of appropriate methods
- Orders are handled through the Server view or Cook view.
    - If there aren't enough ingredients in the inventory to make a menu item, a server cannot place that order.
    - If a chef forgets an Order #1, they will not be allowed to prepare an Order #2 until they complete, acknowledge, or
      cancel Order #1, or any other actions you think should be allowed to resolve Order #1.
    - If a server has not yet delivered an order that is ready for delivery, they cannot place another order until it's delivered.
- Managers can request a list of open orders in the Manger view > Open orders.
- Records of all payments for a given day can be found in the folder `same-day-payments/`
- Servers can print a receipt for an entire table or individual person in Server view > Prepare bill.
    - Tax is set at 13%
    - If there are more than 8 individuals, a 15% gratuity charge is automatically set.
    - **A customer can choose a tip amount of 10%, 15%, 20%. They can also input a custom percentage or dollar amount.**
-**Placed orders are automatically assigned to an available chef.**
-**Each employee must enter their ID number to view individual screens.**
-**Inventory shipments can be imported through a text file or manually adjustment in Inventory view.**
-**Upon termination of the window, a dialog displaying restaurant statistics collected during your simulation runtume.**


## Additional Information
**Inventory Receipts**  
This program assumes that when new inventory is received by the restaurant, an employee who is
equipped with a scanner, will scan each inventory receipt. After each session, the data is converted into a batch order and recorded in `inventory-receipts.txt`.

**Orders**  
An *order* represents one single item from the menu (i.e., one cheeseburger). A server should record orders by the table number. For example, if a Party of 2 is seated at Table 1, all orders from that Party are recorded to Table 1.

**Bill Splitting**   
To support the bill splitting function, a Server identifies individual customers at each table. When
it comes time to bill the multiple parties, the orders will have been already grouped and totaled.

**Refunding Customers**  
A server should be informed of the restaurant policies for refunding an order. It is up to the server to grant a refund when the order is delivered to the customer.

**Additions to MenuItem**  
There is a set number of additions that can be made to a given menu item. If the number of additions
exceeds that limit, only the additions up to that point can be honored. E.g.,
```
if limit = 4 and additions = {"tomato": 2, "cheese": 3}
>> then, only {"tomato": 2, "cheese": 2} is added
```

**The price of MenuItem**  
All additions made are +$0.10. Subtractions are not reflected in the price.

**Inventory shipments**  
Ingredients are automatically added to `requests.txt` when they fall below minimum levels. The standard order of an ingredient is 20 units. The manager can freely adjust these quantities when sending a shipment request e-mail.

## Group 0217

**Restaurant Software**  
The University of Toronto  
*CSC 207 Software Design, Winter 2018*  
Chen, Gu, Washington, Zhu
