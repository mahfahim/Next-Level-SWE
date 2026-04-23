class Node {
  constructor(value) {
    this.value = value;
    this.next = null;
  }
}

const head = new Node(10);

//* { value: 10, next: null }

head.next = new Node(20);

//* { value: 10, next: { value: 20, next: null } }

head.next.next = new Node(30);

console.log(head);

let temp = head;

while (temp !== null) {
  console.log(temp.value, " ");
  temp = temp.next;
}
