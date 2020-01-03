```
public class LinkedListLoop {

	class Node {
		int data;
		Node next;

		Node(int value) {
			this.data = value;
			this.next = null;
		}
	}

	Node head;
```
```
	public void pushAtStart(int value) {
		Node newNode = new Node(value);
		newNode.next = head;
		head = newNode;

	}
```
```
	public void pushAtEnd(int value) {
		Node newNode = new Node(value);
		Node temp = head;
		if (temp == null) {
			head = newNode;
		} else {
			while (temp.next != null) {
				temp = temp.next;
			}
			temp.next = newNode;
		}
	}
```
```
	public void delete(int value) {
		Node temp = head;
		Node prev = null;
		while (temp != null && temp.data == value) {// if value at the head
			head = temp.next;
			return;
		}
		while (temp != null && temp.data != value) {
			prev = temp;
			temp = temp.next;
		}
		if (temp == null)// If value not found
			return;
		prev.next = temp.next;
	}
```
```
	public void print() {
		Node temp = head;
		while (temp != null) {
			System.out.println(temp.data);
			temp = temp.next;
		}
	}
```
```
	public int length() {
		int size = 0;
		Node temp = head;
		while (temp != null) {
			size++;
			temp = temp.next;

		}
		System.out.println("Size " + size);
		return size;
	}
```
```
	public void getNthNode(int index) {
		Node temp = head;
		int count = 1;
		while (temp != null) {
			if (count == index) {
				System.out.println("Data at the " + index + " is " + temp.data);
				return;
			}
			count++;
			temp = temp.next;
		}
		System.out.println("Not found");
	}
```
```
	public void getNthNodeFromEnd(int index) {
		Node temp = head;
		int length = length();
		getNthNode(length - index + 1);

	}
```
```
	public void printMiddle() {
		Node slow = head, fast = head;
		if (head != null) {
			while (fast != null && fast.next != null) {
				slow = slow.next;
				fast = fast.next.next;
			}
			System.out.println("Middle element " + slow.data);
		}
	}
```
```
	public void detectLoop() {
		// Time Complexity: O(n)
		// Auxiliary Space: O(1)
		Node slow = head, fast = head;
		while (slow != null && fast != null && fast.next != null) {
			slow = slow.next;
			fast = fast.next.next;
			if (slow == fast) {
				System.out.println("Loop Found");
				return;
			}
		}
	}
```
```
	public void reverseLinkedList() {
    // Time Complexity: O(n)
    // Space Complexity: O(1)
		Node current = head;
		Node prev = null, next = null;
		while (current != null) {
			next = current.next;
			current.next = prev;
			prev = current;
			current = next;
		}
		head = prev;
	}
```
```
	public static void main(String[] args) {
		LinkedListLoop linkedList = new LinkedListLoop();

		linkedList.pushAtStart(20);
		linkedList.pushAtStart(4);
		linkedList.pushAtStart(15);
		linkedList.pushAtEnd(10);
		linkedList.length();
		linkedList.delete(4);
		linkedList.length();
		System.out.println("Print LinkedList");
		linkedList.print();
		linkedList.getNthNode(3);
	}

}
```
