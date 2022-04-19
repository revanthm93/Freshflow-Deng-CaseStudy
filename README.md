# Freshflow-Deng-CaseStudy

Problem Statement:

Freshflow Data Engineer take-home challenge
Create a simple microservice, that joins multiple tables and outputs a list of dicts
Task
At Freshflow, we calculate how much supermarkets and other grocery retailers should order of every item, every day.

Your task is to output a list of orders (per item, per day) by joining various tables together from this SQLite database that contains information about items, inventory, what’s orderable that day, and how much we think they will sell of each item in a specific day.

Once you’ve done that, create a simple microservice that returns such information as described below.

Output
The output of your microservice should be a list of dicts, in JSON.

Each dict represents an order, for an item, for a day.

An order is a collection of information about the item, when it can be ordered, when it will be delivered, what’s the suggested retail price, what’s the profit margin, the purchase price, in which categories this item is in, any labels, how much there’s in a case of this stuff, how much they should order, and how much do they have in the inventory.

Here’s how this list of dicts should look like:

[
  {  # an order
    'item_number': int,  # uniquely identifies the item
    'ordering_day': datetime.date,  # the day this item can be ordered
    'delivery_day': datetime.date,  # the day this item will be delivered
    'sales_price_suggestion': float,  # supplier's suggested retail price per unit
    'profit_margin': float,  # profit margin at the sales price suggestion
    'purchase_price': float,  # supplier official purchase price per unit
    'item_categories': List<str>,  # any of the categories the item is in
    'labels': List<str>,  # a list that can contain any of `new`, `on_sale` and `price_change`, plus all categories extracted from the item name
    'case': {
      'quantity': float,  # represents how many items (in `case.unit`) a case contains
      'unit': str  # the unit of the `case.quantity`
    },
    'order': {
      'quantity': int,  # represents how many items (in `order.unit`) the user should order given the formula below (look below)
      'unit': str  # the unit of the `order.quantity` (it's always 'CS')
    },
    'inventory': {
      'quantity': float,  # the inventory quantity of the item that day
      'unit': str  # the unit of that quantity
    }
  },  # end of order
  {...}, # another order
  ...  # etcetera
]
The order.quantity is especially important as it is how much the supermarket should order and it is defined as

The sales quantity prediction at delivery_day minus the inventory quantity at ordering_day divided by the amount of items in a case (case_content_quantity).
You’ll have to piece together all this information so please take a look at the description of the input tables below.

Recap
You’re given a SQLite database as input. You can check out a description of the tables below.
Your output should match this.
You’re free to use any web framework of your choice (e.g. Flask, FastAPI, etc.)
The GET / method should return a list of order dictionaries, as described in the output section. Each dictionary represents an order for a day. The list should contain all orders of all items (represented by item_number) for all days (represented by ordering_day).
Requirements
Please use SQL to join the tables, not Python
The post-processing steps (if any) can be done in Python
Please use version control, e.g. git or other
Please submit either a link to your version controlled repository or a zip file of your project containing the necessary version control information.
Please spend a maximum of 3 hours for this take home task
You can use third party libraries
Include any notes, assumptions, or further instructions in the README
Criteria
You’ll be evaluated on:

Correctness of the output (whether it matches its types and description)
Performance
Code Quality and Architecture (how maintainable, how testable, etc.)
Project structure (folders and files)
Git history
Comments and documentation
Hints
You can do a lot in the SQL query. Start from the orderable_items table and LEFT JOIN from there.
Focus on the post-processing step (e.g. making some dict be the child of some other dict) after your SQL query looks alright.
Only create the microservice after the first two steps.
Tables
This is a description of the tables in the SQLite database in input:

inventory
How much the supermarket has in stock each day.

item_number: uniquely identifies the item
day: identifies the day when the inventory count was performed
inventory: the quantity of the item that day
order_intake
How and which items the supermarket received every morning.

item_number: uniquely identifies the item
day: identifies the day when the order intake happened
purchase_price: how much that quantity costed in Euro
quantity: the amount of items received
unit: the unit of the quantity
sales_predictions
What we think the demand will be for each item.

item_number: uniquely identifies the item
day: identifies the day when the sales will happen
sales_quantity: the amount of items we think will be sold
orderable_items
Items available to order at particular day with date of delivery.

item_number: uniquely identifies the item
delivery_day: the day this item will be delivered if ordered at ordering_day
ordering_day: the day this item can be ordered
index: the item’s position in that day’s order proposal (useful for sorting in the app)
purchase_price: supplier official purchase price per case_content_unit
suggested_retail_price: supplier’s suggested retail price per case_content_unit
profit_margin at the suggested_retail_price
tags can contain any of new, on_sale and price_change
is_bio represents if the product is Bio
product_line contains the name of the product line
item_categories contains any of the categories the item is in
extra_categories are categories extracted from the item name
case_content_quantity represents how many items (in case_content_unit) a case contains
case_content_unit: the unit of the case_content_quantity
Diagram
The diagram below shows you the relationships between the tables.
diagram

This task is a simplified version of a problem we already solved in the past at Freshflow.
