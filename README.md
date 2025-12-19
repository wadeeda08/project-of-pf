#include <iostream>
#include <iomanip>
#include <cctype>
#include<string>

using namespace std;
void login();
void mainMenu();
void adminMenu();
void displayMenu();
void addNewItem();
void updateQuantity();
void updatePrice();
void placeOrder();
void generateBill(int soldCounter, string soldItem[], int soldQuantity[], int soldIDs[], int totalBill);                              



struct ITEM {
    string category;
    string name;
    int price;
    int quantity;
};

ITEM items[20] = {
    {"Cone","Vanilla Cone",250,20 },
    {"Cone","Chocolate Cone",260,25},
    {"Cup","Strawberry Cup",180,30},
    {"Cup","Mango Cup",220,25},
    {"Sandae","Oreo Sandae",200,20},
    {"Shake","Chocolate Shake",180,10},
    {"Shake","Strawberry Shake",300,50},
    {"sandae","Oreo Blizzard",140,10},
    {"Kulfi","Pista Kulfi",160,20},
    {"Sandwish","Ice Cream Sandwish",120,30}
};

int currentItems = 10;
int main()
{
    int dailySales = 0;
    int itemBill = 0;
    login();
    mainMenu();
    return 0;
}
                 
                     
void mainMenu()
{
    int choice = 0;
    do
    {
        cout << "1. Admin Menu.\n";
        cout << "2. Place Order.\n";
        cout << "3. Exit.\n";
        cout << "Enter your choice(integer): ";
        cin >> choice;
        switch (choice)
        {
        case 1:
            adminMenu();
            break;
        case 2:
            placeOrder();
            char mainChoice;
            cout << "Do you want to enter main menu?(y/n): ";
            cin >> mainChoice;
            if (tolower(mainChoice) == 'y')
            {
                mainMenu();
            }
            break;
        }
    } while (choice != 3);
}
void displayMenu()
{
    cout << "-----------------------------------------------------------\n";
    cout << "                ICE CREAM PARLOUR MENU                     \n";
    cout << "-----------------------------------------------------------\n";

    cout << left << setw(5) << "ID" << setw(25) << "Item Name"
        << setw(15) << "Category" << setw(10) << "Price(Rs)" << setw(10) << "Quantity" << endl;
    cout << "-----------------------------------------------------------\n";

    for (int i = 0; i < currentItems; i++)
    {
        cout << left << setw(5) << i + 1 << setw(25) << items[i].name
            << setw(15) << items[i].category << setw(10) << items[i].price << setw(10)
            << items[i].quantity << endl;
    }
    cout << "-----------------------------------------------------------\n";
}

void login()
{
    int choice;
    string adminPass;

    cout << "==============================================================================\n";
    cout << "\n                            LOGIN SYSTEM                                    \n";
    cout << "==============================================================================\n";
    cout << "1. Admin Login.\n";
    cout << "2. Coustumer Login.\n";
    cout << "Enter your choice.\n";
    cin >> choice;

    if (choice == 1)
    {
        cout << "Enter Admin Password:";
        cin >> adminPass;
        if (adminPass == "admin123")
        {
            cout << "\nLogin Successful!Wellcome Admin.\n";
            adminMenu();
        }
        else
        {
            cout << "Worng Password!Access denied\n";
        }
    }
    else if (choice == 2)
    {
        cout << "Wellcome Customer!\n";
        placeOrder();
    }
    else
    {
        cout << "Invalid choice!";
    }
}

void adminMenu()
{
    int choice = 0;
    do
    {
        cout << "\n=============ADMIN MENU============\n";
        cout << "1. Add New Items\n";
        cout << "2. Update Quantity\n";
        cout << "3. Change Item Price\n";
        cout << "4. View Stock\n";
        cout << "5. Exit Admin Menu\n";
        cout << "Enter your choice(integer) :";
        cin >> choice;
        switch (choice)
        {
        case 1:
            addNewItem();
            break;
        case 2:
            displayMenu();
            updateQuantity();
            break;
        case 3:
            displayMenu();
            updatePrice();
            break;
        case 4:
            displayMenu();
            break;
        }
    } while (choice != 5);
}

void addNewItem()
{
    cout << "Enter item category: ";
    cin >> items[currentItems].category;
    cout << "Enter item name: ";
    cin >> items[currentItems].name;
    cout << "Enter item price: ";
    cin >> items[currentItems].price;
    cout << "Enter item quantity: ";
    cin >> items[currentItems].quantity;
    currentItems++;
}

void updateQuantity()
{
    int id; int qty;

    cout << "Enter ID to update quantity(integer):";
    cin >> id;

    cout << "Enter quantity to add(integer) :";
    cin >> qty;
    items[id - 1].quantity += qty;

    cout << "Quantity Updated Successfully!\n";
}
void updatePrice()
{
    int id, newPrice;
    cout << "Enter Id to change price(integer) :";
    cin >> id;

    cout << "Enter new price :";
    cin >> newPrice;
    items[id - 1].price = newPrice;
    cout << "Price Updated!\n";
}

void placeOrder()
{
    displayMenu();
    int soldCounter = 0;
    int soldIDs[20] = { 0 };
    int soldQuantity[20] = { 0 };
    string soldItems[20] = { "" };
    int id = 0, quantity = 0;
    int itemBill = 0;
    int totalBill = 0;
    int dailySales = 0;
    char choice = 0;
    do
    {
        cout << "Enter ID you want to  buy: " << endl;
        cin >> id;
        if (id < 1 || id > currentItems)
        {
            cout << "Invalid ID! Please enter ID between 1-10 " << endl;
            continue;
        }

        cout << "Avaiable quanitiy for \"" << items[id - 1].name << "\" is " << items[id - 1].quantity << endl;
        cout << "Enter the quantity of item you want(integer): " << endl;
        cin >> quantity;
        if (quantity <= 0)
        {
            cout << "Invalid Quantity!You can not purchase 0 or less items" << endl;
            continue;
        }

        if (quantity <= items[id - 1].quantity)
        {
            itemBill = quantity * items[id - 1].price;
            totalBill += itemBill;
            dailySales += itemBill;

            items[id - 1].quantity -= quantity;

            soldIDs[soldCounter] = id;
            soldQuantity[soldCounter] = quantity;
            soldItems[soldCounter] = items[id - 1].name;
            soldCounter++;

            cout << "Added to bill:" << itemBill << "Pkr\n";
        }
        else
        {
            cout << "You can not purchase more than " << items[id - 1].quantity << "items " << endl;
            continue;
        }
        cout << "Do you want to buy more items(y/n)?" << endl;
        cin >> choice;
    } while (tolower(choice) == 'y');
    generateBill(soldCounter, soldItems, soldQuantity, soldIDs, totalBill);
}
void generateBill(int soldCounter, string soldItem[], int soldQuantity[], int soldIDs[], int totalBill)
{
    cout << "==============================================\n";
    cout << "             FINAL BILL RECEIPT               \n";
    cout << "==============================================\n";
    cout << left << setw(20) << "Item" << setw(10) << "Quantity" << setw(10) << "Sub Total" << endl;

    for (int i = 0; i < soldCounter; i++)
    {
        cout << left << setw(20) << soldItem[i] << setw(10) << soldQuantity[i] << setw(10) << items[soldIDs[i] - 1].price * soldQuantity[i] << endl;
    }
    cout << "\nTotal bill: " << totalBill << "Pkr\n";
    cout << "==============================================\n";
    cout << "\nThank you for visiting our Ice creamĀ Parlour!\n";
}
