#include <iostream>
#include <vector>
#include <string>
using namespace std;

struct Product {
    int id;
    string name;
    double price;
    int stock;
};

struct CartItem {
    Product product;
    int quantity;
};

class Store {
private:
    vector<Product> products;
    vector<CartItem> cart;

public:
    Store() {
        // Adding some sample products
        products.push_back({1, "Apple", 0.5, 100});
        products.push_back({2, "Banana", 0.3, 150});
        products.push_back({3, "Orange", 0.7, 120});
    }

    void showProducts() {
        cout << "Available Products:\n";
        for (const auto& p : products) {
            cout << p.id << ". " << p.name << " - $" << p.price << " - Stock: " << p.stock << endl;
        }
    }

    void addToCart(int productId, int quantity) {
        for (auto& p : products) {
            if (p.id == productId) {
                if (p.stock >= quantity) {
                    p.stock -= quantity;
                    bool found = false;
                    for (auto& item : cart) {
                        if (item.product.id == productId) {
                            item.quantity += quantity;
                            found = true;
                            break;
                        }
                    }
                    if (!found) {
                        cart.push_back({p, quantity});
                    }
                    cout << quantity << " " << p.name << "(s) added to cart.\n";
                } else {
                    cout << "Sorry, not enough stock.\n";
                }
                return;
            }
        }
        cout << "Product not found.\n";
    }

    void showCart() {
        if (cart.empty()) {
            cout << "Cart is empty.\n";
            return;
        }
        cout << "Your Cart:\n";
        double total = 0;
        for (const auto& item : cart) {
            double itemTotal = item.product.price * item.quantity;
            cout << item.product.name << " x" << item.quantity << " = $" << itemTotal << endl;
            total += itemTotal;
        }
        cout << "Total: $" << total << endl;
    }
};

int main() {
    Store store;
    int choice;

    do {
        cout << "\n--- Nasim Store ---\n";
        cout << "1. Show Products\n";
        cout << "2. Add to Cart\n";
        cout << "3. Show Cart\n";
        cout << "0. Exit\n";
        cout << "Choose an option: ";
        cin >> choice;

        switch (choice) {
            case 1:
                store.showProducts();
                break;
            case 2: {
                int id, qty;
                cout << "Enter product ID to add: ";
                cin >> id;
                cout << "Enter quantity: ";
                cin >> qty;
                store.addToCart(id, qty);
                break;
            }
            case 3:
                store.showCart();
                break;
            case 0:
                cout << "Thank you for visiting Nasim Store!\n";
                break;
            default:
                cout << "Invalid option. Try again.\n";
        }
    } while (choice != 0);

    return 0;
}
