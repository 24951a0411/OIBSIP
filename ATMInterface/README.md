import java.util.ArrayList;
import java.util.Scanner;

class User {
    private String userId;
    private String pin;

    public User(String userId, String pin) {
        this.userId = userId;
        this.pin = pin;
    }

    public String getUserId() {
        return userId;
    }

    public boolean validatePin(String inputPin) {
        return this.pin.equals(inputPin);
    }
}

class Transaction {
    private String type;
    private double amount;

    public Transaction(String type, double amount) {
        this.type = type;
        this.amount = amount;
    }

    public String toString() {
        return type + ": " + amount;
    }
}

class Account {
    private double balance;
    private ArrayList<Transaction> history;

    public Account(double initialBalance) {
        this.balance = initialBalance;
        this.history = new ArrayList<>();
    }

    public double getBalance() {
        return balance;
    }

    public void deposit(double amount) {
        balance += amount;
        history.add(new Transaction("Deposit", amount));
    }

    public boolean withdraw(double amount) {
        if (amount <= balance) {
            balance -= amount;
            history.add(new Transaction("Withdraw", amount));
            return true;
        }
        return false;
    }

    public boolean transfer(Account receiver, double amount) {
        if (amount <= balance) {
            balance -= amount;
            receiver.deposit(amount);
            history.add(new Transaction("Transfer to " + receiver.hashCode(), amount));
            return true;
        }
        return false;
    }

    public void printTransactionHistory() {
        if (history.isEmpty()) {
            System.out.println("No transactions found.");
        } else {
            for (Transaction t : history) {
                System.out.println(t);
            }
        }
    }
}

class ATM {
    private User user;
    private Account account;
    private Scanner scanner;

    public ATM(User user, Account account) {
        this.user = user;
        this.account = account;
        this.scanner = new Scanner(System.in);
    }

    public boolean login() {
        System.out.print("Enter User ID: ");
        String inputId = scanner.nextLine();
        System.out.print("Enter PIN: ");
        String inputPin = scanner.nextLine();

        if (user.getUserId().equals(inputId) && user.validatePin(inputPin)) {
            System.out.println("Login successful!\n");
            return true;
        } else {
            System.out.println("Incorrect User ID or PIN.");
            return false;
        }
    }

    public void showMenu() {
        int choice;
        do {
            System.out.println("\nATM Menu:");
            System.out.println("1. Transaction History");
            System.out.println("2. Withdraw");
            System.out.println("3. Deposit");
            System.out.println("4. Transfer");
            System.out.println("5. Quit");
            System.out.print("Select an option: ");
            choice = scanner.nextInt();

            switch (choice) {
                case 1:
                    System.out.println("Transaction History:");
                    account.printTransactionHistory();
                    break;
                case 2:
                    System.out.print("Enter amount to withdraw: ");
                    double withdrawAmount = scanner.nextDouble();
                    if (account.withdraw(withdrawAmount)) {
                        System.out.println("Withdraw successful!");
                    } else {
                        System.out.println("Insufficient balance.");
                    }
                    break;
                case 3:
                    System.out.print("Enter amount to deposit: ");
                    double depositAmount = scanner.nextDouble();
                    account.deposit(depositAmount);
                    System.out.println("Deposit successful!");
                    break;
                case 4:
                    System.out.print("Enter amount to transfer: ");
                    double transferAmount = scanner.nextDouble();
                    Account receiver = new Account(0); // dummy account for transfer
                    if (account.transfer(receiver, transferAmount)) {
                        System.out.println("Transfer successful!");
                    } else {
                        System.out.println("Insufficient balance.");
                    }
                    break;
                case 5:
                    System.out.println("Exiting. Thank you for using our ATM.");
                    break;
                default:
                    System.out.println("Invalid option. Try again.");
            }
        } while (choice != 5);

    }
}


public class Main {
    public static void main(String[] args) {
        User user = new User("user123", "1234");
        Account account = new Account(1000);

        ATM atm = new ATM(user, account);

        if (atm.login()) {
            atm.showMenu();
        }
    }
}
