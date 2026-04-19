<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/bff545b7-661d-4647-b9b0-acd1719f9b84" /><img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3bbafce0-c9c5-4944-95c9-47233583a270" />

<img width="1670" height="846" alt="image" src="https://github.com/user-attachments/assets/cd9bfa48-b5d5-45ec-849f-22977e8b5218" />

<img width="1614" height="833" alt="image" src="https://github.com/user-attachments/assets/1da4c455-868c-4027-a013-2b63a5f6bfc5" />


```
const firstArray = [];
const secondArray = [];

for (let i = 0; i < 600000; i++) {
  if (i < 300000) {
    firstArray.push(i);
  }
  secondArray.push(i);
}

console.log("first array", firstArray.length);
console.log("second array", secondArray.length);

const firstUserList = firstArray.map((number) => ({ userId: number }));

const secondUserList = secondArray.map((number) => ({ userId: number }));

console.time("find");

// const user = secondUserList.find((user) => user.userId === 500000);

const user = secondUserList[400000];

console.timeEnd("find");
```
