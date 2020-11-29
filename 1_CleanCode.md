# CleanCode-JavaScript

## สารบัญ

1. [Introduction](#introduction)
2. [Variables](#variables)
3. [Functions](#functions)
4. [Objects and Data Structures](#objects-and-data-structures)
5. [Classes](#classes)
6. [SOLID](#solid)
7. [Testing](#testing)
8. [Concurrency](#concurrency)
9. [Error Handling](#error-handling)
10. [Formatting](#formatting)
11. [Comments](#comments)
12. [Translation](#translation)

## Introduction

![Humorous image of software quality estimation as a count of how many expletives
you shout when reading code](https://www.osnews.com/images/comics/wtfm.jpg)

หลักการ Clean Code สร้างขึ้นจากพื้นฐาน 3R  
1.Readable : อ่านทำความเข้าใจง่าย เช่น ชื่อตัวแปรสื่อความหมาย ,โค้ดมีรูปแบบเดียวกันสม่ำเสมอ  
2.Resuable : สามารถใช้โค้ดซ้ำได้ เช่น ไม่ต้องเขียนโมดูลบางอย่าใหม่หมด , ฟังก์ชันมีขนาดเล็ก , มีการหลีกเลี่ยง Side effect  
3.Refactorable : สามารถแก้ไขแแยกส่วนโค้ดได้ โดยไม่ส่งผลกับโปรแกรมใหญ่ทั้งหมด


อีกสิ่งหนึ่งคืออย่าพยายามทำให้สมบูรณ์แบบตั้งแต่ในทีแรก  
อย่าเร่งรีบเอาชนะตัวเองด้วยโค้ดที่สมบูรณ์แบบ เปลี่ยนมาเอาชนะโค้ดทีละเล็กทีละน้อยแทน  

## **Variables**

### ใช้ที่ตัวแปรที่สือความหมายชัดเจน

**Bad:**

```javascript
const yyyymmdstr = moment().format("YYYY/MM/DD");
```

**Good:**

```javascript
const currentDate = moment().format("YYYY/MM/DD");
```

**[⬆ back to top](#table-of-contents)**  

### ใช้คำศัพท์เดียวกันสำหรับตัวแปรประเภทเดียวกัน

**Bad:**

```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**Good:**

```javascript
getUser();
```

**[⬆ back to top](#table-of-contents)**

### ใช้ชื่อตัวแปรที่สามารถค้นหาได้

หากเราไม่ตั้งชื่อตัวแปรใสามารถห้ค้นหาได้จะทำให้เพื่อนร่วมงานหรือคนรีวิวโค้ดนั้นทำงานต่อจากเราปวดเศียรเวียนเกล้า  มีเครื่องมือจำพวก 
[buddy.js](https://github.com/danielstjules/buddy.js) และ
[ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md)
ที่สามารถช่วยให้เราระบุตัวแปรค่าคงที่ที่ยังไม่ถูกตั้งชื่อได้

**Bad:**

```javascript
// What the heck is 86400000 for?
setTimeout(blastOff, 86400000);
```

**Good:**

```javascript
// Declare them as capitalized named constants.
const MILLISECONDS_IN_A_DAY = 86_400_000;

setTimeout(blastOff, MILLISECONDS_IN_A_DAY);
```

**[⬆ back to top](#table-of-contents)**  

### ใช้ชื่อตัวแปรที่อธิบายตัวเองได้

**Bad:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(
  address.match(cityZipCodeRegex)[1],
  address.match(cityZipCodeRegex)[2]
);
```

**Good:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [_, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```

**[⬆ back to top](#table-of-contents)**

### หลีกเลี่ยงการ Map โดยใช้ชื่อตัวแปรที่คนอ่านต้องไปนึกต่อเอาเอง

ตั้งชื่อให้ชัดเจนดีกว่าย่อแบบกำกวม

**Bad:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(l => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  // เดี๋ยวนะ l คิออะไรนะ
  dispatch(l);
});
```

**Good:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(location => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  dispatch(location);
});
```

**[⬆ back to top](#table-of-contents)**    

### อย่าใช้คำฟุ่มเฟือยโดยไม่จำเป็น

ถ้าชื่อคลาสหรือออปเจ็คบอกอะไรเราบางอย่างแล้ว ไม่ต้องใช้คำคำเดิมซ้ำในบริบทหรือสโคปเดียวกัน

**Bad:**

```javascript
const Car = {
  carMake: "Honda",
  carModel: "Accord",
  carColor: "Blue"
};

function paintCar(car) {
  car.carColor = "Red";
}
```

**Good:**

```javascript
const Car = {
  make: "Honda",
  model: "Accord",
  color: "Blue"
};

function paintCar(car) {
  car.color = "Red";
}
```

**[⬆ back to top](#table-of-contents)**  

### ใช้ default arguments แทนการ short circuit หรือการใช้ condition

หากใช้วิธี Defalut argument จะทำให้โค้ดดูคลีนกว่าการทำ Short Circuit  
แต่ใช้ระวังไว้อย่างนึงคือฟังก์ชันจะใช้ค่า default value เมื่อค่า argument เป็น  `undefined` เท่านั้น  
ส่วนค่า falsy อื่นๆเช่น `''`, `""`, `false`, `null`, `0`, และ
`NaN` จะไม่ถูกแทนด้วยค่า default value

**Bad:**

```javascript
function createMicrobrewery(name) {
  const breweryName = name || "Hipster Brew Co.";
  // ...
}
```

**Good:**

```javascript
function createMicrobrewery(name = "Hipster Brew Co.") {
  // ...
}
```

**[⬆ back to top](#table-of-contents)**

## **Functions**

### Function arguments (ใช้แค่ 2 ตัวหรือน้อยกว่านั้นได้จะยิ่งดี)

โดยทั่วไปจะให้ฟังก์ชันนึงมีจำนวน argument ไม่เกิน 2 ตัวหรือ 3 ก็ได้แต่ควรเลี่ยง  
เพื่อลดโอกาสการทำงานผิดพลาดของโปรแกรมเมื่อต้องมีการ Test หลายๆเคส

หากต้องการใส่ argument เป็นจำนวนมากแนะนำให้ใช้ object แทนการใช้ function

เราสามารถใช้ ES2015/ES6 ร่วมกับ Destructuring Syntax   
ในการทำให้รู้ว่าฟังก์ชันของเราต้องการ Property ใด  
โดยประโยชน์ของการใช้งานมีดังนี้

1. ถ้ามีใครมาดูตัวฟังก์ชันเรา เขาจะทราบทันทีว่าฟังก์ชันนี้ต้องการ property อะไรบ้าง  
2. สามารถใช้จำลองชื่อของพารามิเตอร์.
3. ช่วย clones ค่า primitive values ของ argument ก่อน pass เข้าฟังก์ชัน  
หลีกเลี่ยง Side Effect 
  
4. Linters ช่วยแจ้งเตือน property ที่ไม่ถูกใช้งานได้ 

**Bad:**

```javascript
function createMenu(title, body, buttonText, cancellable) {
  // ...
}

createMenu("Foo", "Bar", "Baz", true);

```

**Good:**

```javascript
function createMenu({ title, body, buttonText, cancellable }) {
  // ...
}

createMenu({
  title: "Foo",
  body: "Bar",
  buttonText: "Baz",
  cancellable: true
});
```

**[⬆ back to top](#table-of-contents)**  

### ฟังก์ชันควรมีหน้าที่ทำอย่างใดเพียงอย่างเดียวเท่านั้น

นี่น่าจะเป็นกฏข้อที่สำคัญที่สุด  
เมื่อฟังก์ชันทำงานมากกว่าหนึ่งงาน จะทำให้การ compose,การ test ทำได้ยากลำบากยิ่งขึ้น  
หากฟังก์ชันของเราทำเพียงงานใดงานหนึ่ง จะทำให้ให้โค้ดดูคลีนและง่ายต่อการแยกส่วนโค้ด

**Bad:**

```javascript
function emailClients(clients) {
  clients.forEach(client => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

**Good:**

```javascript
function emailActiveClients(clients) {
  clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

**[⬆ back to top](#table-of-contents)**


### ชื่อของฟังก์ชันควรบ่งบอกว่าตัวมันเองทำอะไรได้

**Bad:**

```javascript
function addToDate(date, month) {
  // ...
}

const date = new Date();

// It's hard to tell from the function name what is added
addToDate(date, 1);
```

**Good:**

```javascript
function addMonthToDate(month, date) {
  // ...
}

const date = new Date();
addMonthToDate(1, date);
```

**[⬆ back to top](#table-of-contents)**

### ฟังก์ชันควรมีระดับความ abstract อยู่แค่หนึ่งระดับ

ถ้าฟังก์ชันมีระดับความ abstract เกินหนึ่งระดับนั่นอาจหมายความว่า  
ฟังก์ชันของคุณอาจจะทำงานหลายอย่างมากเกินไป 
การ spilt ฟังก์ชันออกมาเป็นหลายๆฟังก์ชันจะทำให้การ reuse โค้ดและการ test ทำได้ง่ายกว่า
testing.

**Bad:**

```javascript
function parseBetterJSAlternative(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(" ");
  const tokens = [];
  REGEXES.forEach(REGEX => {
    statements.forEach(statement => {
      // ...
    });
  });

  const ast = [];
  tokens.forEach(token => {
    // lex...
  });

  ast.forEach(node => {
    // parse...
  });
}
```

**Good:**

```javascript
function parseBetterJSAlternative(code) {
  const tokens = tokenize(code);
  const syntaxTree = parse(tokens);
  syntaxTree.forEach(node => {
    // parse...
  });
}

function tokenize(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(" ");
  const tokens = [];
  REGEXES.forEach(REGEX => {
    statements.forEach(statement => {
      tokens.push(/* ... */);
    });
  });

  return tokens;
}

function parse(tokens) {
  const syntaxTree = [];
  tokens.forEach(token => {
    syntaxTree.push(/* ... */);
  });

  return syntaxTree;
}
```

**[⬆ back to top](#table-of-contents)**

### กำจัดการใช้โค้ดซ้ำกันหลายที่ (duplicate code)

นึกภาพว่าหากเรามีลิสต์จดไว้หลายที เวลาอัพเดททีก็ต้องตามไปอัพเดททุกที่ที่จดไว้  
การ duplicate โค้ดก็เช่นกัน

บ่อยครั้งการ duplicate code มาจากการที่เราต้องการใช้บางอย่างหลายๆที่ แต่มีปัจจัยที่แตกตต่างกันเล็กน้อย  
แต่กระนั้นเราควรจะเป็นสองฟังก์ชันหรือมากกว่านั้นจะดีกว่า  

การกำจัด duplicate code อาจหมายึงการสร้าง abstract อย่างหนึ่งขึ้นมา เช่น function/module/class
เพือจัดการกับงานในบริบทที่แตกต่างกัน


**Bad:**

```javascript
function showDeveloperList(developers) {
  developers.forEach(developer => {
    const expectedSalary = developer.calculateExpectedSalary();
    const experience = developer.getExperience();
    const githubLink = developer.getGithubLink();
    const data = {
      expectedSalary,
      experience,
      githubLink
    };

    render(data);
  });
}

function showManagerList(managers) {
  managers.forEach(manager => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();
    const data = {
      expectedSalary,
      experience,
      portfolio
    };

    render(data);
  });
}
```

**Good:**

```javascript
function showEmployeeList(employees) {
  employees.forEach(employee => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();

    const data = {
      expectedSalary,
      experience
    };

    switch (employee.type) {
      case "manager":
        data.portfolio = employee.getMBAProjects();
        break;
      case "developer":
        data.githubLink = employee.getGithubLink();
        break;
    }

    render(data);
  });
}
```

**[⬆ back to top](#table-of-contents)**

### ใช้ Object.assign ในการตั้งค่า defualt ของ Object  

**Bad:**

```javascript
const menuConfig = {
  title: null,
  body: "Bar",
  buttonText: null,
  cancellable: true
};

function createMenu(config) {
  config.title = config.title || "Foo";
  config.body = config.body || "Bar";
  config.buttonText = config.buttonText || "Baz";
  config.cancellable =
    config.cancellable !== undefined ? config.cancellable : true;
}

createMenu(menuConfig);
```

**Good:**

```javascript
const menuConfig = {
  title: "Order",
  // User did not include 'body' key
  buttonText: "Send",
  cancellable: true
};

function createMenu(config) {
  let finalConfig = Object.assign(
    {
      title: "Foo",
      body: "Bar",
      buttonText: "Baz",
      cancellable: true
    },
    config
  );
  return finalConfig
  // config now equals: {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
  // ...
}

createMenu(menuConfig);
```

**[⬆ back to top](#table-of-contents)**  

### อย่าใช้ตัวบอกสถานะ (flag) เป็นฟังก์ชันพารามิเตอร์

หากค่าสถานะเป็นหนึ่งในพารามิเตอร์แสดงว่าฟังก์ชันของคุณทำงานได้มากกว่าหนึ่งอย่าง  
แยกฟังก์ชันของคุณหากต้องการให้โค้ดทำงานต่างกันโดยอ้างอิงจาก Boolean


**Bad:**

```javascript
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**Good:**

```javascript
function createFile(name) {
  fs.create(name);
}

function createTempFile(name) {
  createFile(`./temp/${name}`);
}
```

**[⬆ back to top](#table-of-contents)**


### หลีกเลี่ยง Side Effects (พาร์ท 1)

การหลีกเลี่ยง Side Effect อย่างแรกเลยคือการให้ฟังก์ชันแก้ไข Global Variable โดยตรง
ซึ่งอาจทำให้ตัวแปรนั้นมีการเปลี่ยนแปลงประเภทไปและไมสามารถนำไปใช้กับส่วนอื่นๆ ของโปรแกรมได้

**Bad:** ตัวฟังก์ชันแก้ไขค่าของตัวแปร name โดยตรงและให้ผลลัพธ์เป็น array

```javascript
// Global variable referenced by following function.
// If we had another function that used this name, now it'd be an array and it could break it.
let name = "Ryan McDermott";

function splitIntoFirstAndLastName() {
  name = name.split(" ");
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

**Good:** นำตัวแปร newName มารับค่าจากการ spilt แทนทำให้ name ยังคงเป็น string เหมือนเดิม

```javascript
function splitIntoFirstAndLastName(name) {
  return name.split(" ");
}

const name = "Ryan McDermott";
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```

**[⬆ back to top](#table-of-contents)**



### หลีกเลี่ยง Side Effects (part 2)

ไม่ควรแก้ไข Object, Array โดยตรง เนื่องจากเ็นการ passed by Refference  
ตัวอย่างเช่นผู้ใช้งานรายหนึ่งกำลังซื้อของผ่านช่องทางออนไลน์ ได้เพิ่มสินค้าเข้าไป 5 ชิ้น
และกดปุ่มจ่ายเงิน ปรากฏว่าเกิด bad request และระหว่างที่รอ response นั้น  
ลูกค้าเผลอไปกดเพิ่มสินค้าโดยไม่ได้ตั้งใจ ทำให้ผู้ใช้งานต้องจ่ายเงินสินค้า 6 ชิ้น
ทั้งหมดนี้จะเกิดขึ้นหากเรานำ cart ที่เก็บสินค้าไปใช้งานโดยตรงเลย
วิธีแก้คือควรจะ clone cart แยกออกมาเพิ่อส่งไป purchase เพื่อหลีกเลี่ยงการแก้ไขโดยตรง  
กับข้อมูลประเภท Array , Object 

**Bad:**

```javascript
const addItemToCart = (cart, item) => {
  cart.push({ item, date: Date.now() });
};
```

**Good:**

```javascript
const addItemToCart = (cart, item) => {
  return [...cart, { item, date: Date.now() }];
};
```

**[⬆ back to top](#table-of-contents)**

### อย่าเขียน Global function

ตัวอย่างโค้ดที่ยกมาคือเป็นการเพิ่มเมธอดให้ Array.prototype  
แต่หากคนอืนเอาโค้ดไปรวมอาจจะทำให้ clash เพราะทุกคนก็ใช้ Array.prototype เหมือนกัน  
และอาจจะบังเอิญตั้งชื่อฟังก์ชันที่เพิ่มเข้าไปเหมือนกันด้วย  
กล่าวคืออย่าพยายามไปแก้ไขอะไรที่มัน original ให้ extends ออกมา 

**Bad:** ฟังก์ชัน diff ใน Array.prototype อยู่ใน global Scope

```javascript
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter(elem => !hash.has(elem));
};
```

**Good:** ใช้การ Extend Class แทนจะดีกว่า

```javascript
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter(elem => !hash.has(elem));
  }
}
```

**[⬆ back to top](#table-of-contents)**

### เน้นการเขียนแบบ Functional Programming มากว่า Imperative programming

การเขียนแบบ Imperative คือการอธิบายขั้นตอนการทำงานว่าโค้ดแต่ละบรรทัดมีการทำงานอย่างไร
ส่วนการเขียนแบบ Functional จะเน้นไปที่การประกาศตัวแปรแต่ละตัวว่าเป็อะไรแล้วค่อยประเมิณค่าทีเดียวตอน runtime 

**Bad:**

```javascript
const programmerOutput = [
  {
    name: "Uncle Bobby",
    linesOfCode: 500
  },
  {
    name: "Suzie Q",
    linesOfCode: 1500
  },
  {
    name: "Jimmy Gosling",
    linesOfCode: 150
  },
  {
    name: "Gracie Hopper",
    linesOfCode: 1000
  }
];

let totalOutput = 0;

for (let i = 0; i < programmerOutput.length; i++) {
  totalOutput += programmerOutput[i].linesOfCode;
}
```

**Good:**

```javascript
const programmerOutput = [
  {
    name: "Uncle Bobby",
    linesOfCode: 500
  },
  {
    name: "Suzie Q",
    linesOfCode: 1500
  },
  {
    name: "Jimmy Gosling",
    linesOfCode: 150
  },
  {
    name: "Gracie Hopper",
    linesOfCode: 1000
  }
];

const totalOutput = programmerOutput.reduce(
  (totalLines, output) => totalLines + output.linesOfCode,
  0
);
```

**[⬆ back to top](#table-of-contents)**

### ใช้การห่อหุ้มเงื่อนไข Encapsulate conditionals  

หากใส่เงื่อนไขยาวๆลงไปใน conditional statement จะทำให้อ่านยาก  
ให้นำเงื่อนไขที่ต้องการเช็คมาหุ้มด้วยฟังก์ชันแล้วรีเทิร์นค่าออกไปใช้ต่อจะดูอ่านง่ายกว่า
**Bad:**

```javascript
if (fsm.state === "fetching" && isEmpty(listNode)) {
  // ...
}
```

**Good:**

```javascript
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === "fetching" && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```

**[⬆ back to top](#table-of-contents)**

### หลีกเลี่ยงการใช้ negative conditionals  

ทำให้อ่านทำความเข้าใจยาก ต้องตีความซ้ำซ้อน

**Bad:**

```javascript
function isDOMNodeNotPresent(node) {
  // ...
}

if (!isDOMNodeNotPresent(node)) {
  // ...
}
```

**Good:**

```javascript
function isDOMNodePresent(node) {
  // ...
}

if (isDOMNodePresent(node)) {
  // ...
}
```

**[⬆ back to top](#table-of-contents)**

### หลีกเลี่ยงการใช้เงื่อนไข 

แนวคิดนี้อาจจะดูทำตามได้ยาก แต่เรายึดตามหลักที่ว่าหนึ่งฟังก์ชันควรจะทำหน้าที่แค่อย่างเดียวเท่านั้น  
การมีเงื่อนไขนั่นก็หมายความว่าฟังก์ชันเราทำงานได้หลายอย่างโดยปริยาย  
แนวคิดหลักที่เราจะใช้ก็คือ polymorphism ซึ่งก็ืคอการที่ออปเจ็คมีได้หลายรูปแบบ  
เราจะใช้การสร้างออปเจ็คที่มาจาก class ที่มี superclass เดียวกัน  
โดยใน subclass ใหม่ได้มีการกำหนดการทำงานใหม่ให้กับ method เพื่อให้ตรงจุดประสงค์ของ class นั้นๆ

 
**Bad:** ใช้ switch case 

```javascript
class Airplane {
  // ...
  getCruisingAltitude() {
    switch (this.type) {
      case "777":
        return this.getMaxAltitude() - this.getPassengerCount();
      case "Air Force One":
        return this.getMaxAltitude();
      case "Cessna":
        return this.getMaxAltitude() - this.getFuelExpenditure();
    }
  }
}
```

**Good:** extend class ออกมาและเพิ่มเมธอดของใครของมัน

```javascript
class Airplane {
  // ...
}

class Boeing777 extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getPassengerCount();
  }
}

class AirForceOne extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude();
  }
}

class Cessna extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getFuelExpenditure();
  }
}
```

**[⬆ back to top](#table-of-contents)**

### Avoid type-checking (part 1)

แม้ว่า JS จะเป็นภาษาแบบ Dynamics จนทำให้เราอยากเช็คประเภทของข้อมูลอยู่ตลอดเวลา  
แต่นั้นก็ขัดกับแนวคิดก่อนหน้านั้นว่า ถ้ามีเงื่อนไขฟังก์ชันก็จะทำงานได้มากกว่า 1 แบบ  
วิธีการแรกคือการพิจารณา APIs ที่ใช้อย่างสม่ำเสมอ

**Bad:** ตัดสินใจว่าจะปั่นหรือขับโดยดูว่าพาหนะเป็นชนิดใด

```javascript
function travelToTexas(vehicle) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(this.currentLocation, new Location("texas"));
  } else if (vehicle instanceof Car) {
    vehicle.drive(this.currentLocation, new Location("texas"));
  }
}
```

**Good:** เคลื่อนที่ไปเลยขอแค่เป็นยานพาหนะ

```javascript
function travelToTexas(vehicle) {
  vehicle.move(this.currentLocation, new Location("texas"));
}
```

**[⬆ back to top](#table-of-contents)**

### Avoid type-checking (part 2)

หากเราทำงานกับ string , integer และใช้แนวคิด polymorp ไม่ได้  
แต่ยังต้องการ type checking แนะนำให้ใช้ภาษา TypeScript แทน  
หากเราใช้ JS แล้วต้องมาคอยเช็คประเภทตัวแปรตลอดเวลา จะทำให้เราเขียนโค้ด  
โดยใช้คำฟุ่มเฟือย ไมดูคลีน และสูญเสียความสามารถในการอ่านทำความเข้าใจ
ถ้าจะไม่ใช้ TypeScript ก็ไม่ต้องเช็คไปเลยใน JS ดูคลีนกว่า

**Bad:**

```javascript
function combine(val1, val2) {
  if (
    (typeof val1 === "number" && typeof val2 === "number") ||
    (typeof val1 === "string" && typeof val2 === "string")
  ) {
    return val1 + val2;
  }

  throw new Error("Must be of type String or Number");
}
```

**Good:**

```javascript
function combine(val1, val2) {
  return val1 + val2;
}
```

**[⬆ back to top](#table-of-contents)**

### อย่าพยายาม optimize จนเกินไป 
โดยสรุปคือ Browser ยุคใหม่ๆค่อนข้างมี tool ที่ชวยเรา optimize มาในระดับหนึ่งแล้ว อ้างอิง [There are good
resources](https://github.com/petkaantonov/bluebird/wiki/Optimization-killers)

**Bad:** สมัยก่อนไม่สามารถดักจับ list.length ได้ในการ iterate แต่ละรอบ

```javascript 
// On old browsers, each iteration with uncached `list.length` would be costly
// because of `list.length` recomputation. In modern browsers, this is optimized.
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**Good:** ตอนนี้ไม่้องสร้างตัวแปรมารับค่า list.length แล้ว

```javascript
for (let i = 0; i < list.length; i++) {
  // ...
}
```

**[⬆ back to top](#table-of-contents)**

### กำจัด dead code

ไม่มีเหตุผลที่จะเก็บ Code ที่ไม่ใช้ไว้  
หากคุณคิดว่ามีความจำเป็นที่ต้องใช้งานมันอีกครั้งให้ใช้ version control แทน

**Bad:**

```javascript
function oldRequestModule(url) {
  // ...
}

function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

**Good:**

```javascript
function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

**[⬆ back to top](#table-of-contents)**

## **Objects and Data Structures**

### Use getters and setters

ใช้ getters และ setters เพื่อเข้าถึงข้อมูลของ object จะดีกว่าใช้การมองหา property ใน object และมีข้อดีที่ตามมาหลายๆอย่าง
- สามารถเพิ่ม validation rule ได้เมื่อทำการ `set`
- ง่ายต่อการ logging และ error handling 

**Bad:** เซตค่า property ของ Object โดยตรงโดยใช้ dot notation

```javascript
function makeBankAccount() {
  // ...

  return {
    balance: 0
    // ...
  };
}

const account = makeBankAccount();
account.balance = 100;
```

**Good:** สร้างฟังก์ชัน Getter กับ Setter ไว้ใช้งาน

```javascript
function makeBankAccount() {
  // this one is private
  let balance = 0;

  // a "getter", made public via the returned object below
  function getBalance() {
    return balance;
  }

  // a "setter", made public via the returned object below
  function setBalance(amount) {
    // ... validate before updating the balance
    balance = amount;
  }

  return {
    // ...
    getBalance,
    setBalance
  };
}

const account = makeBankAccount();
account.setBalance(100);
```

**[⬆ back to top](#table-of-contents)**

### Make objects have private members

สามารใช้ได้ผ่านการใช้ closures (สำหรับ ES5 ลงไป).


**Bad:** คนอื่นสามารถเข้าถึงแล้วลบข้อมูลได้
```javascript
const Employee = function(name) {
  this.name = name;
};

Employee.prototype.getName = function getName() {
  return this.name;
};

const employee = new Employee("John Doe");
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Employee name: undefined
```
**Good:** ไม่สามารถลบ name ได้โดยตรง (name เป็น private member)

```javascript
function makeEmployee(name) {
  return {
    getName() {
      return name;
    }
  };
}

const employee = makeEmployee("John Doe");
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
```

**[⬆ back to top](#table-of-contents)**

## **Classes**

### Prefer ES2015/ES6 classes over ES5 plain functions## **Classes**

### Prefer ES2015/ES6 classes over ES5 plain functions

It's very difficult to get readable class inheritance, construction, and method
definitions for classical ES5 classes. If you need inheritance (and be aware
that you might not), then prefer ES2015/ES6 classes. However, prefer small functions over
classes until you find yourself needing larger and more complex objects.

**Bad:** ใช้ method call เพื่อทำการ inheritance ไปเรื่อยๆจาก Animal > Mammal > Human

```javascript
const Animal = function(age) {
  if (!(this instanceof Animal)) {
    throw new Error("Instantiate Animal with `new`");
  }

  this.age = age;
};

Animal.prototype.move = function move() {};

const Mammal = function(age, furColor) {
  if (!(this instanceof Mammal)) {
    throw new Error("Instantiate Mammal with `new`");
  }

  Animal.call(this, age);
  this.furColor = furColor;
};

Mammal.prototype = Object.create(Animal.prototype);
Mammal.prototype.constructor = Mammal;
Mammal.prototype.liveBirth = function liveBirth() {};

const Human = function(age, furColor, languageSpoken) {
  if (!(this instanceof Human)) {
    throw new Error("Instantiate Human with `new`");
  }

  Mammal.call(this, age, furColor);
  this.languageSpoken = languageSpoken;
};

Human.prototype = Object.create(Mammal.prototype);
Human.prototype.constructor = Human;
Human.prototype.speak = function speak() {};
```
**Good:** ใช้การ extend Class แทน

```javascript
class Animal {
  constructor(age) {
    this.age = age;
  }

  move() {
    /* ... */
  }
}

class Mammal extends Animal {
  constructor(age, furColor) {
    super(age);
    this.furColor = furColor;
  }

  liveBirth() {
    /* ... */
  }
}

class Human extends Mammal {
  constructor(age, furColor, languageSpoken) {
    super(age, furColor);
    this.languageSpoken = languageSpoken;
  }

  speak() {
    /* ... */
  }
}
```

**[⬆ back to top](#table-of-contents)**

### Use method chaining

This pattern is very useful in JavaScript and you see it in many libraries such
as jQuery and Lodash. It allows your code to be expressive, and less verbose.
For that reason, I say, use method chaining and take a look at how clean your code
will be. In your class functions, simply return `this` at the end of every function,
and you can chain further class methods onto it.

**Bad:** ไม่รีเทิร์น this ทำให้ต้องเขียน Code 2 บรรทัดในการเรียกฟังก์ชันตั้งค่าสี setColor() และฟังก์ชัน save()

```javascript
class Car {
  constructor(make, model, color) {
    this.make = make;
    this.model = model;
    this.color = color;
  }

  setMake(make) {
    this.make = make;
  }

  setModel(model) {
    this.model = model;
  }

  setColor(color) {
    this.color = color;
  }

  save() {
    console.log(this.make, this.model, this.color);
  }
}

const car = new Car("Ford", "F-150", "red");
car.setColor("pink");
car.save();
```

**Good:** มีการรีเทิร์น this ทำให้สามารถใช้ method chaning ได้

```javascript
class Car {
  constructor(make, model, color) {
    this.make = make;
    this.model = model;
    this.color = color;
  }

  setMake(make) {
    this.make = make;
    // NOTE: Returning this for chaining
    return this;
  }

  setModel(model) {
    this.model = model;
    // NOTE: Returning this for chaining
    return this;
  }

  setColor(color) {
    this.color = color;
    // NOTE: Returning this for chaining
    return this;
  }

  save() {
    console.log(this.make, this.model, this.color);
    // NOTE: Returning this for chaining
    return this;
  }
}

const car = new Car("Ford", "F-150", "red").setColor("pink").save();
```

**[⬆ back to top](#table-of-contents)**

### Prefer composition over inheritance 

As stated famously in [_Design Patterns_](https://en.wikipedia.org/wiki/Design_Patterns) by the Gang of Four,
you should prefer composition over inheritance where you can. There are lots of
good reasons to use inheritance and lots of good reasons to use composition.
The main point for this maxim is that if your mind instinctively goes for
inheritance, try to think if composition could model your problem better. In some
cases it can.

You might be wondering then, "when should I use inheritance?" It
depends on your problem at hand, but this is a decent list of when inheritance
makes more sense than composition:

1. Your inheritance represents an "is-a" relationship and not a "has-a"
   relationship (Human->Animal vs. User->UserDetails).
2. You can reuse code from the base classes (Humans can move like all animals).
3. You want to make global changes to derived classes by changing a base class.
   (Change the caloric expenditure of all animals when they move).

**Bad:** ใช้ inheritance 

```javascript
class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  // ...
}

// ไม่ดี เพราะ Employees "มี" tax data  อีกนัยนึงคือ EmployeeTaxData ไม่ใช่'ประเภท'ของ Employee
class EmployeeTaxData extends Employee {
  constructor(ssn, salary) {
    super();
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}
```

**Good:** เปลี่ยนเป็นการใช้ Class Composition (ซ้อนกัน) แทนการใช้ Inheritance

```javascript
class EmployeeTaxData {
  constructor(ssn, salary) {
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}

class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  setTaxData(ssn, salary) {
    this.taxData = new EmployeeTaxData(ssn, salary);
  }
  // ...
}
```

**[⬆ back to top](#table-of-contents)**

## **SOLID**

### Single Responsibility Principle (SRP) - หลักการรับผิดชอบเพียงอย่างเดียว

As stated in Clean Code, "There should never be more than one reason for a class
to change". It's tempting to jam-pack a class with a lot of functionality, like
when you can only take one suitcase on your flight. The issue with this is
that your class won't be conceptually cohesive and it will give it many reasons
to change. Minimizing the amount of times you need to change a class is important.
It's important because if too much functionality is in one class and you modify
a piece of it, it can be difficult to understand how that will affect other
dependent modules in your codebase.


**Bad:** หนึ่งคลาสมีสองเมธอด อีกนัยนึงคือทำหน้าที่ได้หลายอย่าง

```javascript
class UserSettings {
  constructor(user) {
    this.user = user;
  }

  changeSettings(settings) {
    if (this.verifyCredentials()) {
      // ...
    }
  }

  verifyCredentials() {
    // ...
  }
}
```

**Good:** แต่ละคลาสมีเพียงเมธอดเดียว ทำให้ง่ายต่อการทำความเข้าใจ

```javascript
class UserAuth {
  constructor(user) {
    this.user = user;
  }

  verifyCredentials() {
    // ...
  }
}

class UserSettings {
  constructor(user) {
    this.user = user;
    this.auth = new UserAuth(user);
  }

  changeSettings(settings) {
    if (this.auth.verifyCredentials()) {
      // ...
    }
  }
}
```

**[⬆ back to top](#table-of-contents)**

### Open/Closed Principle (OCP)

As stated by Bertrand Meyer, "software entities (classes, modules, functions,
etc.) should be open for extension, but closed for modification." What does that
mean though? This principle basically states that you should allow users to
add new functionalities without changing existing code.  

ShortNote : Software Entities ต่างๆเช่น classes, modules, functions และอื่นๆ ควรมีลักษะที่ขยายง่าย (extension)แต่ยากสำหรับการแก้ไข (modification)

**Bad:** นำ Modification ของ Adapter ต่างๆมาเขียนเป็นฟังก์ชันด้านนอก

```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = "ajaxAdapter";
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = "nodeAdapter";
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    if (this.adapter.name === "ajaxAdapter") {
      return makeAjaxCall(url).then(response => {
        // transform response and return
      });
    } else if (this.adapter.name === "nodeAdapter") {
      return makeHttpCall(url).then(response => {
        // transform response and return
      });
    }
  }
}

function makeAjaxCall(url) {
  // request and return promise
}

function makeHttpCall(url) {
  // request and return promise
}
```

**Good:** นำ Modification ของ Adapter ต่างๆมาเขียนเป็น Method ด้านในของคลาสเลย  
จากตัวอย่างนี้ทำให้โค้ดส่วน HttpRequester ดูคลีนขึ้นด้วยเพราะไม่ต้องใช้ Conditional 

```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = "ajaxAdapter";
  }

  request(url) {
    // request and return promise
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = "nodeAdapter";
  }

  request(url) {
    // request and return promise
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    return this.adapter.request(url).then(response => {
      // transform response and return
    });
  }
}
```

**[⬆ back to top](#table-of-contents)**

### Liskov Substitution Principle (LSP)

This is a scary term for a very simple concept. It's formally defined as "If S
is a subtype of T, then objects of type T may be replaced with objects of type S
(i.e., objects of type S may substitute objects of type T) without altering any
of the desirable properties of that program (correctness, task performed,
etc.)." That's an even scarier definition.

The best explanation for this is if you have a parent class and a child class,
then the base class and child class can be used interchangeably without getting
incorrect results. This might still be confusing, so let's take a look at the
classic Square-Rectangle example. Mathematically, a square is a rectangle, but
if you model it using the "is-a" relationship via inheritance, you quickly
get into trouble.

ShorNote : คลาสย่อยจะต้องสามารถแทนที่สำหรับคลาสหลักของตัวเอง หรือกล่าวได้ว่า 
- ถ้าเรามีคลาส T1 ซึ่ง extend คลาส B แล้ว   
- ที่ใดก็ตามที่มีคลาส B ถูกใช้งานอยู่ (BaseClass)
- จะต้องสามารถสับเปลี่ยนไปใช้คลาส T1 ได้ (SubClass)
- โดยที่ต้องยังคงสามารถใช้งานได้เหมือนเดิม (ไม่มี Error)

**Bad:**
- คลาส Square (Subclass) ถูก Extend จาห Rectangle (BaseClass)  
- ต่อมามีการเรียกใช้เมธอด getArea (เป็นเมธอดของ BaseClass)
- อีกทั้งมีการเรียกใช้เมธอด render (เป็นเมธอดของ BaseClassเช่นกัน) 
- ส่งผลให้เกิด error เพราะตัว subClass ไม่มีเมธอด getArea 
- กล่าวคือ Square Class (SubClass) ไม่สามารถทำงานแทนที่ Reactangle Class (BaseClass) ของตัวเองได้

```javascript
class Rectangle {
  constructor() {
    this.width = 0;
    this.height = 0;
  }

  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }

  setWidth(width) {
    this.width = width;
  }

  setHeight(height) {
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  setWidth(width) {
    this.width = width;
    this.height = width;
  }

  setHeight(height) {
    this.width = height;
    this.height = height;
  }
}

function renderLargeRectangles(rectangles) {
  rectangles.forEach(rectangle => {
    rectangle.setWidth(4);
    rectangle.setHeight(5);
    const area = rectangle.getArea(); // BAD: Returns 25 for Square. Should be 20.
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**Good:**  
- คลาส Rectangle และ Square (Subclass) ถูก Extend จาห Shape (BaseClass)  
- ต่อมามีการเรียกใช้เมธอด getArea (เมธอดของSubClass)
- อีกทั้งมีการเรียกใช้เมธอด render (เมธอดของBaseClass) ได้โดยไม่ error

```javascript
class Shape {
  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }
}

class Rectangle extends Shape {
  constructor(width, height) {
    super();
    this.width = width;
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Shape {
  constructor(length) {
    super();
    this.length = length;
  }

  getArea() {
    return this.length * this.length;
  }
}

function renderLargeShapes(shapes) {
  shapes.forEach(shape => {
    const area = shape.getArea();
    shape.render(area);
  });
}

const shapes = [new Rectangle(4, 5), new Rectangle(4, 5), new Square(5)];
renderLargeShapes(shapes);
```

**[⬆ back to top](#table-of-contents)**

### Interface Segregation Principle (ISP)

JavaScript doesn't have interfaces so this principle doesn't apply as strictly
as others. However, it's important and relevant even with JavaScript's lack of
type system.

ISP states that "Clients should not be forced to depend upon interfaces that
they do not use." Interfaces are implicit contracts in JavaScript because of
duck typing.

A good example to look at that demonstrates this principle in JavaScript is for
classes that require large settings objects. Not requiring clients to setup
huge amounts of options is beneficial, because most of the time they won't need
all of the settings. Making them optional helps prevent having a
"fat interface".  

ShortNote : อย่าบังให้ให้ User ทำการ Implement interface โดยไม่จำเป็น  
ใน JS จะใช้ในบริบทที่บังคับให้ User ทำการ Setting บางค่าโดยไม่จำเป็น  

**Bad:** บังคับให้ User ทำการ Setting ค่าของ animationModule ทุกครั้ง (ซึ่งบ่อยครั้งไม่จำเป็นต้องใช้)

```javascript
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.settings.animationModule.setup();
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName("body"),
  animationModule() {} // Most of the time, we won't need to animate when traversing.
  // ...
});
```

**Good:** ใส่ Coditional ให้ animationModule ซึ่งเวลาเรียกใช้งานอาจจะมีค่านี้หรือไม่มีก็ได้  
ซึ่งเท่ากับว่า User จะทำการ Set ค่าก็ได้หรือไม่ก็ได้

```javascript
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.options = settings.options;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.setupOptions();
  }

  setupOptions() {
    if (this.options.animationModule) {
      // ...
    }
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName("body"),
  options: {
    animationModule() {}
  }
});
```

**[⬆ back to top](#table-of-contents)**

### Dependency Inversion Principle (DIP)

This principle states two essential things:

1. High-level modules should not depend on low-level modules. Both should
   depend on abstractions.  
   
2. Abstractions should not depend upon details. Details should depend on
   abstractions.  

ShortNote :  
1. High-level Code นั้นไม่ควรที่จะผูกติดอยู่กับ Low-level code แต่ทั้งสองนั้นควรจะผูกติดและขึ้นอยู่กับ Abstraction

2. และ Abstraction นั้นก็ไม่ควรผูกติดอยู่กับรายละเอียดการทำงาน แต่ในทางกลับกันรายละเอียดการทำงานควรจะผูกติดอยู่กับ Abstraction

This can be hard to understand at first, but if you've worked with AngularJS,
you've seen an implementation of this principle in the form of Dependency
Injection (DI). While they are not identical concepts, DIP keeps high-level
modules from knowing the details of its low-level modules and setting them up.
It can accomplish this through DI. A huge benefit of this is that it reduces
the coupling between modules. Coupling is a very bad development pattern because
it makes your code hard to refactor.

As stated previously, JavaScript doesn't have interfaces so the abstractions
that are depended upon are implicit contracts. That is to say, the methods
and properties that an object/class exposes to another object/class. In the
example below, the implicit contract is that any Request module for an
`InventoryTracker` will have a `requestItems` method.

**Bad:**

```javascript
class InventoryRequester {
  constructor() {
    this.REQ_METHODS = ["HTTP"];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryTracker {
  constructor(items) {
    this.items = items;

    // BAD: We have created a dependency on a specific request implementation.
    // We should just have requestItems depend on a request method: `request`
    this.requester = new InventoryRequester();
  }

  requestItems() {
    this.items.forEach(item => {
      this.requester.requestItem(item);
    });
  }
}

const inventoryTracker = new InventoryTracker(["apples", "bananas"]);
inventoryTracker.requestItems();
```

**Good:**

```javascript
class InventoryTracker {
  constructor(items, requester) {
    this.items = items;
    this.requester = requester;
  }

  requestItems() {
    this.items.forEach(item => {
      this.requester.requestItem(item);
    });
  }
}

class InventoryRequesterV1 {
  constructor() {
    this.REQ_METHODS = ["HTTP"];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryRequesterV2 {
  constructor() {
    this.REQ_METHODS = ["WS"];
  }

  requestItem(item) {
    // ...
  }
}

// By constructing our dependencies externally and injecting them, we can easily
// substitute our request module for a fancy new one that uses WebSockets.
const inventoryTracker = new InventoryTracker(
  ["apples", "bananas"],
  new InventoryRequesterV2()
);
inventoryTracker.requestItems();
```

**[⬆ back to top](#table-of-contents)**

## **Testing**

Testing is more important than shipping. If you have no tests or an
inadequate amount, then every time you ship code you won't be sure that you
didn't break anything. Deciding on what constitutes an adequate amount is up
to your team, but having 100% coverage (all statements and branches) is how
you achieve very high confidence and developer peace of mind. This means that
in addition to having a great testing framework, you also need to use a
[good coverage tool](https://gotwarlost.github.io/istanbul/).

There's no excuse to not write tests. There are [plenty of good JS test frameworks](https://jstherightway.org/#testing-tools), so find one that your team prefers.
When you find one that works for your team, then aim to always write tests
for every new feature/module you introduce. If your preferred method is
Test Driven Development (TDD), that is great, but the main point is to just
make sure you are reaching your coverage goals before launching any feature,
or refactoring an existing one.

**Bad:** เขียนเทสทีละหลายคอนเซป

```javascript
import assert from "assert";

describe("MomentJS", () => {
  it("handles date boundaries", () => {
    let date;

    date = new MomentJS("1/1/2015");
    date.addDays(30);
    assert.equal("1/31/2015", date);

    date = new MomentJS("2/1/2016");
    date.addDays(28);
    assert.equal("02/29/2016", date);

    date = new MomentJS("2/1/2015");
    date.addDays(28);
    assert.equal("03/01/2015", date);
  });
});
```

**Good:** เทสทีละคอนเซป

```javascript
import assert from "assert";

describe("MomentJS", () => {
  it("handles 30-day months", () => {
    const date = new MomentJS("1/1/2015");
    date.addDays(30);
    assert.equal("1/31/2015", date);
  });

  it("handles leap year", () => {
    const date = new MomentJS("2/1/2016");
    date.addDays(28);
    assert.equal("02/29/2016", date);
  });

  it("handles non-leap year", () => {
    const date = new MomentJS("2/1/2015");
    date.addDays(28);
    assert.equal("03/01/2015", date);
  });
});
```

**[⬆ back to top](#table-of-contents)**

## **Concurrency**

### Use Promises, not callbacks
ใช้ Promise ไม่ใช้ Callbacks เพราะไม่คลีนและอาจนำไปสู่ Callback Hell

Callbacks aren't clean, and they cause excessive amounts of nesting. With ES2015/ES6,
Promises are a built-in global type. Use them!

**Bad:** ใช้ Callback ซ้อนกันไปเรื่อยๆ

```javascript
import { get } from "request";
import { writeFile } from "fs";

get(
  "https://en.wikipedia.org/wiki/Robert_Cecil_Martin",
  (requestErr, response, body) => {
    if (requestErr) {
      console.error(requestErr);
    } else {
      writeFile("article.html", body, writeErr => {
        if (writeErr) {
          console.error(writeErr);
        } else {
          console.log("File written");
        }
      });
    }
  }
);
```

**Good:** ใช้ Promise แทน

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin")
  .then(body => {
    return writeFile("article.html", body);
  })
  .then(() => {
    console.log("File written");
  })
  .catch(err => {
    console.error(err);
  });
```

**[⬆ back to top](#table-of-contents)**

### Async/Await are even cleaner than Promises
ใช้ Async/Await จะดูคลีนกว่า Promise

Promises are a very clean alternative to callbacks, but ES2017/ES8 brings async and await
which offer an even cleaner solution. All you need is a function that is prefixed
in an `async` keyword, and then you can write your logic imperatively without
a `then` chain of functions. Use this if you can take advantage of ES2017/ES8 features
today!
 
**Bad:** ใช้ Promise 

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin")
  .then(body => {
    return writeFile("article.html", body);
  })
  .then(() => {
    console.log("File written");
  })
  .catch(err => {
    console.error(err);
  });
```

**Good:** ใช้ Async กับ Await แทน

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

async function getCleanCodeArticle() {
  try {
    const body = await get(
      "https://en.wikipedia.org/wiki/Robert_Cecil_Martin"
    );
    await writeFile("article.html", body);
    console.log("File written");
  } catch (err) {
    console.error(err);
  }
}

getCleanCodeArticle()
```

**[⬆ back to top](#table-of-contents)**

## **Error Handling**

Thrown errors are a good thing! They mean the runtime has successfully
identified when something in your program has gone wrong and it's letting
you know by stopping function execution on the current stack, killing the
process (in Node), and notifying you in the console with a stack trace.

### Don't ignore caught errors
อย่าลืมที่จะเขียนดัก error 

Doing nothing with a caught error doesn't give you the ability to ever fix
or react to said error. Logging the error to the console (`console.log`)
isn't much better as often times it can get lost in a sea of things printed
to the console. If you wrap any bit of code in a `try/catch` it means you
think an error may occur there and therefore you should have a plan,
or create a code path, for when it occurs.

**Bad:** เขียนดัก error โดยใช้แค่ console.log

```javascript
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}
```

**Good:** เขียนดัก error โดยใช้ console.error และอื่นๆ (เขียนดักแบบหลากหลาย)

```javascript
try {
  functionThatMightThrow();
} catch (error) {
  // One option (more noisy than console.log):
  console.error(error);
  // Another option:
  notifyUserOfError(error);
  // Another option:
  reportErrorToService(error);
  // OR do all three!
}
```

### Don't ignore rejected promises  
้เช่นเดียวกับหัวข้อก่อนหน้า อย่าลืมเขียนดัก reject จาก Promise

For the same reason you shouldn't ignore caught errors
from `try/catch`.

**Bad:** เขียนดัก error โดยใช้แค่ console.log

```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data);
  })
  .catch(error => {
    console.log(error);
  });
```

**Good:** เขียนดัก error โดยใช้ console.error และอื่นๆ (เขียนดักแบบหลากหลาย)

```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data);
  })
  .catch(error => {
    // One option (more noisy than console.log):
    console.error(error);
    // Another option:
    notifyUserOfError(error);
    // Another option:
    reportErrorToService(error);
    // OR do all three!
  });
```
**[⬆ back to top](#table-of-contents)**  

## **Formatting**

Formatting is subjective. Like many rules herein, there is no hard and fast
rule that you must follow. The main point is DO NOT ARGUE over formatting.
There are [tons of tools](https://standardjs.com/rules.html) to automate this.
Use one! It's a waste of time and money for engineers to argue over formatting.

For things that don't fall under the purview of automatic formatting
(indentation, tabs vs. spaces, double vs. single quotes, etc.) look here
for some guidance.

### Use consistent capitalization
ใช้ตัวใหญ่-ตัวเล็กในการประกาศค่าต่างๆเช่นตัวแปร,ฟังก์ชัน,คลาส และอื่น โดยมีรูปแบบเดิมอย่างเสมอต้นเสมอปลาย

JavaScript is untyped, so capitalization tells you a lot about your variables,
functions, etc. These rules are subjective, so your team can choose whatever
they want. The point is, no matter what you all choose, just be consistent.

**Bad:** ใช้รูปแบบการประกาศที่สลับไปสลับมา ตัวใหญ่บ้างเล็กบ้าง จนทำให้สับสน

```javascript
const DAYS_IN_WEEK = 7;
const daysInMonth = 30;

const songs = ["Back In Black", "Stairway to Heaven", "Hey Jude"];
const Artists = ["ACDC", "Led Zeppelin", "The Beatles"];

function eraseDatabase() {}
function restore_database() {}

class animal {}
class Alpaca {}
```

**Good:** ใช้รูปแบบการประกาศแบบเดิมตลอด

```javascript
const DAYS_IN_WEEK = 7;
const DAYS_IN_MONTH = 30;

const SONGS = ["Back In Black", "Stairway to Heaven", "Hey Jude"];
const ARTISTS = ["ACDC", "Led Zeppelin", "The Beatles"];

function eraseDatabase() {}
function restoreDatabase() {}

class Animal {}
class Alpaca {}
```

**[⬆ back to top](#table-of-contents)**  
### Function callers and callees should be close

If a function calls another, keep those functions vertically close in the source
file. Ideally, keep the caller right above the callee. We tend to read code from
top-to-bottom, like a newspaper. Because of this, make your code read that way.

**Bad:** เมธอด getPeerReview() ควรอยู่ข้างล่าง เมธอด perfReview()  
เพราะ perfReview เป็นตัว Caller เรียกใช้  getPeerReview ซึ่งถือว่าเป็น Calee

```javascript
class PerformanceReview {
  constructor(employee) {
    this.employee = employee;
  }

  lookupPeers() {
    return db.lookup(this.employee, "peers");
  }

  lookupManager() {
    return db.lookup(this.employee, "manager");
  }

  getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  perfReview() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.perfReview();
```

**Good:** เมธอด getPeerReview() อยู่ด้านบนเมธอด perfReview()  
ซึ่งเป้นการเรียงลำดับตาม Caller --> Callee


```javascript
class PerformanceReview {
  constructor(employee) {
    this.employee = employee;
  }

  perfReview() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();
  }

  getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  lookupPeers() {
    return db.lookup(this.employee, "peers");
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  lookupManager() {
    return db.lookup(this.employee, "manager");
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.perfReview();
```

**[⬆ back to top](#table-of-contents)**

## **Comments**

### Only comment things that have business logic complexity.
ใส่คอมเม้นเฉพาะส่วนที่ซับซ้อนเท่านั้น (อธิบายในส่วนที่อ่านยาก ไม่ใช่ต้องเขียนทุกส่วน)

Comments are an apology, not a requirement. Good code _mostly_ documents itself.

**Bad:** ใส่คอมเม้นแทบทุกบรรทัด อธิบายแบบ line by line 

```javascript
function hashIt(data) {
  // The hash
  let hash = 0;

  // Length of string
  const length = data.length;

  // Loop through every character in data
  for (let i = 0; i < length; i++) {
    // Get character code.
    const char = data.charCodeAt(i);
    // Make the hash
    hash = (hash << 5) - hash + char;
    // Convert to 32-bit integer
    hash &= hash;
  }
}
```

**Good:** ใส่คอมเม้นอธิบายเฉพาะส่วนที่อ่านหรือทำความเข้าใจยาก

```javascript
function hashIt(data) {
  let hash = 0;
  const length = data.length;

  for (let i = 0; i < length; i++) {
    const char = data.charCodeAt(i);
    hash = (hash << 5) - hash + char;

    // Convert to 32-bit integer
    hash &= hash;
  }
}
```

**[⬆ back to top](#table-of-contents)**

### Don't leave commented out code in your codebase

Version control exists for a reason. Leave old code in your history.

**Bad:** เปลี่ยนโค้ดที่ไม่ใช้แล้วเป็น Comment แทนที่จะลบทิ้งแล้วใช้ Version Control เช่น git แทน

```javascript
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**Good:** ใช้ Version Control ทำให้เก็บไว้เฉพาะโค้ดที่ยังต้องการใช้งาน

```javascript
doStuff();
```

**[⬆ back to top](#table-of-contents)**

### Don't have journal comments  
เช่นเดียวกับหัวข้อก่อนหน้า อย่าใช้คอมเม้นเป็นการบันทึกประวัติการเขียนโค้ด ให้ใช้ version control แทน

Remember, use version control! There's no need for dead code, commented code,
and especially journal comments. Use `git log` to get history!

**Bad:** จดรายละเอียดว่าเมื่อไหร่ทำอะไรบ้าง

```javascript
/**
 * 2016-12-20: Removed monads, didn't understand them (RM)
 * 2016-10-01: Improved using special monads (JP)
 * 2016-02-03: Removed type-checking (LI)
 * 2015-03-14: Added combine with type-checking (JR)
 */
function combine(a, b) {
  return a + b;
}
```

**Good:** ไม่ใช้คอมเม้นบันทึกประวัติ แต่ใช้ version control แทน

```javascript
function combine(a, b) {
  return a + b;
}
```

**[⬆ back to top](#table-of-contents)**

### Avoid positional markers

They usually just add noise. Let the functions and variable names along with the
proper indentation and formatting give the visual structure to your code.

**Bad:** ไม่ใช้ Comment ในการสร้างจุดสังเกตในการอ่าน Code 

```javascript
////////////////////////////////////////////////////////////////////////////////
// Scope Model Instantiation
////////////////////////////////////////////////////////////////////////////////
$scope.model = {
  menu: "foo",
  nav: "bar"
};

////////////////////////////////////////////////////////////////////////////////
// Action setup
////////////////////////////////////////////////////////////////////////////////
const actions = function() {
  // ...
};
```

**Good:** ใช้การตั้งชื่อ การจัดรูปแบบ(format,indent) ต่างๆช่วยในการอ่านโค้ดแทน

```javascript
$scope.model = {
  menu: "foo",
  nav: "bar"
};

const actions = function() {
  // ...
};
```

**[⬆ back to top](#table-of-contents)**

## Translation

This is also available in other languages:

- ![am](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Armenia.png) **Armenian**: [hanumanum/clean-code-javascript/](https://github.com/hanumanum/clean-code-javascript)
- ![bd](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Bangladesh.png) **Bangla(বাংলা)**: [InsomniacSabbir/clean-code-javascript/](https://github.com/InsomniacSabbir/clean-code-javascript/)
- ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Brazilian Portuguese**: [fesnt/clean-code-javascript](https://github.com/fesnt/clean-code-javascript)
- ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Simplified Chinese**:
  - [alivebao/clean-code-js](https://github.com/alivebao/clean-code-js)
  - [beginor/clean-code-javascript](https://github.com/beginor/clean-code-javascript)
- ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Traditional Chinese**: [AllJointTW/clean-code-javascript](https://github.com/AllJointTW/clean-code-javascript)
- ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **French**: [GavBaros/clean-code-javascript-fr](https://github.com/GavBaros/clean-code-javascript-fr)
- ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **German**: [marcbruederlin/clean-code-javascript](https://github.com/marcbruederlin/clean-code-javascript)
- ![id](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Indonesia.png) **Indonesia**: [andirkh/clean-code-javascript/](https://github.com/andirkh/clean-code-javascript/)
- ![it](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **Italian**: [frappacchio/clean-code-javascript/](https://github.com/frappacchio/clean-code-javascript/)
- ![ja](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [mitsuruog/clean-code-javascript/](https://github.com/mitsuruog/clean-code-javascript/)
- ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [qkraudghgh/clean-code-javascript-ko](https://github.com/qkraudghgh/clean-code-javascript-ko)
- ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **Polish**: [greg-dev/clean-code-javascript-pl](https://github.com/greg-dev/clean-code-javascript-pl)
- ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**:
  - [BoryaMogila/clean-code-javascript-ru/](https://github.com/BoryaMogila/clean-code-javascript-ru/)
  - [maksugr/clean-code-javascript](https://github.com/maksugr/clean-code-javascript)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [tureey/clean-code-javascript](https://github.com/tureey/clean-code-javascript)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Uruguay.png) **Spanish**: [andersontr15/clean-code-javascript](https://github.com/andersontr15/clean-code-javascript-es)
- ![tr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Turkey.png) **Turkish**: [bsonmez/clean-code-javascript](https://github.com/bsonmez/clean-code-javascript/tree/turkish-translation)
- ![ua](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Ukraine.png) **Ukrainian**: [mindfr1k/clean-code-javascript-ua](https://github.com/mindfr1k/clean-code-javascript-ua)
- ![vi](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **Vietnamese**: [hienvd/clean-code-javascript/](https://github.com/hienvd/clean-code-javascript/)

**[⬆ back to top](#table-of-contents)**
