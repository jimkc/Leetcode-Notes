### Level 1: Basic Bank Operations
Problem Description:  
Design a simplified bank system that processes queries. Each query is in the format: [action, timestamp, amount (optional), from account (optional), to account (optional)]

Implement the following actions:  
* CREATE_ACCOUNT: Creates a new bank account.
* DEPOSIT: Adds a specified amount to an existing account.
* TRANSFER: Transfers a specified amount from one account to another.

Consider these edge cases for TRANSFER:  
* Insufficient Funds: If the from account does not have enough balance, the transfer should fail.
* Self-Transfer: A transfer from an account to itself should not be allowed.
* Invalid Accounts: If either the from account or to account does not exist, the transfer should fail.
* Invalid Amount: Amounts for deposit and transfer must be positive.

```java
import java.util.HashMap;
import java.util.Map;

// Account class to hold account details
class Account {
    String accountId;
    double balance;

    public Account(String accountId) {
        this.accountId = accountId;
        this.balance = 0.0; // New accounts start with 0 balance
    }

    public String getAccountId() {
        return accountId;
    }

    public double getBalance() {
        return balance;
    }

    public void deposit(double amount) {
        if (amount > 0) {
            this.balance += amount;
        }
    }

    public boolean withdraw(double amount) {
        if (amount > 0 && this.balance >= amount) {
            this.balance -= amount;
            return true;
        }
        return false; // Insufficient funds or invalid amount
    }

    @Override
    public String toString() {
        return "Account{" +
               "accountId='" + accountId + '\'' +
               ", balance=" + balance +
               '}';
    }
}

// BankSystem class to manage bank operations
public class BankSystem {
    private Map<String, Account> accounts; // Stores accounts by accountId

    public BankSystem() {
        this.accounts = new HashMap<>();
    }

    /**
     * Creates a new bank account.
     * @param accountId The ID for the new account.
     * @return true if the account was created successfully, false if an account with this ID already exists.
     */
    public boolean createAccount(String accountId) {
        if (accounts.containsKey(accountId)) {
            System.out.println("Error: Account '" + accountId + "' already exists.");
            return false;
        }
        accounts.put(accountId, new Account(accountId));
        System.out.println("Account '" + accountId + "' created successfully.");
        return true;
    }

    /**
     * Deposits an amount into a specified account.
     * @param accountId The ID of the account to deposit into.
     * @param amount The amount to deposit. Must be positive.
     * @return true if the deposit was successful, false otherwise.
     */
    public boolean deposit(String accountId, double amount) {
        if (amount <= 0) {
            System.out.println("Error: Deposit amount must be positive.");
            return false;
        }
        Account account = accounts.get(accountId);
        if (account == null) {
            System.out.println("Error: Account '" + accountId + "' not found for deposit.");
            return false;
        }
        account.deposit(amount);
        System.out.println("Deposited " + amount + " into account '" + accountId + "'. New balance: " + account.getBalance());
        return true;
    }

    /**
     * Transfers an amount from one account to another.
     * @param fromAccountId The ID of the account to transfer from.
     * @param toAccountId The ID of the account to transfer to.
     * @param amount The amount to transfer. Must be positive.
     * @return true if the transfer was successful, false otherwise.
     */
    public boolean transfer(String fromAccountId, String toAccountId, double amount) {
        if (amount <= 0) {
            System.out.println("Error: Transfer amount must be positive.");
            return false;
        }
        if (fromAccountId.equals(toAccountId)) {
            System.out.println("Error: Cannot transfer to the same account.");
            return false;
        }

        Account fromAccount = accounts.get(fromAccountId);
        Account toAccount = accounts.get(toAccountId);

        if (fromAccount == null) {
            System.out.println("Error: 'From' account '" + fromAccountId + "' not found.");
            return false;
        }
        if (toAccount == null) {
            System.out.println("Error: 'To' account '" + toAccountId + "' not found.");
            return false;
        }

        if (!fromAccount.withdraw(amount)) {
            System.out.println("Error: Insufficient funds in account '" + fromAccountId + "' for transfer.");
            return false;
        }

        toAccount.deposit(amount);
        System.out.println("Transferred " + amount + " from '" + fromAccountId + "' to '" + toAccountId + "'.");
        System.out.println("New balance for '" + fromAccountId + "': " + fromAccount.getBalance());
        System.out.println("New balance for '" + toAccountId + "': " + toAccount.getBalance());
        return true;
    }

    // Helper method to get account balance (for testing/verification)
    public double getAccountBalance(String accountId) {
        Account account = accounts.get(accountId);
        return account != null ? account.getBalance() : -1.0; // -1.0 to indicate account not found
    }

    public static void main(String[] args) {
        BankSystem bank = new BankSystem();

        System.out.println("\n--- Testing CREATE_ACCOUNT ---");
        bank.createAccount("ACC001");
        bank.createAccount("ACC002");
        bank.createAccount("ACC003");
        bank.createAccount("ACC001"); // Attempt to create duplicate

        System.out.println("\n--- Testing DEPOSIT ---");
        bank.deposit("ACC001", 1000.0);
        bank.deposit("ACC002", 500.0);
        bank.deposit("ACC004", 200.0); // Deposit to non-existent account
        bank.deposit("ACC001", -50.0); // Invalid deposit amount

        System.out.println("\n--- Testing TRANSFER ---");
        bank.transfer("ACC001", "ACC002", 200.0); // Successful transfer
        bank.transfer("ACC002", "ACC001", 800.0); // Insufficient funds
        bank.transfer("ACC001", "ACC001", 100.0); // Self-transfer
        bank.transfer("ACC001", "ACC004", 50.0); // Transfer to non-existent account
        bank.transfer("ACC004", "ACC001", 50.0); // Transfer from non-existent account
        bank.transfer("ACC002", "ACC003", -100.0); // Invalid transfer amount

        // Verify balances after operations
        System.out.println("\n--- Final Balances ---");
        System.out.println("Balance for ACC001: " + bank.getAccountBalance("ACC001")); // Expected: 800.0
        System.out.println("Balance for ACC002: " + bank.getAccountBalance("ACC002")); // Expected: 300.0
        System.out.println("Balance for ACC003: " + bank.getAccountBalance("ACC003")); // Expected: 0.0
    }
}
```

### Level 2: Top N Most Active Users
Problem Description:  
On top of Level 1 functionalities, add a new feature:
* GET_TOP_N_ACTIVE_USERS: Find the top N users who have made the most outbound TRANSFER transaction

```java
import java.util.HashMap;
import java.util.Map;
import java.util.List;
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;

// Account class (MODIFIED for Level 2: added transactionOutCount)
class Account {
    String accountId;
    double balance;
    int transactionOutCount; // New field for Level 2

    public Account(String accountId) {
        this.accountId = accountId;
        this.balance = 0.0;
        this.transactionOutCount = 0; // Initialize for Level 2
    }

    public String getAccountId() {
        return accountId;
    }

    public double getBalance() {
        return balance;
    }

    // New getter for Level 2
    public int getTransactionOutCount() {
        return transactionOutCount;
    }

    public void deposit(double amount) {
        if (amount > 0) {
            this.balance += amount;
        }
    }

    public boolean withdraw(double amount) {
        if (amount > 0 && this.balance >= amount) {
            this.balance -= amount;
            return true;
        }
        return false;
    }

    // New method for Level 2
    public void incrementTransactionOutCount() {
        this.transactionOutCount++;
    }

    @Override
    public String toString() {
        return "Account{" +
               "accountId='" + accountId + '\'' +
               ", balance=" + balance +
               ", transactionOutCount=" + transactionOutCount + // Include in toString for debugging
               '}';
    }
}

// BankSystem class (MODIFIED for Level 2: added getTopNActiveUsers)
public class BankSystem {
    private Map<String, Account> accounts;

    public BankSystem() {
        this.accounts = new HashMap<>();
    }

    public boolean createAccount(String accountId) {
        if (accounts.containsKey(accountId)) {
            System.out.println("Error: Account '" + accountId + "' already exists.");
            return false;
        }
        accounts.put(accountId, new Account(accountId));
        System.out.println("Account '" + accountId + "' created successfully.");
        return true;
    }

    public boolean deposit(String accountId, double amount) {
        if (amount <= 0) {
            System.out.println("Error: Deposit amount must be positive.");
            return false;
        }
        Account account = accounts.get(accountId);
        if (account == null) {
            System.out.println("Error: Account '" + accountId + "' not found for deposit.");
            return false;
        }
        account.deposit(amount);
        System.out.println("Deposited " + amount + " into account '" + accountId + "'. New balance: " + account.getBalance());
        return true;
    }

    // transfer method (MODIFIED for Level 2: increment transactionOutCount)
    public boolean transfer(String fromAccountId, String toAccountId, double amount) {
        if (amount <= 0) {
            System.out.println("Error: Transfer amount must be positive.");
            return false;
        }
        if (fromAccountId.equals(toAccountId)) {
            System.out.println("Error: Cannot transfer to the same account.");
            return false;
        }

        Account fromAccount = accounts.get(fromAccountId);
        Account toAccount = accounts.get(toAccountId);

        if (fromAccount == null) {
            System.out.println("Error: 'From' account '" + fromAccountId + "' not found.");
            return false;
        }
        if (toAccount == null) {
            System.out.println("Error: 'To' account '" + toAccountId + "' not found.");
            return false;
        }

        if (!fromAccount.withdraw(amount)) {
            System.out.println("Error: Insufficient funds in account '" + fromAccountId + "' for transfer.");
            return false;
        }

        toAccount.deposit(amount);
        fromAccount.incrementTransactionOutCount(); // Level 2: Increment count for 'from' account
        System.out.println("Transferred " + amount + " from '" + fromAccountId + "' to '" + toAccountId + "'.");
        System.out.println("New balance for '" + fromAccountId + "': " + fromAccount.getBalance());
        System.out.println("New balance for '" + toAccountId + "': " + toAccount.getBalance());
        return true;
    }

    /**
     * Returns a list of top N most active users based on outbound transfers.
     * @param n The number of top users to retrieve.
     * @return A list of Account objects, sorted by transactionOutCount in descending order.
     */
    public List<Account> getTopNActiveUsers(int n) {
        if (n <= 0) {
            System.out.println("Error: N must be a positive integer.");
            return Collections.emptyList();
        }

        List<Account> allAccounts = new ArrayList<>(accounts.values());

        // Sort accounts by transactionOutCount in descending order
        Collections.sort(allAccounts, new Comparator<Account>() {
            @Override
            public int compare(Account a1, Account a2) {
                return Integer.compare(a2.getTransactionOutCount(), a1.getTransactionOutCount());
            }
        });

        // Return the top N accounts
        return allAccounts.subList(0, Math.min(n, allAccounts.size()));
    }

    public double getAccountBalance(String accountId) {
        Account account = accounts.get(accountId);
        return account != null ? account.getBalance() : -1.0;
    }

    public static void main(String[] args) {
        BankSystem bank = new BankSystem();

        System.out.println("\n--- Level 1 Operations (repeated) ---");
        bank.createAccount("ACC001");
        bank.createAccount("ACC002");
        bank.createAccount("ACC003");
        bank.createAccount("ACC004"); // Added for more transfers
        bank.createAccount("ACC005"); // Added for more transfers

        bank.deposit("ACC001", 1000.0);
        bank.deposit("ACC002", 500.0);
        bank.deposit("ACC003", 200.0);
        bank.deposit("ACC004", 100.0);
        bank.deposit("ACC005", 50.0);

        bank.transfer("ACC001", "ACC002", 100.0); // ACC001: 1 transfer
        bank.transfer("ACC001", "ACC003", 50.0);  // ACC001: 2 transfers
        bank.transfer("ACC002", "ACC004", 200.0); // ACC002: 1 transfer
        bank.transfer("ACC003", "ACC005", 100.0); // ACC003: 1 transfer
        bank.transfer("ACC001", "ACC005", 20.0);  // ACC001: 3 transfers
        bank.transfer("ACC002", "ACC001", 30.0);  // ACC002: 2 transfers

        System.out.println("\n--- Level 2: Testing GET_TOP_N_ACTIVE_USERS ---");
        int n = 3;
        List<Account> topUsers = bank.getTopNActiveUsers(n);
        System.out.println("Top " + n + " Most Active Users (by outbound transfers):");
        if (topUsers.isEmpty()) {
            System.out.println("No active users found or N is invalid.");
        } else {
            for (Account account : topUsers) {
                System.out.println("  - " + account.getAccountId() + " (Transfers: " + account.getTransactionOutCount() + ")");
            }
        }

        System.out.println("\n--- Final Balances ---");
        System.out.println("Balance for ACC001: " + bank.getAccountBalance("ACC001"));
        System.out.println("Balance for ACC002: " + bank.getAccountBalance("ACC002"));
        System.out.println("Balance for ACC003: " + bank.getAccountBalance("ACC003"));
        System.out.println("Balance for ACC004: " + bank.getAccountBalance("ACC004"));
        System.out.println("Balance for ACC005: " + bank.getAccountBalance("ACC005"));

        System.out.println("\n--- Check individual transaction counts (for verification) ---");
        System.out.println("ACC001 transaction out count: " + bank.accounts.get("ACC001").getTransactionOutCount()); // Expected: 3
        System.out.println("ACC002 transaction out count: " + bank.accounts.get("ACC002").getTransactionOutCount()); // Expected: 2
        System.out.println("ACC003 transaction out count: " + bank.accounts.get("ACC003").getTransactionOutCount()); // Expected: 1
        System.out.println("ACC004 transaction out count: " + bank.accounts.get("ACC004").getTransactionOutCount()); // Expected: 0
        System.out.println("ACC005 transaction out count: " + bank.accounts.get("ACC005").getTransactionOutCount()); // Expected: 0
    }
}
```

### Level 3: Pay and Cashback + Level 4 merge accounts
Problem Description:  
On top of Level 1 and Level 2 functionalities, add a new action:
* PAY: Deducts a specified amount from an account.
* Each query now includes an int day representing the current day.
For every PAY transaction, a 2% cashback of the paid amount should be credited back to the same account one day after the PAY transaction was made. This means if a payment occurs on day N, the cashback is due on day N + 1.


```java
import java.util.HashMap;
import java.util.Map;
import java.util.List;
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;

// Account class (Same as Level 2, no changes needed for Level 3 logic)
class Account {
    String accountId;
    double balance;
    int transactionOutCount;

    public Account(String accountId) {
        this.accountId = accountId;
        this.balance = 0.0;
        this.transactionOutCount = 0;
    }

    public String getAccountId() {
        return accountId;
    }

    public double getBalance() {
        return balance;
    }

    public int getTransactionOutCount() {
        return transactionOutCount;
    }

    public void deposit(double amount) {
        if (amount > 0) {
            this.balance += amount;
        }
    }

    public boolean withdraw(double amount) {
        if (amount > 0 && this.balance >= amount) {
            this.balance -= amount;
            return true;
        }
        return false;
    }

    public void incrementTransactionOutCount() {
        this.transactionOutCount++;
    }

    @Override
    public String toString() {
        return "Account{" +
               "accountId='" + accountId + '\'' +
               ", balance=" + balance +
               ", transactionOutCount=" + transactionOutCount +
               '}';
    }
}

// CashbackTask class (Same as Level 3 previously)
class CashbackTask {
    String accountId;
    double amount;

    public CashbackTask(String accountId, double amount) {
        this.accountId = accountId;
        this.amount = amount;
    }

    public String getAccountId() {
        return accountId;
    }

    public void setAccountId(String accountId) { // Important for Level 4 merge
        this.accountId = accountId;
    }

    public double getAmount() {
        return amount;
    }

    @Override
    public String toString() {
        return "CashbackTask{" +
               "accountId='" + accountId + '\'' +
               ", amount=" + amount +
               '}';
    }
}

// BankSystem class (MODIFIED for Level 3: int day input for pay and processCashbacks)
public class BankSystem {
    private Map<String, Account> accounts;
    // Map to store pending cashbacks: int day -> List of CashbackTask for that day
    private Map<Integer, List<CashbackTask>> pendingCashbacks; // Key is now int day
    private static final double CASHBACK_RATE = 0.02; // 2% cashback

    public BankSystem() {
        this.accounts = new HashMap<>();
        this.pendingCashbacks = new HashMap<>();
    }

    public boolean createAccount(String accountId) {
        if (accounts.containsKey(accountId)) {
            System.out.println("Error: Account '" + accountId + "' already exists.");
            return false;
        }
        accounts.put(accountId, new Account(accountId));
        System.out.println("Account '" + accountId + "' created successfully.");
        return true;
    }

    public boolean deposit(String accountId, double amount) {
        if (amount <= 0) {
            System.out.println("Error: Deposit amount must be positive.");
            return false;
        }
        Account account = accounts.get(accountId);
        if (account == null) {
            System.out.println("Error: Account '" + accountId + "' not found for deposit.");
            return false;
        }
        account.deposit(amount);
        System.out.println("Deposited " + amount + " into account '" + accountId + "'. New balance: " + account.getBalance());
        return true;
    }

    public boolean transfer(String fromAccountId, String toAccountId, double amount) {
        if (amount <= 0) {
            System.out.println("Error: Transfer amount must be positive.");
            return false;
        }
        if (fromAccountId.equals(toAccountId)) {
            System.out.println("Error: Cannot transfer to the same account.");
            return false;
        }

        Account fromAccount = accounts.get(fromAccountId);
        Account toAccount = accounts.get(toAccountId);

        if (fromAccount == null) {
            System.out.println("Error: 'From' account '" + fromAccountId + "' not found.");
            return false;
        }
        if (toAccount == null) {
            System.out.println("Error: 'To' account '" + toAccountId + "' not found.");
            return false;
        }

        if (!fromAccount.withdraw(amount)) {
            System.out.println("Error: Insufficient funds in account '" + fromAccountId + "' for transfer.");
            return false;
        }

        toAccount.deposit(amount);
        fromAccount.incrementTransactionOutCount();
        System.out.println("Transferred " + amount + " from '" + fromAccountId + "' to '" + toAccountId + "'.");
        System.out.println("New balance for '" + fromAccountId + "': " + fromAccount.getBalance());
        System.out.println("New balance for '" + toAccountId + "': " + toAccount.getBalance());
        return true;
    }

    /**
     * Handles a PAY action, deducting amount and scheduling cashback.
     * @param accountId The account ID for the payment.
     * @param amount The amount to pay.
     * @param currentDay The current day when the payment is made.
     * @return true if payment was successful, false otherwise.
     */
    public boolean pay(String accountId, double amount, int currentDay) {
        if (amount <= 0) {
            System.out.println("Error: Pay amount must be positive.");
            return false;
        }
        Account account = accounts.get(accountId);
        if (account == null) {
            System.out.println("Error: Account '" + accountId + "' not found for payment.");
            return false;
        }

        if (!account.withdraw(amount)) {
            System.out.println("Error: Insufficient funds in account '" + accountId + "' for payment.");
            return false;
        }

        System.out.println("Paid " + amount + " from account '" + accountId + "'. New balance: " + account.getBalance());

        // Schedule cashback for one day later
        double cashbackAmount = amount * CASHBACK_RATE;
        int cashbackDay = currentDay + 1; // Cashback due on the next day

        pendingCashbacks.computeIfAbsent(cashbackDay, k -> new ArrayList<>())
                        .add(new CashbackTask(accountId, cashbackAmount));
        System.out.println("Scheduled " + String.format("%.2f", cashbackAmount) + " cashback for '" + accountId + "' on Day " + cashbackDay);
        return true;
    }

    /**
     * Processes all pending cashbacks for a given day.
     * @param currentDay The day for which to process cashbacks.
     */
    public void processCashbacks(int currentDay) {
        System.out.println("\n--- Processing cashbacks for Day " + currentDay + " ---");
        List<CashbackTask> tasksToProcess = pendingCashbacks.remove(currentDay); // Remove after getting
        if (tasksToProcess == null || tasksToProcess.isEmpty()) {
            System.out.println("No cashbacks due on Day " + currentDay);
            return;
        }

        for (CashbackTask task : tasksToProcess) {
            Account account = accounts.get(task.getAccountId());
            if (account != null) {
                account.deposit(task.getAmount());
                System.out.println("Cashback processed: " + String.format("%.2f", task.getAmount()) + " credited to account '" + task.getAccountId() + "'. New balance: " + account.getBalance());
            } else {
                System.out.println("Warning: Account '" + task.getAccountId() + "' for cashback not found (might have been merged). Cashback amount: " + String.format("%.2f", task.getAmount()));
            }
        }
    }

    public List<Account> getTopNActiveUsers(int n) {
        if (n <= 0) {
            System.out.println("Error: N must be a positive integer.");
            return Collections.emptyList();
        }

        List<Account> allAccounts = new ArrayList<>(accounts.values());
        Collections.sort(allAccounts, new Comparator<Account>() {
            @Override
            public int compare(Account a1, Account a2) {
                return Integer.compare(a2.getTransactionOutCount(), a1.getTransactionOutCount());
            }
        });
        return allAccounts.subList(0, Math.min(n, allAccounts.size()));
    }

    // This method will be from Level 4, copied here for compilation context but not tested in Level 3 main
    public boolean mergeAccounts(String sourceAccountId, String targetAccountId) {
        if (sourceAccountId.equals(targetAccountId)) {
            System.out.println("Error: Cannot merge an account into itself.");
            return false;
        }

        Account sourceAccount = accounts.get(sourceAccountId);
        Account targetAccount = accounts.get(targetAccountId);

        if (sourceAccount == null) {
            System.out.println("Error: Source account '" + sourceAccountId + "' not found for merging.");
            return false;
        }
        if (targetAccount == null) {
            System.out.println("Error: Target account '" + targetAccountId + "' not found for merging.");
            return false;
        }

        System.out.println("\n--- Merging Account '" + sourceAccountId + "' into '" + targetAccountId + "' ---");

        targetAccount.deposit(sourceAccount.getBalance());
        System.out.println("Transferred balance of " + sourceAccount.getBalance() + " from '" + sourceAccountId + "' to '" + targetAccountId + "'.");

        targetAccount.addTransactionOutCount(sourceAccount.getTransactionOutCount());
        System.out.println("Transferred " + sourceAccount.getTransactionOutCount() + " outbound transactions from '" + sourceAccountId + "' to '" + targetAccountId + "'.");

        // Re-point pending cashbacks
        for (List<CashbackTask> tasks : pendingCashbacks.values()) {
            for (CashbackTask task : tasks) {
                if (task.getAccountId().equals(sourceAccountId)) {
                    task.setAccountId(targetAccountId);
                    System.out.println("Re-pointed pending cashback of " + String.format("%.2f", task.getAmount()) + " from '" + sourceAccountId + "' to '" + targetAccountId + "'.");
                }
            }
        }

        accounts.remove(sourceAccountId);
        System.out.println("Source account '" + sourceAccountId + "' removed successfully.");
        System.out.println("Merge successful. New balance for '" + targetAccountId + "': " + targetAccount.getBalance());
        System.out.println("New transaction out count for '" + targetAccountId + "': " + targetAccount.getTransactionOutCount());
        return true;
    }


    public double getAccountBalance(String accountId) {
        Account account = accounts.get(accountId);
        return account != null ? account.getBalance() : -1.0;
    }

    public static void main(String[] args) {
        BankSystem bank = new BankSystem();

        bank.createAccount("ACC001");
        bank.createAccount("ACC002");
        bank.createAccount("ACC003");

        bank.deposit("ACC001", 1000.0);
        bank.deposit("ACC002", 500.0);

        System.out.println("\n--- Level 3: Testing PAY and CASHBACK with 'day' input ---");

        int currentDay = 1; // Simulate Day 1
        System.out.println("Current Day (simulated): " + currentDay);

        // Pay on Day 1
        bank.pay("ACC001", 100.0, currentDay); // Cashback of 2.0 scheduled for Day 2
        bank.pay("ACC002", 50.0, currentDay);  // Cashback of 1.0 scheduled for Day 2

        currentDay++; // Simulate moving to Day 2
        System.out.println("\nSimulating next day: Day " + currentDay);

        // Process cashbacks for Day 2
        bank.processCashbacks(currentDay);

        currentDay++; // Simulate moving to Day 3
        System.out.println("\nSimulating next day: Day " + currentDay);

        // Process cashbacks for Day 3 (should be none from previous payments)
        bank.processCashbacks(currentDay);

        // Make another payment on Day 3
        bank.pay("ACC001", 20.0, currentDay); // Cashback of 0.4 scheduled for Day 4

        currentDay++; // Simulate moving to Day 4
        System.out.println("\nSimulating next day: Day " + currentDay);
        bank.processCashbacks(currentDay); // Process cashback for Day 4

        System.out.println("\n--- Level 2: Testing GET_TOP_N_ACTIVE_USERS (with previous transfers) ---");
        bank.transfer("ACC001", "ACC002", 10.0); // Adds to ACC001's activity count
        bank.transfer("ACC001", "ACC003", 5.0);  // Adds to ACC001's activity count

        List<Account> topUsers = bank.getTopNActiveUsers(2);
        System.out.println("Top 2 Most Active Users (by outbound transfers):");
        for (Account account : topUsers) {
            System.out.println("  - " + account.getAccountId() + " (Transfers: " + account.getTransactionOutCount() + ")");
        }

        System.out.println("\n--- Final Balances ---");
        System.out.println("Balance for ACC001: " + bank.getAccountBalance("ACC001"));
        System.out.println("Balance for ACC002: " + bank.getAccountBalance("ACC002"));
        System.out.println("Balance for ACC003: " + bank.getAccountBalance("ACC003"));
    }
}
```
