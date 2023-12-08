# python-challenge-1_revision
This is the revised version of python-challenge-1
# Menu dictionary
menu = {
    "Snacks": {
        "Cookie": .99,
        "Banana": .69,
        "Apple": .49,
        "Granola bar": 1.99
    },
    "Meals": {
        "Burrito": 4.49,
        "Teriyaki Chicken": 9.99,
        "Sushi": 7.49,
        "Pad Thai": 6.99,
        "Pizza": {
            "Cheese": 8.99,
            "Pepperoni": 10.99,
            "Vegetarian": 9.99
        },
        "Burger": {
            "Chicken": 7.49,
            "Beef": 8.49
        }
    },
    "Drinks": {
        "Soda": {
            "Small": 1.99,
            "Medium": 2.49,
            "Large": 2.99
        },
        "Tea": {
            "Green": 2.49,
            "Thai iced": 3.99,
            "Irish breakfast": 2.49
        },
        "Coffee": {
            "Espresso": 2.99,
            "Flat white": 2.99,
            "Iced": 3.49
        }
    },
    "Dessert": {
        "Chocolate lava cake": 10.99,
        "Cheesecake": {
            "New York": 4.99,
            "Strawberry": 6.49
        },
        "Australian Pavlova": 9.99,
        "Rice pudding": 4.99,
        "Fried banana": 4.49
    }
}

# 1. Set up order list. Order list will store a list of dictionaries for
# menu item name, item price, and quantity ordered
#Below was converted to list for second submission --  December 7, 2023
customer_order = []

selection_count = 0

# Launch the store and present a greeting to the customer
print("Welcome to the variety food truck.")

# Customers may want to order multiple items, so let's create a continuous
# loop
place_order = True
while place_order:
    # Ask the customer from which menu category they want to order
    print("From which menu would you like to order? ")

    # Create a variable for the menu item number
    i = 1
    # Create a dictionary to store the menu for later retrieval
    menu_items = {}

    # Print the options to choose from menu headings (all the first level
    # dictionary items in menu).
    for key in menu.keys():
        print(f"{i}: {key}")
        # Store the menu category associated with its menu item number
        menu_items[i] = key
        # Add 1 to the menu item number
        i += 1

    # Get the customer's input
    menu_category = input("Type menu number: ")

    # Check if the customer's input is a number
    if menu_category.isdigit():
        # Check if the customer's input is a valid option
        if int(menu_category) in menu_items.keys():
            # Save the menu category name to a variable
            menu_category_item = menu_items[int(menu_category)]
            # Print out the menu category name they selected
            print(f"You selected {menu_category_item}")

            # Print out the menu options from the menu_category_name
            print(f"What {menu_category_item} item would you like to order?")
            i = 1
            menu_items = {}
            print("Item # | Item name                | Price")
            print("-------|--------------------------|-------")
            for key, value in menu[menu_category_item].items():
                # Check if the menu item is a dictionary to handle differently
                if type(value) is dict:
                    for key2, value2 in value.items():
                        num_item_spaces = 24 - len(key + key2) - 3
                        item_spaces = " " * num_item_spaces
                        print(f"{i}      | {key} - {key2}{item_spaces} | ${value2}")
                        menu_items[i] = {
                            "Item name": key + " - " + key2,
                            "Price": value2
                        }
                        i += 1
                else:
                    num_item_spaces = 24 - len(key)
                    item_spaces = " " * num_item_spaces
                    print(f"{i}      | {key}{item_spaces} | ${value}")
                    menu_items[i] = {
                        "Item name": key,
                        "Price": value
                    }
                    i += 1
           # 2. Ask customer to input menu item number
            menu_selection = input("Please enter the item number you would like to order: ")

            # 3. Check if the customer typed a number
            if menu_selection.isdigit():
                # Convert the menu selection to an integer
                menu_selection = int(menu_selection)
                
                # 4. Check if the menu selection is in the menu items
                if menu_selection in menu_items.keys():
                    # Store the item name as a variable
                    menu_selection_name = menu_items[menu_selection]["Item name"]
                    menu_selection_price = menu_items[menu_selection]["Price"]

                    # Ask the customer for the quantity of the menu item
                    quantity = input(f"You selected '{menu_selection_name}', How many would you like to order? ")

                    # Check if the quantity is a number, default to 1 if not
                    if quantity.isdigit():
                        menu_selection_quantity_int = int(quantity)
                    else:
                        menu_selection_quantity_int = 1

                    # Add the item name, price, and quantity to the order list
                    selection_count += 1
                    
                    #December 7, 2023 edit
                    customer_order.append( {"Item name": menu_selection_name,
                                                    "Price": menu_selection_price,
                                                    "Quantity": menu_selection_quantity_int
                                                    })
                else:
                    # Tell the customer that their input isn't valid
                    print(f"'{menu_selection}' is not valid. Try again.")
            else:
                # Tell the customer they didn't select a menu option
                print(f"'{menu_selection}' is not a menu option. Try again.")


        else:
            # Tell the customer they didn't select a menu option
            print(f"'{menu_category}' is not a menu option. Try again.")
    else:
        # Tell the customer they didn't select a number
        print("You didn't select a number.  Try again")

    while True:
        # Ask the customer if they would like to order anything else
        keep_ordering = input("Would you like to keep ordering? 'y' is for Yes 'n' is for No ")

        # 5. Check the customer's input
        match keep_ordering.lower():
            case 'y':
                # Keep ordering
                # Exit the keep ordering question loop
                place_order = True
                break
            case 'n':
                # Complete the order
                # Since the customer decided to stop ordering, thank them for
                # their order
                # Exit the keep ordering question loop
                place_order = False
                #Additional check to confirm that customer ordered one or more items
                if selection_count > 0:
                    print("Thank You.")
                else:
                    print("Visit us in the future.")
                break
            case _:
                # Tell the customer to try again
                print("'y' for Yes or 'n' for no required)")

if selection_count > 0:
    # Exit the keep ordering question loop
    # Print out the customer's order
    print("Your order.")

    print("Item name                 | Price  | Quantity")
    print("--------------------------|--------|----------")

    # 6. Loop through the order/dictionary in the customer's order
for order in customer_order:
    # Store the dictionary items as variables
    price = order['Price']
    quantity = order['Quantity']
    item_name = order['Item name']

    # Calculate the number of spaces for formatted printing
    num_item_spaces = 25 - len(item_name)

    # Create space strings
    item_spaces = " " * num_item_spaces

    # Print the item name, price, and quantity
    print(f"{item_name}{item_spaces} | ${price:5.2f} | {quantity:2}")

    # 11. Calculate the cost of the order using list comprehension
   
    order_item_total_prices = [order['Price'] * order['Quantity'] for order in customer_order]

 # Print Final Receipt with Total Price
print("Your order is ready.")
print("Receipt")
print("Item name                 | Price   | Quantity | Item Total ")
print("--------------------------|---------|----------|------------")

for order, total_price in zip(customer_order, order_item_total_prices):
    item_name = order['Item name']
    price = order['Price']
    quantity = order['Quantity']
    print("{:25} | ${:6.2f} |    {:3}   | ${:7.2f}".format(item_name, price, quantity, total_price))

# Calculate Total Order Price
order_total_price = sum(order_item_total_prices)

# Print Total Order Price
dashed_line = "-" * 60
print(f"{dashed_line}")
item_spaces = " " * 34
print(f"{item_spaces} Total Price   ${order_total_price:7.2f}")

# Create Dashed line and Print Customer Thank you
dashed_line = "-" * 60
print(f"{dashed_line}")
print("Thank You.")
