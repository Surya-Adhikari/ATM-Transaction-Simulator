import java.util.Scanner;

public class Main {
    private double balance; // Variable to store the account balance
    private int pin; // Variable to store the user's PIN
    private String[] history = new String[5]; // Array to store transaction history
    private int historyCount = 0; // Counter for the number of transactions in history

    // Constructor to initialize the ATM with a PIN and initial balance
    public Main(int initialPin, double initialBalance) {
        this.pin = initialPin;
        this.balance = initialBalance;
    }

    // Method to validate the entered PIN
    public boolean validatePIN(int inputPin) {
        return this.pin == inputPin; // Returns true if the input PIN matches the stored PIN
    }

    // Method to change the user's PIN
    public void changePIN(int newPin) {
        this.pin = newPin; // Update the stored PIN
        System.out.println("PIN successfully changed.");
        addHistory("PIN changed."); // Log the PIN change in transaction history
    }

    // Method to check the current balance
    public void checkBalance() {
        System.out.printf("Your current balance is: %.2f\n", balance); // Display the balance
        addHistory("Checked balance."); // Log the balance check in transaction history
    }

    // Method to withdraw cash from the account
    public void withdraw(double amount) {
        if (amount <= 0) {
            System.out.println("Invalid withdrawal amount."); // Check for valid amount
        } else if (amount > balance) {
            System.out.println("Insufficient balance."); // Check for sufficient balance
        } else {
            balance -= amount; // Deduct the amount from the balance
            System.out.printf("%.2f withdrawn successfully.\n", amount); // Confirm withdrawal
            addHistory("Withdrew: " + amount); // Log the withdrawal in transaction history
        }
    }

    // Method to deposit cash into the account
    public void deposit(double amount) {
        if (amount <= 0) {
            System.out.println("Invalid deposit amount."); // Check for valid amount
        } else {
            balance += amount; // Add the amount to the balance
            System.out.printf("%.2f deposited successfully.\n", amount); // Confirm deposit
            addHistory("Deposited: " + amount); // Log the deposit in transaction history
        }
    }

    // Method to view the transaction history
    public void viewHistory() {
        System.out.println("Transaction History:");
        for (int i = 0; i < historyCount; i++) {
            System.out.println(history[i]); // Print each transaction in history
        }
    }

    // Method to add a transaction to the history
    private void addHistory(String transaction) {
        if (historyCount < 5) {
            history[historyCount++] = transaction; // Add transaction if there's space
        } else {
            // Shift history left to make room for new transaction
            for (int i = 1; i < history.length; i++) {
                history[i - 1] = history[i]; // Shift each transaction to the left
            }
            history[history.length - 1] = transaction; // Add the new transaction at the end
        }
    }

    // Main method to run the ATM program
    public static void main(String[] args) {
        try (Scanner scanner = new Scanner(System.in)) {
            Main atm = new Main(1234, 1000.0); // Create an ATM instance with a default PIN and balance

            System.out.println("Welcome to the ATM!");
            System.out.print("Enter your PIN: ");
            int enteredPin = scanner.nextInt(); // Read the entered PIN

            // Validate the entered PIN
            if (atm.validatePIN(enteredPin)) {
                int choice; // Variable to store the user's menu choice
                do {
                    // Display the menu options
                    System.out.println("\nMenu:");
                    System.out.println("1. Check Balance");
                    System.out.println("2. Withdraw Cash");
                    System.out.println("3. Deposit Cash");
                    System.out.println("4. Change PIN");
                    System.out.println("5. View Transaction History");
                    System.out.println("6. Exit");
                    System.out.print("Choose an option: ");
                    choice = scanner.nextInt(); // Read the user's choice

                    // Handle the user's choice
                    switch (choice) {
                        case 1:
                            atm.checkBalance(); // Check balance
                            break;
                        case 2:
                            System.out.print("Enter amount to withdraw: ");
                            double withdrawAmount = scanner.nextDouble(); // Read withdrawal amount
                            atm.withdraw(withdrawAmount); // Withdraw cash
                            break;
                        case 3:
                            System.out.print("Enter amount to deposit: ");
                            double depositAmount = scanner.nextDouble(); // Read deposit amount
                            atm.deposit(depositAmount); // Deposit cash
                            break;
                        case 4:
                            System.out.print("Enter new PIN: ");
                            int newPin = scanner.nextInt(); // Read new PIN
                            atm.changePIN(newPin); // Change the PIN
                            break;
                        case 5:
                            atm.viewHistory(); // View transaction history
                            break;
                        case 6:
                            System.out.println("Thank you for using the ATM!"); // Exit message
                            break;
                        default:
                            System.out.println("Invalid choice. Please try again."); // Handle invalid choice
                    }
                } while (choice != 6); // Repeat until the user chooses to exit
            } else {
                System.out.println("Invalid PIN. Access denied."); // Handle invalid PIN
            }
        } catch (Exception e) {
            System.out.println("An error occurred: " + e.getMessage()); // Handle exceptions
        }
    }
}
