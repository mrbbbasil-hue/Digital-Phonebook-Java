
package phonebook.java;

import java.util.Scanner;





 

 
// ==================== Node for Linked List ====================
class Node {
    String personName;
    String phoneNumber;
    Node next;
 
    Node(String name, String phone) {
        personName = name;
        phoneNumber = phone;
        next = null;
    }
}
 
// ==================== Sorted Linked List ====================
class LinkedListPhonebook {
 
    Node head;
    int size;
 
    LinkedListPhonebook() {
        head = null;
        size = 0;
    }
 
    // insert sorted by name
    void insert(String name, String phone) {
        if (name.isEmpty() || phone.isEmpty()) {
            System.out.println("Error: name or phone cannot be empty.");
            return;
        }
 
        Node newNode = new Node(name, phone);
 
        // check duplicate
        if (search(name) != null) {
            System.out.println("Contact already exists: " + name);
            return;
        }
 
        // insert at head if list is empty or name comes before head
        if (head == null || name.compareToIgnoreCase(head.personName) < 0) {
            newNode.next = head;
            head = newNode;
        } else {
            Node curr = head;
            while (curr.next != null && curr.next.personName.compareToIgnoreCase(name) < 0) {
                curr = curr.next;
            }
            newNode.next = curr.next;
            curr.next = newNode;
        }
        size++;
        System.out.println("Inserted: " + name + " | " + phone);
    }
 
    // search by name, returns node or null
    Node search(String name) {
        Node curr = head;
        while (curr != null) {
            if (curr.personName.equalsIgnoreCase(name))
                return curr;
            curr = curr.next;
        }
        return null;
    }
 
    // remove by name
    void remove(String name) {
        if (head == null) {
            System.out.println("Phonebook is empty.");
            return;
        }
        if (head.personName.equalsIgnoreCase(name)) {
            head = head.next;
            size--;
            System.out.println("Removed: " + name);
            return;
        }
        Node curr = head;
        while (curr.next != null) {
            if (curr.next.personName.equalsIgnoreCase(name)) {
                curr.next = curr.next.next;
                size--;
                System.out.println("Removed: " + name);
                return;
            }
            curr = curr.next;
        }
        System.out.println("Not found: " + name);
    }
 
    // update phone number if contact exists
    void updateExistingOne(String name, String newPhone) {
        Node found = search(name);
        if (found != null) {
            found.phoneNumber = newPhone;
            System.out.println("Updated: " + name + " -> " + newPhone);
        } else {
            System.out.println("Contact not found: " + name);
        }
    }
 
    // display all contacts sorted by name (already sorted in list)
    void display() {
        if (head == null) {
            System.out.println("Phonebook is empty.");
            return;
        }
        System.out.println("\n--- All Contacts (A to Z) ---");
        Node curr = head;
        int i = 1;
        while (curr != null) {
            System.out.println(i + ". " + curr.personName + " | " + curr.phoneNumber);
            curr = curr.next;
            i++;
        }
        System.out.println("-----------------------------");
    }
 
    // isolate all contacts whose phone starts with 0100
    LinkedListPhonebook isolate0100() {
        LinkedListPhonebook result = new LinkedListPhonebook();
        Node curr = head;
        while (curr != null) {
            if (curr.phoneNumber.startsWith("0100")) {
                result.insert(curr.personName, curr.phoneNumber);
            }
            curr = curr.next;
        }
        return result;
    }
 
    int size() {
        return size;
    }
}
 
// ==================== BST Node ====================
class BSTNode {
    String personName;
    String phoneNumber;
    BSTNode left, right;
 
    BSTNode(String name, String phone) {
        personName = name;
        phoneNumber = phone;
        left = null;
        right = null;
    }
}
 
// ==================== BST Phonebook ====================
class BSTPhonebook {
 
    BSTNode root;
    int size;
 
    BSTPhonebook() {
        root = null;
        size = 0;
    }
 
    // insert into BST sorted by name
    void insert(String name, String phone) {
        if (name.isEmpty() || phone.isEmpty()) {
            System.out.println("Error: name or phone cannot be empty.");
            return;
        }
        if (search(name) != null) {
            System.out.println("Contact already exists: " + name);
            return;
        }
        root = insertHelper(root, name, phone);
        size++;
        System.out.println("Inserted: " + name + " | " + phone);
    }
 
    BSTNode insertHelper(BSTNode node, String name, String phone) {
        if (node == null)
            return new BSTNode(name, phone);
 
        int cmp = name.compareToIgnoreCase(node.personName);
        if (cmp < 0)
            node.left = insertHelper(node.left, name, phone);
        else if (cmp > 0)
            node.right = insertHelper(node.right, name, phone);
 
        return node;
    }
 
    // search returns node or null
    BSTNode search(String name) {
        return searchHelper(root, name);
    }
 
    BSTNode searchHelper(BSTNode node, String name) {
        if (node == null)
            return null;
 
        int cmp = name.compareToIgnoreCase(node.personName);
        if (cmp == 0)
            return node;
        else if (cmp < 0)
            return searchHelper(node.left, name);
        else
            return searchHelper(node.right, name);
    }
 
    // remove by name
    void remove(String name) {
        if (search(name) == null) {
            System.out.println("Not found: " + name);
            return;
        }
        root = removeHelper(root, name);
        size--;
        System.out.println("Removed: " + name);
    }
 
    BSTNode removeHelper(BSTNode node, String name) {
        if (node == null)
            return null;
 
        int cmp = name.compareToIgnoreCase(node.personName);
        if (cmp < 0) {
            node.left = removeHelper(node.left, name);
        } else if (cmp > 0) {
            node.right = removeHelper(node.right, name);
        } else {
            // node found
            if (node.left == null) return node.right;
            if (node.right == null) return node.left;
 
            // two children: replace with in-order successor
            BSTNode successor = findMin(node.right);
            node.personName  = successor.personName;
            node.phoneNumber = successor.phoneNumber;
            node.right = removeHelper(node.right, successor.personName);
        }
        return node;
    }
 
    BSTNode findMin(BSTNode node) {
        while (node.left != null)
            node = node.left;
        return node;
    }
 
    // update phone if contact exists
    void updateExistingOne(String name, String newPhone) {
        BSTNode found = search(name);
        if (found != null) {
            found.phoneNumber = newPhone;
            System.out.println("Updated: " + name + " -> " + newPhone);
        } else {
            System.out.println("Contact not found: " + name);
        }
    }
 
    // display all contacts in order (A to Z) using in-order traversal
    void display() {
        if (root == null) {
            System.out.println("Phonebook is empty.");
            return;
        }
        System.out.println("\n--- All Contacts (A to Z) ---");
        inorder(root, new int[]{1});
        System.out.println("-----------------------------");
    }
 
    void inorder(BSTNode node, int[] counter) {
        if (node == null) return;
        inorder(node.left, counter);
        System.out.println(counter[0] + ". " + node.personName + " | " + node.phoneNumber);
        counter[0]++;
        inorder(node.right, counter);
    }
 
    // isolate all contacts whose phone starts with 0100
    BSTPhonebook isolate0100() {
        BSTPhonebook result = new BSTPhonebook();
        collectNodes(root, result);
        return result;
    }
 
    void collectNodes(BSTNode node, BSTPhonebook result) {
        if (node == null) return;
        if (node.phoneNumber.startsWith("0100"))
            result.insert(node.personName, node.phoneNumber);
        collectNodes(node.left, result);
        collectNodes(node.right, result);
    }
 
    int size() {
        return size;
    }
}
 
// ==================== Main Menu ====================
public class Phonebook {
 
    static Scanner sc = new Scanner(System.in);
    static LinkedListPhonebook ll  = new LinkedListPhonebook();
    static BSTPhonebook        bst = new BSTPhonebook();
 
    public static void main(String[] args) {
        loadSampleData();
 
        int choice = -1;
        while (choice != 0) {
            printMenu();
            try {
                choice = Integer.parseInt(sc.nextLine().trim());
            } catch (NumberFormatException e) {
                System.out.println("Please enter a valid number.");
                continue;
            }
 
            switch (choice) {
                case 1: doInsert();      break;
                case 2: doSearch();      break;
                case 3: doRemove();      break;
                case 4: doUpdate();      break;
                case 5: doDisplay();     break;
                case 6: doIsolate0100(); break;
                case 7: doSize();        break;
                case 0: System.out.println("Bye!"); break;
                default: System.out.println("Invalid choice, try again.");
            }
        }
    }
 
    static void printMenu() {
        System.out.println("\n==============================");
        System.out.println("   Digital Phonebook System");
        System.out.println("==============================");
        System.out.println("1. Insert Contact");
        System.out.println("2. Search Contact");
        System.out.println("3. Remove Contact");
        System.out.println("4. Update Phone Number");
        System.out.println("5. Display All Contacts");
        System.out.println("6. Isolate 0100 Contacts");
        System.out.println("7. Show Size");
        System.out.println("0. Exit");
        System.out.print("Enter choice: ");
    }
 
    static void doInsert() {
        System.out.print("Name  : ");
        String name = sc.nextLine().trim();
        System.out.print("Phone : ");
        String phone = sc.nextLine().trim();
 
        System.out.println("\n[Linked List]");
        ll.insert(name, phone);
        System.out.println("[BST]");
        bst.insert(name, phone);
    }
 
    static void doSearch() {
        System.out.print("Enter name to search: ");
        String name = sc.nextLine().trim();
 
        System.out.println("\n[Linked List]");
        Node r1 = ll.search(name);
        if (r1 != null)
            System.out.println("Found: " + r1.personName + " | " + r1.phoneNumber);
        else
            System.out.println("Not found.");
 
        System.out.println("[BST]");
        BSTNode r2 = bst.search(name);
        if (r2 != null)
            System.out.println("Found: " + r2.personName + " | " + r2.phoneNumber);
        else
            System.out.println("Not found.");
    }
 
    static void doRemove() {
        System.out.print("Enter name to remove: ");
        String name = sc.nextLine().trim();
        System.out.println("\n[Linked List]");
        ll.remove(name);
        System.out.println("[BST]");
        bst.remove(name);
    }
 
    static void doUpdate() {
        System.out.print("Enter name to update: ");
        String name = sc.nextLine().trim();
        System.out.print("Enter new phone: ");
        String phone = sc.nextLine().trim();
        System.out.println("\n[Linked List]");
        ll.updateExistingOne(name, phone);
        System.out.println("[BST]");
        bst.updateExistingOne(name, phone);
    }
 
    static void doDisplay() {
        System.out.println("\n[Linked List]");
        ll.display();
        System.out.println("[BST]");
        bst.display();
    }
 
    static void doIsolate0100() {
        System.out.println("\n[Linked List - contacts starting with 0100]");
        LinkedListPhonebook sub1 = ll.isolate0100();
        if (sub1.size() == 0)
            System.out.println("No contacts found.");
        else
            sub1.display();
 
        System.out.println("[BST - contacts starting with 0100]");
        BSTPhonebook sub2 = bst.isolate0100();
        if (sub2.size() == 0)
            System.out.println("No contacts found.");
        else
            sub2.display();
    }
 
    static void doSize() {
        System.out.println("Linked List size : " + ll.size());
        System.out.println("BST size         : " + bst.size());
    }
 
    static void loadSampleData() {
        System.out.println("Loading sample data...");
        String[][] data = {
            {"Ahmed Hassan",  "01001234567"},
            {"Fatima Nour",   "01112345678"},
            {"Mohamed Ali",   "01001987654"},
            {"Sara Khaled",   "01234567890"},
            {"Omar Ibrahim",  "01002345678"},
            {"Layla Mostafa", "01501234567"},
            {"Youssef Tarek", "01100001111"},
            {"Nada Samir",    "01000099999"},
            {"Khaled Wael",   "01200012345"},
            {"Dina Ramadan",  "01001111222"}
        };
        for (String[] d : data) {
            ll.insert(d[0], d[1]);
            bst.insert(d[0], d[1]);
        }
        System.out.println("Done.\n");
    }
}
 
