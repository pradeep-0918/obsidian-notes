# 10 — Java Code Templates

> **MOC:** [[Linked List MOC]] | **Prev:** [[09 - Recursion on Linked Lists]] | **Next:** [[11 - Top 30 Problems Classified]]

---

## 🔧 Complete Singly Linked List Implementation

```java
class SinglyLinkedList {
    
    // ── Node ───────────────────────────────────────────
    static class ListNode {
        int val;
        ListNode next;
        ListNode(int val) { this.val = val; }
        ListNode(int val, ListNode next) { this.val = val; this.next = next; }
    }

    ListNode head;

    // ── Build ───────────────────────────────────────────
    public ListNode fromArray(int[] arr) {
        ListNode dummy = new ListNode(0);
        ListNode curr = dummy;
        for (int v : arr) { curr.next = new ListNode(v); curr = curr.next; }
        return head = dummy.next;
    }

    // ── Insert ──────────────────────────────────────────
    public void insertHead(int val) {
        ListNode node = new ListNode(val);
        node.next = head;
        head = node;
    }

    public void insertTail(int val) {
        if (head == null) { head = new ListNode(val); return; }
        ListNode curr = head;
        while (curr.next != null) curr = curr.next;
        curr.next = new ListNode(val);
    }

    public void insertAt(int pos, int val) {
        ListNode dummy = new ListNode(0, head);
        ListNode curr = dummy;
        for (int i = 0; i < pos && curr.next != null; i++) curr = curr.next;
        ListNode node = new ListNode(val);
        node.next = curr.next;
        curr.next = node;
        head = dummy.next;
    }

    // ── Delete ──────────────────────────────────────────
    public void deleteHead() { if (head != null) head = head.next; }

    public void deleteValue(int val) {
        ListNode dummy = new ListNode(0, head);
        ListNode curr = dummy;
        while (curr.next != null) {
            if (curr.next.val == val) curr.next = curr.next.next;
            else curr = curr.next;
        }
        head = dummy.next;
    }

    // ── Search ──────────────────────────────────────────
    public ListNode find(int val) {
        ListNode curr = head;
        while (curr != null) {
            if (curr.val == val) return curr;
            curr = curr.next;
        }
        return null;
    }

    // ── Reverse ─────────────────────────────────────────
    public void reverse() {
        ListNode prev = null, curr = head;
        while (curr != null) {
            ListNode next = curr.next;
            curr.next = prev;
            prev = curr;
            curr = next;
        }
        head = prev;
    }

    // ── Utilities ───────────────────────────────────────
    public int size() {
        int count = 0;
        ListNode curr = head;
        while (curr != null) { count++; curr = curr.next; }
        return count;
    }

    public ListNode middle() {
        ListNode slow = head, fast = head;
        while (fast != null && fast.next != null) {
            slow = slow.next;
            fast = fast.next.next;
        }
        return slow;
    }

    @Override
    public String toString() {
        StringBuilder sb = new StringBuilder();
        ListNode curr = head;
        while (curr != null) {
            sb.append(curr.val);
            if (curr.next != null) sb.append(" → ");
            curr = curr.next;
        }
        sb.append(" → null");
        return sb.toString();
    }
}
```

---

## 🔧 Complete Doubly Linked List Implementation

```java
class DoublyLinkedList {

    static class Node {
        int val;
        Node prev, next;
        Node(int val) { this.val = val; }
    }

    Node head, tail;

    public void insertHead(int val) {
        Node node = new Node(val);
        if (head == null) { head = tail = node; return; }
        node.next = head;
        head.prev = node;
        head = node;
    }

    public void insertTail(int val) {
        Node node = new Node(val);
        if (tail == null) { head = tail = node; return; }
        tail.next = node;
        node.prev = tail;
        tail = node;
    }

    public void delete(Node node) {
        if (node.prev != null) node.prev.next = node.next;
        else head = node.next;
        if (node.next != null) node.next.prev = node.prev;
        else tail = node.prev;
    }
}
```

---

## 🔧 LRU Cache Implementation (DLL + HashMap — LC 146)

```java
class LRUCache {
    class Node {
        int key, val;
        Node prev, next;
        Node(int k, int v) { key = k; val = v; }
    }

    private final int capacity;
    private final Map<Integer, Node> map;
    private final Node head, tail;  // dummy sentinels

    public LRUCache(int capacity) {
        this.capacity = capacity;
        this.map = new HashMap<>();
        head = new Node(0, 0);
        tail = new Node(0, 0);
        head.next = tail;
        tail.prev = head;
    }

    public int get(int key) {
        if (!map.containsKey(key)) return -1;
        Node node = map.get(key);
        moveToFront(node);
        return node.val;
    }

    public void put(int key, int value) {
        if (map.containsKey(key)) {
            Node node = map.get(key);
            node.val = value;
            moveToFront(node);
        } else {
            if (map.size() == capacity) {
                Node lru = tail.prev;
                remove(lru);
                map.remove(lru.key);
            }
            Node node = new Node(key, value);
            insertFront(node);
            map.put(key, node);
        }
    }

    private void moveToFront(Node node) { remove(node); insertFront(node); }

    private void insertFront(Node node) {
        node.next = head.next;
        node.prev = head;
        head.next.prev = node;
        head.next = node;
    }

    private void remove(Node node) {
        node.prev.next = node.next;
        node.next.prev = node.prev;
    }
}
```

---

## 🔧 Quick Utility Functions (Paste into any solution)

```java
// Convert array to linked list
static ListNode toList(int[] arr) {
    ListNode dummy = new ListNode(0);
    ListNode curr = dummy;
    for (int v : arr) { curr.next = new ListNode(v); curr = curr.next; }
    return dummy.next;
}

// Convert linked list to array (for testing)
static int[] toArray(ListNode head) {
    List<Integer> list = new ArrayList<>();
    while (head != null) { list.add(head.val); head = head.next; }
    return list.stream().mapToInt(i -> i).toArray();
}

// Print linked list
static void print(ListNode head) {
    StringBuilder sb = new StringBuilder();
    while (head != null) {
        sb.append(head.val).append(head.next != null ? " → " : "");
        head = head.next;
    }
    System.out.println(sb + " → null");
}

// Reverse (inline, for use within solutions)
static ListNode reverse(ListNode head) {
    ListNode prev = null, curr = head;
    while (curr != null) {
        ListNode next = curr.next;
        curr.next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
}

// Find middle
static ListNode middle(ListNode head) {
    ListNode slow = head, fast = head;
    while (fast != null && fast.next != null) {
        slow = slow.next; fast = fast.next.next;
    }
    return slow;
}
```

---

## 🔗 Connected Notes
- [[04 - Core Operations]] — concepts behind each template
- [[08 - Merge and Sort]] — LRU Cache uses DLL
- [[11 - Top 30 Problems Classified]] — which template goes with which problem

---
`#DSA` `#LinkedList` `#Java` `#Templates` `#Reference`
