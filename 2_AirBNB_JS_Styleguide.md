# Airbnb JavaScript Style Guide() {


## <a name='TOC'>สารบัญ</a>

  1. [Types](#types)
  1. [References](#references)
  1. [Objects](#objects)
  1. [Arrays](#arrays)
  1. [Destructuring](#destructuring)
  1. [Strings](#strings)
  1. [Functions](#functions)
  1. [Arrow Functions](#arrow-functions)
  1. [Classes & Constructors](#classes--constructors)
  1. [Modules](#modules)
  1. [Iterators and Generators](#iterators-and-generators)
  1. [Properties](#properties)
  1. [Variables](#variables)
  1. [Hoisting](#hoisting)
  1. [Comparison Operators & Equality](#comparison-operators--equality)
  1. [Blocks](#blocks)
  1. [Control Statements](#control-statements)
  1. [Comments](#comments)
  1. [Whitespace](#whitespace)
  1. [Commas](#commas)
  1. [Semicolons](#semicolons)
  1. [Type Casting & Coercion](#type-casting--coercion)
  1. [Naming Conventions](#naming-conventions)
  1. [Accessors](#accessors)
  1. [Events](#events)
  1. [jQuery](#jquery)
  1. [ECMAScript 5 Compatibility](#ecmascript-5-compatibility)
  1. [ECMAScript 6+ (ES 2015+) Styles](#ecmascript-6-es-2015-styles)
  1. [Standard Library](#standard-library)
  1. [Testing](#testing)
  1. [Performance](#performance)
  1. [Resources](#resources)
  1. [In the Wild](#in-the-wild)
  1. [Translation](#translation)
  1. [The JavaScript Style Guide Guide](#the-javascript-style-guide-guide)
  1. [Chat With Us About Javascript](#chat-with-us-about-javascript)
  1. [Contributors](#contributors)
  1. [License](#license)
  1. [Amendments](#amendments)

## Types

  - [1.1](#1.1) <a name='1.1'></a> **Primitives**: 
  
  การใช้งานตัวแปรประเภท Primitive จะเป็นการอ้างอิงโดยค่า(by Value)
  ตัวอย่าง ตัวแปรประเภท Primitive เช่น

    + `string`
    + `number`
    + `boolean`
    + `null`
    + `undefined`


  ตัวอย่างเช่นตัวแปร bar เก็บค่า 1 ไว้ จากนั้นสามารถนำไปใช้งานต่อได้โดยไม่เกี่ยวข้องกับ foo อีกต่อไป
    ```javascript
    const foo = 1;
    let bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```
  - [1.2](#1.2) <a name='1.2'></a> **Complex**: 

  การใช้งานตัวแปรที่ซับซ้อน(ที่ไม่ใช่ตัวแปร Primitive) จะเป็นการเก็บค่าโดยอ้างอิงจากที่อยู่ (by Reference)
  
    + `object`
    + `array`
    + `function`

  ตัวอย่างเช่น การแก้ไขอาเรย์ของ bar ทำให้อาเรย์ของ foo ถูกแก้ไขไปด้วย
    ```javascript
    const foo = [1, 2];
    const bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[[⬆ กลับไปด้านบน]](#TOC)**

## References

  - [2.1](#2.1) <a name='2.1'></a> ใช้ `const` สำหรับค่าคงที่ และหลีกเลี่ยงการใช้ `var`

    > เพราะการใช้งาน `const` เป็นการป้องกันไม่ให้ค่าถูกเปลี่ยนแปลงจากปัจจัยอื่นๆ เช่น   อาจมีกรณีที่เราลืมไปเขียนโค้ดเปลี่ยนแปลงค่าของตัวแปร หรือมีไลบรารี่อื่นๆที่เราใช้มาเปลี่ยนแปลงค่าตัวแปรของเรา

    ```javascript
    // ไม่ดี
    var a = 1;
    var b = 2;

    // ดี
    const a = 1;
    const b = 2;
    ```

  - [2.2](#2.2) <a name='2.2'></a> ถ้าอยากให้ตัวแปรเปลี่ยนแปลงค่าที่หลังได้`let` และอย่าใช้ `var`

    > เพราะ `let` จะสามารถใช้ได้แค่ใน Scope ที่เราประกาศเท่านั้น(ในปีกกา) เรียกใช้งานนอกปีกกาที่ประกาศไมได้    
     ส่วน `var` จะสามารถเรียกใช้ได้ภายในฟังก์ชันที่ประกาศ (Function Scope) 

    ```javascript
    // ไม่ดี
    var count = 1;
    if (true) {
      count += 1;
    }

    // ดี
    let count = 1;
    if (true) {
      count += 1;
    }
    ```

  - [2.3](#2.3) <a name='2.3'></a> `let` และ `const` จะมีค่าอยู่แค่ในปีกกาที่ประกาศ (Block-scoped) เท่านั้น  
  ไม่สามารถเรียกใช้งานได้จากด้านนอกของ Scope

    ```javascript
    {
      let a = 1;
      const b = 1;
    }
    console.log(a); // ReferenceError เมื่อออกนอกปีกกาที่ประกาศจะไม่สามารถเรียกใช้งานตัวแปรได้
    console.log(b); // ReferenceError เมื่อออกนอกปีกกาที่ประกาศจะไม่สามารถเรียกใช้งานตัวแปรได้
    ```

**[[⬆ กลับไปด้านบน]](#TOC)**

## Objects

  - [3.1](#3.1) <a name='3.1'></a> ใช้ปีกกา `{}` ในการประกาศออบเจ็กต์ ไม่ใช่ new Object  
  หรือเราอาจตั้งค่าใน eslint `no-new-object`

    ```javascript
    // ไม่ดี
    const item = new Object();

    // ดี
    const item = {};
    ```

  - [3.2](#3.2) <a name='3.2'></a> อย่าใช้คำสงวนจะทำให้มีปัญหาใน IE8 

    ```javascript
    // ไม่ดี
    const superman = {
      default: { clark: 'kent' }, // default เป็นคำสงวน
      private: true,
    };

    // ดี
    const superman = {
      defaults: { clark: 'kent' },
      hidden: true,
    };
    ```

  - [3.3](#3.3) <a name='3.3'></a> ใช้คำที่มีความหมายเหมือนกันแทนคำสงวนแต่ให้สื่อความหมายด้วย

    ```javascript
    // ไม่ดี
    const superman = {
      class: 'alien', // class เป็นคำสงวน
    };

    // ไม่ดี
    const superman = {
      klass: 'alien', // การแปลงคำทำให้เดาความหมายได้ยาก
    };

    // ดี เพราะใช้คำว่า type มาแทนคำว่า class
    const superman = {
      type: 'alien',
    };
    ```

  <a name="es6-computed-properties"></a>
  - [3.4](#3.4) <a name='3.4'></a> ถ้าต้องการสร้างพรอพเพอร์ตี้ของออบเจ็กต์จากตัวแปร (Dynamic property) ให้สร้างตอนที่ประกาศออบเจ็กต์โดยใช้วิธี computed property กล่าวคือใช้ `[]`
  ไม่ควรสร้างหลังจากประกาศออบเจ็กต์ไปแล้ว


    ```javascript

    function getKey(k) {
      return `a key named ${k}`;
    }

    // ไม่ดี
    const obj = {
      id: 5,
      name: 'San Francisco',
    };
    obj[getKey('enabled')] = true; // สร้างหลังจากประกาศออบเจ็กต์เสร็จแล้ว ทำให้มองยากกว่า

    // ดี สร้างตอนประกาศออบเจ็กต์
    const obj = {
      id: 5,
      name: 'San Francisco',
      [getKey('enabled')]: true, //  ทำให้มองเห็นพรอพเพอร์ตี้ของออบเจ็กต์ทั้งหมดในที่เดียว
    };
    ```

  <a name="es6-object-shorthand"></a>
  - [3.5](#3.5) <a name='3.5'></a> สร้าง Method ด้วยการเขียนย่อ (Method Shorthand) เพื่อทำให้ code อ่านง่ายขึ้น

    ```javascript
    // ไม่ดี
    const atom = {
      value: 1,

      addValue: function (value) { // การประกาศแบบปกติ
        return atom.value + value;
      },
    };

    // ดี
    const atom = {
      value: 1,

      addValue(value) { // การประกาศแบบย่อ จะตัดคีย์เวิร์ดฟังก์ชันออกไป ทำให้โค้ดอ่านง่ายขึ้น
        return atom.value + value;
      },
    };
    ```

  <a name="es6-object-concise"></a>
  - [3.6](#3.6) <a name='3.6'></a> เมื่อสร้าง Property ให้เขียนแบบย่อ (Property value shorthand) ถ้าหากค่า key และ value เหมือนกัน


    ```javascript
    const lukeSkywalker = 'Luke Skywalker';

    // ไม่ดี
    const obj = {
      lukeSkywalker: lukeSkywalker,
    };

    // ดี
    const obj = {
      lukeSkywalker, // มีค่าเท่ากับด้านบนเพียงแต่ทำให้อ่านง่ายขึ้น 
    };
    ```

  - [3.7](#3.7) <a name='3.7'></a> จัดกลุ่ม Property value shorthand แล้วลำดับไว้ที่ด้านบนสุดของการประกาศ key-value ใน Object นั้นๆเพราะทำให้หาง่ายกว่า

    

    ```javascript
    const anakinSkywalker = 'Anakin Skywalker';
    const lukeSkywalker = 'Luke Skywalker';

    // ไม่ดี เรียงแบบไม่มีรูปแบบ
    const obj = {
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      lukeSkywalker,
      episodeThree: 3,
      mayTheFourth: 4,
      anakinSkywalker,
    };

    // ดี
    const obj = {
      lukeSkywalker,
      anakinSkywalker,
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      episodeThree: 3,
      mayTheFourth: 4,
    };
    ```

**[[⬆ กลับไปด้านบน]](#TOC)**

## Arrays

  - [4.1](#4.1) <a name='4.1'></a> ใช้วงเล็บก้ามปู `[]` ในการประกาศอาร์เรย์ใช้ new Array();

    ```javascript
    // ไม่ดี
    const items = new Array();

    // ดี
    const items = [];
    ```

  - [4.2](#4.2) <a name='4.2'></a> ใช้การ push ค่าเข้าไปต่อท้าย

    ```javascript
    const someStack = [];

    // ไม่ดี เป็นการเพิ่ม element โดยคิดจากค่าความยาวของ array
    someStack[someStack.length] = 'abracadabra';

    // ดี
    someStack.push('abracadabra');
    ```

  <a name="es6-array-spreads"></a>
  - [4.3](#4.3) <a name='4.3'></a> ใช้ `...` (Spreads operator) ในการคัดลอกอาเรย์แทนการ loop

    ```javascript
    // ไม่ดี - ใช้การวนลูป
    const len = items.length;
    const itemsCopy = [];
    let i;

    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // ดี - ใช้ Spread Syntax
    const itemsCopy = [...items];
    ```
  - [4.4](#4.4) <a name='4.4'></a> ใช้ Array.from เมื่อต้องการเปลี่ยน Object ให้เป็น Array

    ```javascript
    const foo = document.querySelectorAll('.foo');
    const nodes = Array.from(foo);
    ```

**[[⬆ กลับไปด้านบน]](#TOC)**

## Destructuring

  - [5.1](#5.1) <a name='5.1'></a> ใช้การ `Destructuring` เมื่อต้องการนำค่า Property ของ Object มาเก็บไว้ในตัวแปร
    

    ```javascript
    // ไม่ดี - เป็นการสร้างตัวแปรเพิ่มชั่วคราวมารับค่า
    function getFullName(user) {
      const firstName = user.firstName; 
      const lastName = user.lastName; 

      return `${firstName} ${lastName}`;
    }

    // ดี - ใช้ Destructuring ก่อนแล้วนำค่าไปใช้ต่อ
    function getFullName(obj) {
      const { firstName, lastName } = obj; 
      return `${firstName} ${lastName}`;
    }

    // ดีที่สุด - รับค่าโดยใช้ Destructuring
    function getFullName({ firstName, lastName }) { 
      return `${firstName} ${lastName}`;
    }
    ```

  - [5.2](#5.2) <a name='5.2'></a> ใช้การ `Destructuring` เมื่อต้องการแปลง Element ของ Array มาเก็บเป็นตัวแปร

    ```javascript
    const arr = [1, 2, 3, 4];

    // ไม่ดี - เพราะสร้างตัวแปรช่วงคราวมารับค่า
    const first = arr[0];
    const second = arr[1];

    // ดี - ใช้ Destructuring ในการแปลงค่า element ของอาเรย์ให้เป็นตัวแปร
    const [first, second] = arr; 
    ```

  - [5.3](#5.3) <a name='5.3'></a> เมื่อต้องการคืนค่าออกจากฟังก์ชันหลายค่า ใช้ `Destructuring` ในรูปแบบ object ไม่ใช้การคืนค่าในรูปแบบของ Array เพราะ   
  - Object ลำดับการส่งคืนค่าไม่สำคัญ  
  - แต่ใน Array ลำดับการส่งคืนค่าสำคัญ 

   

    ```javascript
    // ไม่ดี - เพราะ return ในรูปของ Array
    function processInput(input) {
      return [left, right, top, bottom];
    }

    // คนที่เรียกใช้งานฟังก์ชันจะต้องคำนึงถึงลำดับของตัวแปรที่จะส่งไป
    const [left, __, top] = processInput(input);

    // ดี - เพราะ return ในรูปของ object
    function processInput(input) {
      return { left, right, top, bottom };
    }

    // คนที่เรียกใช้งานฟังก์ชันสามารถส่งเฉพาะค่าที่ตนเองต้องการ โดยมันจะจับคู่ให้อัตโนมัติ
    const { left, right } = processInput(input);
    ```


**[[⬆ กลับไปด้านบน]](#TOC)**

## Strings

  - [6.1](#6.1) <a name='6.1'></a> ใช้ Single quotes `''` สำหรับตัวแปรประเภท String

    ```javascript
    // ไม่ดี
    const name = "Capt. Janeway";

    // ดี
    const name = 'Capt. Janeway';
    ```

  - [6.2](#6.2) <a name='6.2'></a> สตริงที่ยาวกว่า 80 ตัวอักษร ควรจะแยกเขียนในหลายบรรทัด และค่อยนำมาเชื่อมต่อกันภายหลัง
  - [6.3](#6.3) <a name='6.3'></a> หมายเหตุ: ไม่ควรใช้สตริงที่ยาวมากเกินไป เพราะทำให้ Performance แย่ลง  

    ```javascript
    // ไม่ดี
    const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';

    // ไม่ดี
    const errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // ดี
    const errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';
    ```

  <a name="es6-template-literals"></a>
  - [6.4](#6.4) <a name='6.4'></a> เมื่อต้องการสร้างสตริงที่รับค่าตัวแปรได้ ให้ใช้ template literal แทนการ concacenation (การเชื่อมสตริงเองด้วยเครื่องหมายบวก) จะทำให้อ่านง่ายกว่า

  

    ```javascript
    // ไม่ดี - ใช้ concacenation
    function sayHi(name) {
      return 'How are you, ' + name + '?';
    }

    // ไม่ดี - ใช้การ join
    function sayHi(name) {
      return ['How are you, ', name, '?'].join();
    }

    // ดี - ใช้ template literal
    function sayHi(name) {
      return `How are you, ${name}?`;
    }
    ```

**[[⬆ กลับไปด้านบน]](#TOC)**


## Functions

  - [7.1](#7.1) <a name='7.1'></a> การประกาศฟังก์ชันให้ใช้แบบ Declaration ไม่ใช้แบบ Expression เพราะ 
  1. Function declarations มีชื่อให้เห็นชัดเจน เมื่อทำการดีบัคโค้ดจะสามารถเห็นชื่อฟังก์ชันใน Call stacks 
  2. นอกจากนั้นจาวาสคริปต์จะ Hoisting ฟังก์ชันที่ประกาศแบบ Function declarations (ตอนCompile จะนำไปอยู่ด้านบนสุดของ Scope) ทำให้สามารถเรียกใช้ฟังก์ชันได้ทุกที่

  เสริม : เมื่อใดที่ต้องการใช้งาน Function expressions ให้ใช้ [Arrow Functions](#arrow-functions) แทนเสมอ

    ```javascript
    // ไม่ดี
    const foo = function () {
    };

    // ดี
    function foo() {
    }
    ```

  - [7.2](#7.2) <a name='7.2'></a> Function expressions - คือการประกาศฟังก์ชันและใช้ตัวแปรมารับค่าเพื่อใช้อ้างอิงภายหลัง  หรืออาจจะไม่ใช้ตัวแปรมารับค่าก็ได้ในกรณีที่เป็น Anonymous function(ฟังก์ชันไร้ชื่อ)

    ```javascript
    // immediately-invoked function expression (IIFE)
    (() => {
      console.log('Welcome to the Internet. Please follow me.');
    })();
    ```

  - [7.3](#7.3) <a name='7.3'></a> ภายใน conditional Statement เช่น if, else, while, และ อื่นๆ  
  อย่าพยายามประกาศฟังก์ชัน ถ้าจำเป็นต้องประกาศ ให้ประกาศในรูปแบบของ Function Expressions (ดู 7.4 เพิ่มเติม) ไม่ใช่ Function Declarations (อาจทำให้บราวเซอร์จะตีความหมายผิด)   
  
  

  - [7.4](#7.4) <a name='7.4'></a> **หมายเหตุ:** ECMA-262 บอกไว้ว่าใน if, else, while, และอื่นๆ ต้องประกอบไปด้วย statements เท่านั้น ซึ่ง Function Declarations ไม่นับเป็น statement 

    ```javascript
    // ไม่ดี
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // ดี
    let test;
    if (currentUser) {
      test = () => {
        console.log('Yup.');
      };
    }
    ```
<!-- ################ ทำถึงตรงนี้แล้ว ########################## -->
  - [7.5](#7.5) <a name='7.5'></a> ห้ามตั้งชื่อพารามิเตอร์ว่า `arguments` เพราะจะไปทับซ้อนออบเจ็กต์ `arguments` ที่ตัวภาษาจาวาสคริปต์มีไว้ให้อยู่แล้วในทุกๆฟังก์ชัน

    ```javascript
    // ไม่ดี
    function nope(name, options, arguments) {
      // ...stuff...
    }

    // ดี - ใช้คำว่า args แทนคำว่า arguments
    function yup(name, options, args) {
      // ...stuff...
    }
    ```

  <a name="es6-rest"></a>
  - [7.6](#7.6) <a name='7.6'></a> ให้ใช้ `...` (Rest Parament) แทนการใช้ `arguments` เพราะว่า  
    > 1. `...` สามารถทำให้รู้ว่าฟังก์ชันนั้นมีการรับค่าพารามิเตอร์  
    > 2. อีกทั้ง `...` จะได้ค่าอาร์เรย์จริงๆ ไม่ใช่ค่าออบเจ็กต์เหมือน `arguments`

    ```javascript
    // ไม่ดี - รับมาเป็น Object แล้วนำไปทำเป็น Array อีกที
    // แถมดูยากด้วยว่าฟังก์ชันนี้รับพารามิเตอร์หรือไม่รับกันแน่
    function concatenateAll() {
      const args = Array.prototype.slice.call(arguments);
      return args.join('');
    }

    // ดี - ทำให้รู้ว่าฟังก์ชันนี้รับพารามิเตอร์ได้หลายค่า
    function concatenateAll(...args) { 
      return args.join('');
    }
    ```

  <a name="es6-default-parameters"></a>
  - [7.7](#7.7) <a name='7.7'></a> ใส่ค่า default ให้กับพารามิเตอร์ทุกตัว

    ```javascript
    // แย่มาก
    function handleThings(opts) {
      // ไม่ควรเปลี่ยนค่าของพารามิเตอร์หลังจากรับค่ามาแล้ว 
      opts = opts || {};
      // ...
    }

    // แย่ - มี conditional
    function handleThings(opts) {
      if (opts === void 0) {
        opts = {};
      }
      // ...
    }

    // ดี - ใส่ Default Value
    function handleThings(opts = {}) {
      // ...
    }
    ```

  - [7.8](#7.8) <a name='7.8'></a> หลีกเลี่ยงการตั้งค่า defualt แบบซับซ้อน (ทำให้สับสนง่าย)


  ```javascript
  var b = 1;
  // ไม่ดี - ทุกครั้งที่เรียกฟังก์ชันแบบไม่มีพารามิเตอร์ค่า b จะเปลี่ยนไปเรื่อยๆ
  function count(a = b++) {
    console.log(a);
  }
  count();  // 1
  count();  // 2
  count(3); // 3 เพราะว่ามีการกำหนดอาร์กิวเมนต์เป็น 3 ดังนั้นค่าเริ่มต้นจะไม่ถูกเรียก (= b++ ไม่ถูกเรียก)
  count();  // 3
  ```


**[[⬆ กลับไปด้านบน]](#TOC)**

## Arrow Functions

  - [8.1](#8.1) <a name='8.1'></a> ใช้ `Arrow Functions` เมื่อต้องการสร้าง `Function expressions` รวมถึง `Anonymous function`

    > เพราะค่าของ this ใน Arrow functions จะมีค่าเท่ากับค่า this ของฟังก์ชันที่ห่อหุ้ม Arrow functions อยู่

    > แต่หากถ้าฟังก์ชันยาวมากๆ ให้แยกออกมาเป็น Function declarations แทนจะดีกว่า

    ```javascript
    // ไม่ดี - ทำเป็น Declaration
    [1, 2, 3].map(function (x) {
      return x * x;
    });

    // ดี - ทำเป็น Expression
    [1, 2, 3].map((x) => {
      return x * x;
    });
    ```

  - [8.2](#8.2) <a name='8.2'></a> ถ้าฟังก์ชันมีแค่บรรทัดเดียวให้ลบ `()` ออกได้ และหากมีเพียงบรรทัดเดียว ให้ลบ {}` ออกได้ แต่ในกรณีอื่นๆให้ใช้ `{}`, `()` และ `return` คีย์เวิร์ด


    ```javascript
    // ดี
    [1, 2, 3].map(x => x * x); // จะสังเกตว่า Arrow functions ที่สั้นๆแบบนี้เมื่อไม่ใส่ {} และ () แล้วทำให้ดูง่ายขึ้น

    // ดี
    [1, 2, 3].reduce((total, n) => { // Arrow functions ที่ซับซ้อนมากขึ้นควรใส่ {} และ ()
      return total + n;
    }, 0);
    ```

**[[⬆ กลับไปด้านบน]](#TOC)**


## Classes & Constructors

  - [9.1](#9.1) <a name='9.1'></a> ใช้ `class` และพยายามหลีกเลี่ยงการเรียกใช้ `prototype` 


    ```javascript
    // ไม่ดี
    function Queue(contents = []) {
      this._queue = [...contents];
    }
    Queue.prototype.pop = function() {
      const value = this._queue[0];
      this._queue.splice(0, 1);
      return value;
    }

    // ดี - นำฟังก์ชัน pop มาใส่เป็นเมธอดของคลาส แทนที่จะอ้างอิงถึง prototype
    class Queue {
      constructor(contents = []) {
        this._queue = [...contents];
      }
      pop() {
        const value = this._queue[0];
        this._queue.splice(0, 1);
        return value;
      }
    }
    ```

  - [9.2](#9.2) <a name='9.2'></a> ใช้ `extends` ในการสืบทอดคุณสมบัติของคลาส

    > ใน ES6 วิธีนี้ช่วยให้ฟังก์ชัน `instanceof` ทำงานได้อย่างถูกต้อง

    ```javascript
    // ไม่ดี
    const inherits = require('inherits');
    function PeekableQueue(contents) {
      Queue.apply(this, contents);
    }
    inherits(PeekableQueue, Queue);
    PeekableQueue.prototype.peek = function() {
      return this._queue[0];
    }

    // ดี - เพราะใช้การ extend แทน prototype
    class PeekableQueue extends Queue {
      peek() {
        return this._queue[0];
      }
    }
    ```

  - [9.3](#9.3) <a name='9.3'></a> Method ใดๆควรคืนค่า `this` เพื่อทำให้สามารถใช้ Method chaining

    ```javascript
    // ไม่ดี 
    Jedi.prototype.jump = function() {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function(height) {
      this.height = height;
    };

    const luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20); // => undefined ใช้งานเมธอดต่อไม่ได้แล้ว 

    // ดี - สามารถ Chain Method ได้ทำให้โค้ดอ่านง่ายขึ้น
    class Jedi {
      jump() {
        this.jumping = true;
        return this;
      }

      setHeight(height) {
        this.height = height;
        return this;
      }
    }

    const luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```


  - [9.4](#9.4) <a name='9.4'></a> สามารถทำการเขียนทับเมธอด toString() ที่ Javascript มีให้อยู่แล้วได้ ได้แต่ควรจะตรวจสอบให้มั่นใจว่าจะไม่เกิดข้อผิดพลาดขึ้นในอนาคต

    ```javascript
    class Jedi {
      constructor(options = {}) {
        this.name = options.name || 'no name';
      }

      getName() {
        return this.name;
      }

      toString() {
        return `Jedi - ${this.getName()}`;
      }
    }
    ```

  - [9.5](#9.5) <a name="9.5"></a> ถ้า constructor ไม่ได้สร้างค่าอะไรใหม่ก็ไม่ควรเขียนลงไป  (๋JS มี default constructor ให้อยู่แล้ว)

    ```javascript
    // ไม่ดี - เพราะ Constructor ไม่ได้สร้างค่าอะไรใหม่
    class Jedi {
      constructor() {}

      getName() {
        return this.name;
      }
    }

    // ไม่ดี
    class Rey extends Jedi {
      constructor(...args) {
        super(...args);
      }
    }

    // ดี - ตัว Constructor ได้ใช้สร้าง key-value
    class Rey extends Jedi {
      constructor(...args) {
        super(...args);
        this.name = 'Rey';
      }
    }
    ```

  - [9.6](#9.6) <a name="9.6"></a> หลีกเลี่ยงการใช้ชื่อ method ซ้ำกันใน class เดียวกัน (อาจทำให้เกิดบัค)

    

    ```javascript
    // ไม่ดี
    class Foo {
      bar() { return 1; }
      bar() { return 2; }
    }

    // ดี
    class Foo {
      bar() { return 1; }
    }

    // ดี
    class Foo {
      bar() { return 2; }
    }
    ```

**[[⬆ กลับไปด้านบน]](#TOC)**


## Modules

  - [10.1](#10.1) <a name='10.1'></a> ควรใช้งานโมดูลในรูปแบบที่ ES6 มีให้ (`import`/`export`) แทนการใช้งานโมดูลรูปแบบอื่นๆ เนื่องจากเราสามารถที่จะคอมไพล์ไฟล์เป็นโมดูลในระบบอื่นๆในภายหลังได้

 

    ```javascript
    // ไม่ดี
    const AirbnbStyleGuide = require('./AirbnbStyleGuide');
    module.exports = AirbnbStyleGuide.es6;

    // ok
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    export default AirbnbStyleGuide.es6;

    // ดีที่สุด - รับมาแบบ Destructuring
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

  - [10.2](#10.2) <a name='10.2'></a> หลีกเลี่ยงการใช้ `*` ในการอิมพอร์ต (หมายถึงการ import ทั้งหมด)

    ```javascript
    // ไม่ดี
    import * as AirbnbStyleGuide from './AirbnbStyleGuide';

    // ดี
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    ```

  - [10.3](#10.3) <a name='10.3'></a>หลีกเลี่ยงการเอ็กพอร์ตทันทีหลังจาก import (อ่านยาก)

    ```javascript
    // ไม่ดี
    export { es6 as default } from './airbnbStyleGuide';

    // ดี - เพราะแยกบรรทัด Import Export ออกจากกัน อ่านง่ายกว่า
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

**[[⬆ กลับไปด้านบน]](#TOC)**

## Iterators and Generators

  - [11.1](#11.1) <a name='11.1'></a> ใช้ `map()` และ `reduce()` แทนการใช้งานลูปอย่าง `for-of`(หลีกเลี่ยงการใช้ `Iterators`)

    ```javascript
    const numbers = [1, 2, 3, 4, 5];

    // ไม่ดี
    let sum = 0;
    for (let num of numbers) {
      sum += num;
    }

    sum === 15;

    // ดี
    let sum = 0;
    numbers.forEach((num) => sum += num);
    sum === 15;

    // ดีที่สุด (use the functional force)
    const sum = numbers.reduce((total, num) => total + num, 0);
    sum === 15;
    ```

  - [11.2](#11.2) <a name='11.2'></a> หลีกเลี่ยงการใช้งาน `Generators` (ณ ปัจจุบัน)
 เพราะว่ายังไม่สามารถคอมไพล์กลับไปเป็น ES5 ได้อย่างสมบูรณ์

**[[⬆ กลับไปด้านบน]](#TOC)**


## Properties

  - [12.1](#12.1) <a name='12.1'></a> ใช้จุด `.` ในการเข้าถึงพรอพเพอร์ตี้ (properties) ที่ทราบค่า

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    // ไม่ดี
    const isJedi = luke['jedi'];

    // ดี
    const isJedi = luke.jedi;
    ```

  - [12.2](#12.2) <a name='12.2'></a> ใช้วงเล็บก้ามปู `[]` ในการเข้าถึงพรอพเพอร์ตี้ที่เป็นตัวแปร

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };
    // ฟังก์ชันนี้จะคืนค่า property ของ object ที่มีชื่อว่า luke 
    // โดยใช้ [] เพื่อให้ user เป็นคนกำหนดเองว่าจะเลือก key อันไหนจาก object
    function getProp(prop) {
      return luke[prop];
    }

    const isJedi = getProp('jedi');
    ```

**[[⬆ กลับไปด้านบน]](#TOC)**


## Variables

  - [13.1](#13.1) <a name='13.1'></a> ใช้ `const` ในการประกาศตัวแปรเสมอ ถ้าไม่ใช้คำขึ้นต้นจะทำให้ตัวแปรเป็น `global` ซึ่งอาจมีผลต่อไฟล์หรือโมดูลอื่นๆ

    ```javascript
    // ไม่ดี
    superPower = new SuperPower();

    // ดี
    const superPower = new SuperPower();
    ```

  - [13.2](#13.2) <a name='13.2'></a> ใช้ `const`หนึ่งครั้งต่อการประกาศตัวแปรหนึ่งตัว (อ่านง่ายกว่าและป้องกันการลืมเขียนคอมม่า `,` ) 

    

    ```javascript
    // ไม่ดี
    const items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // ไม่ดี - เขียน comma บ้าง semi-colon บ้าง
    // (compare to above, and try to spot the mistake)
    const items = getItems(),
        goSportsTeam = true;
        dragonball = 'z';

    // ดี - ประกาศแยกของใครของมันไปเลย
    const items = getItems();
    const goSportsTeam = true;
    const dragonball = 'z';
    ```

  - [13.3](#13.3) <a name='13.3'></a> ลำดับของประเภทตัวแปร  
  - ประกาศ `const` ไว้ที่เดียวกันเป็นกลุ่ม  
  - จากนั้นตามด้วยการประกาศ `let` ไว้ที่เดียวกัน 
  - ส่วนตัวแปรที่ยังไม่ทราบค่าให้เขียนไว้ด้านล่างเสมอจะได้อ่านง่าย (อย่าสลับไปมา)  


    ```javascript
    // ไม่ดี
    let i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // ไม่ดี
    let i;
    const items = getItems();
    let dragonball;
    const goSportsTeam = true;
    let len;

    // ดี
    const goSportsTeam = true;
    const items = getItems();
    let dragonball;
    let i;
    let length;
    ```

  - [13.4](#13.4) <a name='13.4'></a> ประกาศตัวแปรในที่ๆเหมาะสม (จากวิจารณญาณ โดยดูจากความเป็นระเบียบและการอ่านง่ายเป็นหลัก)


    ```javascript
    // ดี
    function() {
      test();
      console.log('doing stuff..');

      //..other stuff..

      const name = getName();

      if (name === 'test') {
        return false;
      }

      return name;
    }

    // ไม่ดี 
    function(hasName) {
      const name = getName(); //อยู่ห่างจากฟังก์ชันที่เรียดใช้

      if (!hasName) {
        return false;
      }

      this.setFirstName(name);

      return true;
    }

    // ดี - ประกาศสิ่งที่ต้องใช้ด้วยกันให้อยู่บรรทัดใกล้ๆกัน
    function(hasName) {
      if (!hasName) {
        return false;
      }

      const name = getName();
      this.setFirstName(name);

      return true;
    }
    ```

**[[⬆ กลับไปด้านบน]](#TOC)**


## Hoisting

  - [14.1](#14.1) <a name='14.1'></a> เวลาคอมไพล์จาวาสคริปต์จะอ่านตัวแปร `var` ที่ประกาศไว้ก่อนหน้าสิ่งอื่นๆในสโคป แต่ค่าที่ใส่ให้ตัวแปรจะยังไม่ถูกอ่าน ส่วนการประกาศ `const` และ `let` จะใช้วิธีการใหม่ที่เรียกว่า [Temporal Dead Zones (TDZ)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_dead_zone_and_errors_with_let) อ่านเหตุผลของวิธีการใหม่นี้ได้จาก [typeof is no longer safe](http://es-discourse.com/t/why-typeof-is-no-longer-safe/15)

    > โดยสรุปคือ  
    1.สำหรับ `let` และ `const` นั้น ภายในปีกกาเดียวกันจะไม่สามารถประกาศซ้ำกันสองครั้งได้  
    2.ตัวแปรจะมีค่าก็ต่อเมื่อคอมไพล์เลอร์อ่านถึงบรรทัดที่ประกาศตัวแปร หากมีการเรียกใช้งานตัวแปรก่อนบรรทัดที่ประกาศตัวแปรจะได้ผลลัพธ์เป็น ReferenceError แทนที่จะได้ undefined เหมือนใน ES5

    ```javascript
    // สมมติว่าเราไม่ได้ประกาศตัวแปร notDefined
    function example() {
      console.log(notDefined); // => throws a ReferenceError
    }

    // ประกาศตัวแปรหลังจากใช้งานนั้นทำได้ (ไม่มี error)
    // เพราะว่าตัวแปรจะถูกคอมไพล์และดึงขึ้นมาไว้ข้างบนสโคป
    // แต่ค่าของตัวแปรไม่ได้ถูกดึงขึ้นมาด้วย(Hoisting) จึงทำให้ค่าของตัวแปรนั้นเป็น undefined
    function example() {
      console.log(declaredButNotAssigned); // => undefined ใน ES5
      var declaredButNotAssigned = true;
    }

    // ตัวอย่างเมื่อคอมไพล์เลอร์ทำงานในตัวอย่างข้างต้น
    // คอมไพล์เลอร์จะอ่านตัวแปรและดึงขึ้นมาไว้ด้านบนของสโคป
    function example() {
      var declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined ใน ES5
      declaredButNotAssigned = true;
    }

    // using const and let
    function example() {
      console.log(declaredButNotAssigned); // => throws a ReferenceError
      console.log(typeof declaredButNotAssigned); // => throws a ReferenceError
      const declaredButNotAssigned = true;
    }
    ```
<!-- ####################### ทำถึงตรงนี้จ้า ################################ -->
  - [14.2](#14.2) <a name='14.2'></a> Anonymous function expressions  
  การประกาศฟังก์ชันโดยไม่ใส่ชื่อฟังก์ชัน เมื่อคอมไพล์ จะอ่านตัวแปรและดึงค่าขึ้นไปด้านบนของสโคป แต่จะยังไม่อ่านฟังก์ชัน หากมีการเรียกใช้งานก่อนบรรทัดที่ประกาศฟังก์ชันจะไม่สามารถเรียกใช้งานได้

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function() {
        console.log('anonymous function expression');
      };
    }
    ```

  - [14.3](#14.3) <a name='14.3'></a> Named function expressions - การประกาศฟังก์ชันโดยใส่ชื่อฟังก์ชัน ได้ผลลัพธ์เหมือนตัวอย่างก่อนหน้า

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };
    }

    // ประกาศฟังก์ชันชื่อเดียวกับตัวแปร ก็ได้ผลลัพธ์เช่นเดียวกันกับตัวอย่างก่อนหน้า
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      var named = function named() {
        console.log('named');
      }
    }
    ```

  - [14.4](#14.4) <a name='14.4'></a> Function declarations - การประกาศฟังก์โดยไม่ได้นำตัวแปรมารับค่า(Declaration) เมื่อคอมไพล์จะอ่านทั้งชื่อและฟังก์ชัน
    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

   - อ่านเพิ่มเติมได้ที่ [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting) โดย [Ben Cherry](http://www.adequatelygood.com/)

**[[⬆ กลับไปด้านบน]](#TOC)**


## Comparison Operators & Equality

  - [15.1](#15.1) <a name='15.1'></a> ใช้ `===` และ `!==` แทน `==` และ `!=` (เช็คแบบ Strong Type)
  - [15.2](#15.2) <a name='15.2'></a> หากใช้ตัวดำเนินการแบบเปรียบเทียบ  
  จาวาสคริปต์จะแปลงค่าเหล่านั้นเป็น boolean เหมือนกับการใช้ฟังก์ชัน `ToBoolean` โดยใช้กฎต่างๆดังต่อไปนี้:

    + **Objects** ได้ผลลัพธ์เป็น **true**
    + **Undefined** ได้ผลลัพธ์เป็น **false**
    + **Null** ได้ผลลัพธ์เป็น **false**
    + **Booleans** ได้ผลลัพธ์ **ขึ้นอยู่กับค่าของ boolean**
    + **Numbers** ได้ผลลัพธ์เป็น **false** ถ้า **+0, -0, or NaN**, นอกนั้นได้ **true**
    + **Strings** ได้ผลลัพธ์เป็น **false** ถ้า `''`, นอกนั้นได้ **true**

    ```javascript
    if ([0]) {
      // true
      // เพราะ array คือออบเจ็กต์
    }
    ```

  - [15.3](#15.3) <a name='15.3'></a> ใช้ Shortcuts จะสามารถทำให้ใช้ค่าของตัวแปรแทนค่า boolean ได้เลยโดยไม่ต้องเขียนการเช็คเงื่อนไขเพิ่ม 

    ```javascript
    // ไม่ดี
    if (name !== '') {
      // ...stuff...
    }

    // ดี
    if (name) {
      // ...stuff...
    }

    // ไม่ดี
    if (collection.length > 0) {
      // ...stuff...
    }

    // ดี
    if (collection.length) {
      // ...stuff...
    }
    ```

  - [15.4](#15.4) <a name='15.4'></a> อ่านเพิ่มเติมได้ที่ [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) โดย Angus Croll.

**[[⬆ กลับไปด้านบน]](#TOC)**


## Blocks

  - [16.1](#16.1) <a name='16.1'></a> ใช้วงเล็บปีกกา `{}` ในกรณีที่ประกาศบล็อกมากกว่าหนึ่งบรรทัด

    ```javascript
    // ไม่ดี
    if (test)
      return false;

    // ดี
    if (test) return false; // วางไว้บรรทัดเดียวกันจะอ่านง่ายกว่า

    // ดี
    if (test) {
      return false;
    }

    // ไม่ดี
    function() { return false; }

    // ดี
    function() { // ถ้ามีวงเล็บปีกกาให้วางไว้คนละบรรทัดจะอ่านง่ายกว่า
      return false;
    }
    ```

  - [16.2](#16.2) <a name='16.2'></a> ถ้าประกาศโดยมีทั้ง `if` และ `else` ให้ใส่  `else` ไว้บรรทัดเดียวกับวงเล็บปีกกาปิดของ `if`

    ```javascript
    // ไม่ดี
    if (test) {
      thing1();
      thing2();
    }
    else {
      thing3();
    }

    // ดี
    if (test) {
      thing1();
      thing2();
    } else {
      thing3();
    }
    ```


**[[⬆ กลับไปด้านบน]](#TOC)**

## Control Statements

  - [17.1](#17.1) ในกรณีที่มีการใช้ control statement เช่น `if`, `while` และอื่นๆ มีความยาวเกินความยาวต่อบรรทัดสูงสุด ให้รวมกรุ๊ปแต่ละเงื่อนไขและตัดขึ้นเป็นบรรทัดใหม่สำหรับแต่ละเงื่อนไข  
 

    ```javascript
    // ไม่ดี
    if ((foo === 123 || bar === 'abc') && doesItLookGoodWhenItBecomesThatLong() && isThisReallyHappening()) {
      thing1();
    }

    // ไม่ดี
    if (foo === 123 &&
      bar === 'abc') {
      thing1();
    }

    // ไม่ดี
    if (foo === 123
      && bar === 'abc') {
      thing1();
    }

    // ไม่ดี
    if (
      foo === 123 &&
      bar === 'abc'
    ) {
      thing1();
    }

    // ดี
    if (
      foo === 123
      && bar === 'abc'
    ) {
      thing1();
    }

    // ดี
    if (
      (foo === 123 || bar === "abc")
      && doesItLookGoodWhenItBecomesThatLong()
      && isThisReallyHappening()
    ) {
      thing1();
    }

    // ดี
    if (foo === 123 && bar === 'abc') {
      thing1();
    }
    ```

**[[⬆ กลับไปด้านบน]](#TOC)**


## Comments

  - [18.1](#18.1) <a name='18.1'></a>   
  1.ใช้ `/** ... */` สำหรับคอมเม้นต์ที่มากกว่าหนึ่งบรรทัด  
  2.ควรจะบอกประเภทและค่าของพารามิเตอร์พร้อมทั้งค่าที่จะรีเทิร์น

    ```javascript
    // ไม่ดี
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...stuff...

      return element;
    }

    // ดี
    /**
     * make() returns a new element
     * based on the passed in tag name
     *
     * @param {String} tag
     * @return {Element} element
     */
    function make(tag) {

      // ...stuff...

      return element;
    }
    ```

  - [18.2](#18.2) <a name='18.2'></a>   
  1.ใช้ `//` สำหรับคอมเม้นต์บรรทัดเดียว โดยใส่ไว้บรรทัดบนของสิ่งที่ต้องการคอมเม้นต์  
  2.เพิ่มบรรทัดว่างไว้ด้านบนคอมเม้นต์ด้วยเพื่อให้อ่านง่ายขึ้น

    ```javascript
    // ไม่ดี
    const active = true;  // is current tab

    // ดี
    // is current tab
    const active = true;

    // ไม่ดี
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      const type = this._type || 'no type';

      return type;
    }

    // ดี
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      const type = this._type || 'no type';

      return type;
    }
    ```

  - [18.3](#18.3) <a name='18.3'></a> ใส่ `FIXME` หรือ `TODO` ไว้ด้านหน้าคอมเม้นต์ ซึ่งจะช่วยให้ผู้พัฒนาระบบท่านอื่นๆทราบได้ว่าสิ่งเหล่านั้นอาจจะต้องแก้ไข หรือยังไม่ได้ทำ (IDE บางตัวสามารถค้นหาคอมเม้นต์เหล่านี้อัตโนมัติ และบอกถึงสิ่งที่ควรจะแก้ไขหรือทำเพิ่ม)

  - [18.4](#18.4) <a name='18.4'></a> ใช้ `// FIXME:` เพื่อบอกปัญหา

    ```javascript
    class Calculator {
      constructor() {
        // FIXME: shouldn't use a global here
        total = 0;
      }
    }
    ```

  - [18.5](#18.5) <a name='18.5'></a> ใช้ `// TODO:` เพื่อบอกแนวทางในการแก้ไขปัญหา (แต่ยังไม่ได้ทำ)

    ```javascript
    class Calculator {
      constructor() {
        // TODO: total should be configurable by an options param
        this.total = 0;
      }
    }
    ```

**[[⬆ กลับไปด้านบน]](#TOC)**


## Whitespace

  - [19.1](#19.1) <a name='19.1'></a> ควรตั้งค่าหนึ่งแท็บเท่ากับสองช่องว่าง (สามารถตั้งค่าใน Editor หรือ IDE ได้)

    ```javascript
    // ไม่ดี
    function() {
    ∙∙∙∙const name;
    }

    // ไม่ดี
    function() {
    ∙const name;
    }

    // ดี
    function() {
    ∙∙const name;
    }
    ```

  - [19.2](#19.2) <a name='19.2'></a> ใส่ช่องว่างก่อนวงเล็บปีกกาเปิด

    ```javascript
    // ไม่ดี
    function test(){
      console.log('test');
    }

    // ดี
    function test() {
      console.log('test');
    }

    // ไม่ดี
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });

    // ดี
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });
    ```

  - [19.3](#19.3) <a name='19.3'></a> ใส่ช่องว่างก่อนเปิดวงเล็บสำหรับ control statements (`if`, `else`, `while`, และอื่นๆ) แต่สำหรับพารามิเตอร์ไม่ต้องใส่ช่องว่าง

    ```javascript
    // ไม่ดี
    if(isJedi) {
      fight ();
    }

    // ดี
    if (isJedi) {
      fight();
    }

    // ไม่ดี
    function fight () {
      console.log ('Swooosh!');
    }

    // ดี
    function fight() {
      console.log('Swooosh!');
    }
    ```

  - [19.4](#19.4) <a name='19.4'></a> ใส่ช่องว่างเวลาประกาศตัวแปร

    ```javascript
    // ไม่ดี
    const x=y+5;

    // ดี
    const x = y + 5;
    ```

  - [19.5](#19.5) <a name='19.5'></a> ลงท้ายไฟล์ด้วยการขึ้นบรรทัดใหม่เสมอ (แค่หนึ่งบรรทัดเท่านั้น)

    ```javascript
    // ไม่ดี
    (function(global) {
      // ...stuff...
    })(this);
    ```

    ```javascript
    // ไม่ดี
    (function(global) {
      // ...stuff...
    })(this);↵
    ↵
    ```

    ```javascript
    // ดี
    (function(global) {
      // ...stuff...
    })(this);↵
    ```

  - [19.5](#19.5) <a name='19.5'></a> ใส่ย่อหน้าเวลาเรียกใช้เมท็อตแบบต่อเนื่อง (Method chaining) ให้วางจุด `.` ไว้ด้านหน้าเสมอ เพื่อบอกว่าเป็นการเรียกเมท็อต

    ```javascript
    // ไม่ดี
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // ไม่ดี
    $('#items').
      find('.selected').
        highlight().
        end().
      find('.open').
        updateCount();

    // ดี
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    // ไม่ดี
    const leds = stage.selectAll('.led').data(data).enter().append('svg:svg').class('led', true)
        .attr('width', (radius + margin) * 2).append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);

    // ดี
    const leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .classed('led', true)
        .attr('width', (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);
    ```

  - [19.6](#19.6) <a name='19.6'></a> ใส่บรรทัดว่างหลังจากจบบล็อก และก่อนที่จะขึ้น statement ใหม่

    ```javascript
    // ไม่ดี
    if (foo) {
      return bar;
    }
    return baz;

    // ดี
    if (foo) {
      return bar;
    }

    return baz;

    // ไม่ดี
    const obj = {
      foo() {
      },
      bar() {
      },
    };
    return obj;

    // ดี
    const obj = {
      foo() {
      },

      bar() {
      },
    };

    return obj;
    ```


**[[⬆ กลับไปด้านบน]](#TOC)**

## Commas

  - [20.1](#20.1) <a name='20.1'></a> อย่าวางจุลภาค `,` ไว้ด้านหน้า

    ```javascript
    // ไม่ดี
    const story = [
        once
      , upon
      , aTime
    ];

    // ดี
    const story = [
      once,
      upon,
      aTime,
    ];

    // ไม่ดี
    const hero = {
        firstName: 'Ada'
      , lastName: 'Lovelace'
      , birthYear: 1815
      , superPower: 'computers'
    };

    // ดี
    const hero = {
      firstName: 'Ada',
      lastName: 'Lovelace',
      birthYear: 1815,
      superPower: 'computers',
    };
    ```

  - [20.2](#20.2) <a name='20.2'></a> ควรใส่จุลภาค `,` ต่อท้ายพรอพเพอร์ตี้ตัวสุดท้าย

    > เพราะว่าเวลาดูใน `git diff` จะเป็นการเพิ่มบรรทัดอย่างเดียว โดยไม่มีการลบบรรทัดก่อนหน้า นอกจากนั้น `Transpilers` เช่น Babel จะลบตัวจุลภาคนี้ออกเองเวลาคอมไพล์ ทำให้ไม่ต้องกังวลเกี่ยวกับ [ปัญหาจุลภาคที่เกินมา](es5/README.md#commas) ในบราวเซอร์เวอร์ชันเก่า

    ```javascript
    // ไม่ดี - git diff เมื่อไม่มีจุลภาคต่อท้าย
    const hero = {
         firstName: 'Florence',
    -    lastName: 'Nightingale' // ลบตัวสุดท้ายออก
    +    lastName: 'Nightingale',
    +    inventorOf: ['coxcomb graph', 'modern nursing']
    }

    // ดี - git diff เมื่อมีจุลภาคต่อท้าย
    const hero = {
         firstName: 'Florence',
         lastName: 'Nightingale',
    +    inventorOf: ['coxcomb chart', 'modern nursing'], // เพิ่มบรรทัดอย่างเดียว
    }

    // ไม่ดี
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully'
    };

    const heroes = [
      'Batman',
      'Superman'
    ];

    // ดี
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully',
    };

    const heroes = [
      'Batman',
      'Superman',
    ];
    ```

**[[⬆ กลับไปด้านบน]](#TOC)**


## Semicolons

  - [21.1](#21.1) <a name='21.1'></a> ควรใส่ `;` เมื่อจบ statement

    ```javascript
    // ไม่ดี
    (function() {
      const name = 'Skywalker'
      return name
    })()

    // ดี
    (() => {
      const name = 'Skywalker';
      return name;
    })();

    // ดี (เป็นการป้องกันไม่ให้ฟังก์ชันถูกตีความเป็น argument เมื่อทำการต่อไฟล์สองไฟล์ที่ใช้ IIFEs)
    ;(() => {
      const name = 'Skywalker';
      return name;
    })();
    ```

    [อ่านเพิ่มเติม](http://stackoverflow.com/a/7365214/1712802).

**[[⬆ กลับไปด้านบน]](#TOC)**


## Type Casting & Coercion

  - [22.1](#22.1) <a name='22.1'></a> ทำการแปลงค่าไว้ด้านหน้าสุดเสมอ เพราะเวลาอ่านจะทราบได้ทันที่ว่าค่าที่จะได้ จะเป็นชนิดใด
  - [22.2](#22.2) <a name='22.2'></a> สตริง:

    ```javascript
    //  => this.reviewScore = 9;

    // ไม่ดี
    const totalScore = this.reviewScore + '';

    // ดี
    const totalScore = String(this.reviewScore);
    ```

  - [22.3](#22.3) <a name='22.3'></a> เวลาใช้ `parseInt` ในการแปลงค่าให้เป็นตัวเลข ควรจะใส่เลขฐานที่ต้องการแปลงด้วย เพราะถ้าไม่ใส่อาจจะมีข้อผิดพลาดได้ถ้าค่าที่แปลงเป็นสตริงที่ไม่ได้ประกอบไปด้วยตัวเลขทั้งหมด

    ```javascript
    const inputValue = '4';

    // ไม่ดี
    const val = new Number(inputValue);

    // ไม่ดี
    const val = +inputValue;

    // ไม่ดี
    const val = inputValue >> 0;

    // ไม่ดี
    const val = parseInt(inputValue);

    // ดี
    const val = Number(inputValue);

    // ดี
    const val = parseInt(inputValue, 10);
    ```

  - [22.4](#22.4) <a name='22.4'></a> ในบางกรณีที่ต้องการให้ได้ประสิทธิภาพสูงสุดด้วยการใช้ Bitshift แทนการแปลงค่าโดยใช้ `parseInt` สามารถอ่านเพิ่มเติมได้ที่ [performance reasons](http://jsperf.com/coercion-vs-casting/3) นอกจากนั้นควรใส่คอมเม้นต์ต่างๆอธิบายเหตุผลไว้ด้วย

    ```javascript
    // ดี
    /**
     * ถ้า parseInt ทำให้โค้ดช้า ให้ใช้
     * Bitshifting เพื่อแปลงค่าเป็นตัวเลขแทน
     * ซึ่งทำให้โค้ดสามารถทำงานได้เร็วขึ้นอย่างมาก
     */
    const val = inputValue >> 0;
    ```

  - [22.5](#22.5) <a name='22.5'></a> ควรระวังการใช้งาน bitshift เพราะตัวเลขปกติจะเป็น [64-bit values](http://es5.github.io/#x4.3.19), แต่ Bitshift จะคืนค่าเป็น 32-bit เสมอ ([ที่มา](http://es5.github.io/#x11.7)) Bitshift อาจทำให้ค่าผิดแปลกไปถ้าค่าของตัวเลขใหญ่กว่า 32 bits. [ดูการพูดคุยในเรื่องนี้](https://github.com/airbnb/javascript/issues/109) ตัวเลขที่มากที่สุดของ 32-bit Int คือ 2,147,483,647:

    ```javascript
    2147483647 >> 0 //=> 2147483647
    2147483648 >> 0 //=> -2147483648 เกินค่ามากที่สุดของ 32-bit Int จึงทำให้เกิดข้อผิดพลาด
    2147483649 >> 0 //=> -2147483647 เกินค่ามากที่สุดของ 32-bit Int จึงทำให้เกิดข้อผิดพลาด
    ```

  - [22.6](#22.6) <a name='22.6'></a> Booleans:

    ```javascript
    const age = 0;

    // ไม่ดี
    const hasAge = new Boolean(age);

    // ดี
    const hasAge = Boolean(age);

    // ดี
    const hasAge = !!age;
    ```

**[[⬆ กลับไปด้านบน]](#TOC)**


## Naming Conventions

  - [23.1](#23.1) <a name='23.1'></a> ควรจะตั้งชื่อให้สื่อความหมาย

    ```javascript
    // ไม่ดี
    function q() {
      // ...stuff...
    }

    // ดี
    function query() {
      // ..stuff..
    }
    ```

  - [23.2](#23.2) <a name='23.2'></a> ใช้ camelCase (ขึ้นต้นด้วยตัวเล็กและคำต่อไปขึ้นต้นด้วยตัวใหญ่) เมื่อต้องการตั้งชื่อออบเจ็กต์, ฟังก์ชัน, และ instance

    ```javascript
    // ไม่ดี
    const OBJEcttsssss = {};
    const this_is_my_object = {};
    function c() {}

    // ดี
    const thisIsMyObject = {};
    function thisIsMyFunction() {}
    ```

  - [23.3](#23.3) <a name='23.3'></a> ใช้ PascalCase (ขึ้นต้นทุกคำด้วยตัวใหญ่) เมื่อต้องการตั้งชื่อ constructor หรือ class

    ```javascript
    // ไม่ดี
    function user(options) {
      this.name = options.name;
    }

    const bad = new user({
      name: 'nope',
    });

    // ดี
    class User {
      constructor(options) {
        this.name = options.name;
      }
    }

    const good = new User({
      name: 'yup',
    });
    ```

  - [23.4](#23.4) <a name='23.4'></a> ขึ้นต้นด้วยขีดล่าง (`_`) เมื่อต้องการตั้งชื่อพรอพเพอร์ตี้ที่เป็น Private

    ```javascript
    // ไม่ดี
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';

    // ดี
    this._firstName = 'Panda';
    ```

  - [23.5](#23.5) <a name='23.5'></a> อย่าบันทึกค่า `this` ไว้ใช้ ให้ใช้ Arrow functions หรือ Function#bind.

    ```javascript
    // ไม่ดี
    function foo() {
      const self = this;
      return function() {
        console.log(self);
      };
    }

    // ไม่ดี
    function foo() {
      const that = this;
      return function() {
        console.log(that);
      };
    }

    // ดี
    function foo() {
      return () => {
        console.log(this);
      };
    }
    ```

  - [23.6](#23.6) <a name='23.6'></a> ถ้าในไฟล์มีแค่หนึ่งคลาส ให้ตั้งชื่อไฟล์ให้เป็นชื่อเดียวกับชื่อคลาส
    ```javascript
    // file contents
    class CheckBox {
      // ...
    }
    export default CheckBox;

    // in some other file
    // ไม่ดี
    import CheckBox from './checkBox';

    // ไม่ดี
    import CheckBox from './check_box';

    // ดี
    import CheckBox from './CheckBox';
    ```

  - [23.7](#23.7) <a name='23.7'></a> ใช้ camelCase เมื่อต้องการเอ็กพอร์ตฟังก์ชัน ชื่อไฟล์ควรเป็นชื่อเดียวกับชื่อฟังก์ชัน

    ```javascript
    function makeStyleGuide() {
    }

    export default makeStyleGuide;
    ```

  - [23.8](#23.8) <a name='23.8'></a> ใช้ PascalCase เมื่อเอ็กพอร์ต Singleton / Function library / หรือ Bare object

    ```javascript
    const AirbnbStyleGuide = {
      es6: {
      }
    };

    export default AirbnbStyleGuide;
    ```


**[[⬆ กลับไปด้านบน]](#TOC)**


## Accessors

  - [24.1](#24.1) <a name='24.1'></a> Accessor functions (ฟังก์ชันที่ใช้ในการเข้าถึงพรอพเพอร์ตี้) ไม่จำเป็นต้องมีก็ได้
  - [24.2](#24.2) <a name='24.2'></a> แต่ถ้ามีควรจะตั้งชื่อในรูปแบบ getVal() และ setVal('hello')

    ```javascript
    // ไม่ดี
    dragon.age();

    // ดี
    dragon.getAge();

    // ไม่ดี
    dragon.age(25);

    // ดี
    dragon.setAge(25);
    ```

  - [24.3](#24.3) <a name='24.3'></a> ถ้าพรอพเพอร์ตี้เป็นค่าบูลีน (boolean) ให้ใช้ isVal() หรือ hasVal().

    ```javascript
    // ไม่ดี
    if (!dragon.age()) {
      return false;
    }

    // ดี
    if (!dragon.hasAge()) {
      return false;
    }
    ```

  - [24.4](#24.4) <a name='24.4'></a> ความจริงแล้วตั้งชื่อ get() และ set() ก็ไม่เสียหายอะไร แต่ต้องตั้งให้เหมือนกันในทุกๆที่

    ```javascript
    class Jedi {
      constructor(options = {}) {
        const lightsaber = options.lightsaber || 'blue';
        this.set('lightsaber', lightsaber);
      }

      set(key, val) {
        this[key] = val;
      }

      get(key) {
        return this[key];
      }
    }
    ```

**[[⬆ กลับไปด้านบน]](#TOC)**


## Events

  - [25.1](#25.1) <a name='25.1'></a> เมื่อทำการเชื่อมต่ออีเว้นต์ ให้ส่งค่าที่เป็นออบเจ็กต์ไป ซึ่งจะดีกว่าการส่งค่าแบบธรรมดา เพราะจะช่วยให้ตัวเมท็อตที่รับค่าสามารถแก้ไขค่าและเพิ่มพรอพเพอร์ตี้ได้ง่ายขึ้น

    ```javascript
    // ไม่ดี
    $(this).trigger('listingUpdated', listing.id);

    ...

    $(this).on('listingUpdated', function(e, listingId) {
      // do something with listingId
    });
    ```

    ```javascript
    // ดี
    $(this).trigger('listingUpdated', { listingId : listing.id });

    ...

    $(this).on('listingUpdated', function(e, data) {
      // do something with data.listingId
    });
    ```

  **[[⬆ กลับไปด้านบน]](#TOC)**


## jQuery

  - [26.1](#26.1) <a name='26.1'></a> ใส่สัญลักษณ์ `$` ไว้ด้านหน้าตัวแปรทุกตัวที่เป็น jQuery Object

    ```javascript
    // ไม่ดี
    const sidebar = $('.sidebar');

    // ดี
    const $sidebar = $('.sidebar');
    ```

  - [26.2](#26.2) <a name='26.2'></a> ในกรณีที่ต้องค้นหา DOM โดยใช้ jQuery ควรจะเก็บแคช (Cache) ไว้เสมอ เพราะการค้นหา DOM ซ้ำๆหลายรอบจะส่งผลต่อประสิทธิภาพของโค้ด

    ```javascript
    // ไม่ดี
    function setSidebar() {
      $('.sidebar').hide();

      // ...stuff...

      $('.sidebar').css({
        'background-color': 'pink'
      });
    }

    // ดี
    function setSidebar() {
      const $sidebar = $('.sidebar'); // เก็บแคชในการค้นหาไว้ในตัวแปร เพื่อนำไปใช้ต่อไป
      $sidebar.hide();

      // ...stuff...

      $sidebar.css({
        'background-color': 'pink'
      });
    }
    ```

  - [26.3](#26.3) <a name='26.3'></a> เวลาค้นหา DOM ให้ใช้รูปแบบของ Cascading เช่น  `$('.sidebar ul')` หรือ parent > child `$('.sidebar > ul')` -  [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)
  - [26.4](#26.4) <a name='26.4'></a> ใช้ `find` ร่วมกับ jQuery object (ที่เราแคชไว้ก่อนหน้านี้)

    ```javascript
    // ไม่ดี
    $('ul', '.sidebar').hide();

    // ไม่ดี
    $('.sidebar').find('ul').hide();

    // ดี
    $('.sidebar ul').hide();

    // ดี
    $('.sidebar > ul').hide();

    // ดี
    $sidebar.find('ul').hide();
    ```

**[[⬆ กลับไปด้านบน]](#TOC)**


## ECMAScript 5 Compatibility

  - [27.1](#27.1) <a name='27.1'></a> อ่านเพิ่มเติมได้ที่ [Kangax](https://twitter.com/kangax/)'s ES5 [compatibility table](http://kangax.github.com/es5-compat-table/)

**[[⬆ กลับไปด้านบน]](#TOC)**

## ECMAScript 6+ (ES 2015+) Styles

  - [28.1](#28.1) <a name='28.1'></a> อ่านเพิ่มเติมเกี่ยวกับฟีเจอร์ต่างๆของ ES6:

1. [Arrow Functions](#arrow-functions)
1. [Classes](#constructors)
1. [Object Shorthand](#es6-object-shorthand)
1. [Object Concise](#es6-object-concise)
1. [Object Computed Properties](#es6-computed-properties)
1. [Template Strings](#es6-template-literals)
1. [Destructuring](#destructuring)
1. [Default Parameters](#es6-default-parameters)
1. [Rest](#es6-rest)
1. [Array Spreads](#es6-array-spreads)
1. [Let and Const](#references)
1. [Iterators and Generators](#iterators-and-generators)
1. [Modules](#modules)

**[[⬆ กลับไปด้านบน]](#TOC)**

## Standard Library

  ตัว [Standard Library](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects) มี Utilities หลายตัวที่อาจจะทำงานผิดพลาดหรือไม่ถูกต้องแต่ยังมีให้ใช้อยู่ด้วยเหตุผลทางด้าน Legacy ดังนั้นให้เลือกใช้ตัวที่เหมาะสม

  - [29.1](#29.1) <a name="29.1"></a> ใช้ `Number.isNaN` แทนที่จะใช้ `isNaN` ที่เป็นฟังก์ชัน Global

    > เพราะว่าฟังก์ชั่น `isNaN` ที่เป็น Global จะแปลงค่าที่ไม่ได้เป็น numbers ให้กลายเป็น numbers และ return ค่า true

    ```javascript
    // ไม่ดี
    isNaN('1.2'); // false
    isNaN('1.2.3'); // true

    // ดี
    Number.isNaN('1.2.3'); // false
    Number.isNaN(Number('1.2.3')); // true
    ```

  - [29.2](#29.2) <a name="29.2"></a> ใช้ `Number.isFinite` แทนที่จะใช้ `isFinite` ที่เป็นฟังก์ชั่น Global

    > เพราะว่าฟังก์ชั่น `isFinite` ที่เป็น Global จะแปลงค่าที่ไม่ได้เป็น numbers ให้กลายเป็น numbers และ return ค่า true

    ```javascript
    // ไม่ดี
    isFinite('2e3'); // true

    // ดี
    Number.isFinite('2e3'); // false
    Number.isFinite(parseInt('2e3', 10)); // true
    ```

**[[⬆ กลับไปด้านบน]](#TOC)**

## Testing

  - [30.1](#30.1) <a name='30.1'></a> **Yup.**

    ```javascript
    function() {
      return true;
    }
    ```

**[[⬆ กลับไปด้านบน]](#TOC)**


## Performance

**อ่านเพิ่มเติมจากข้อมูลต่อไปนี้**

  - [On Layout & Web Performance](http://kellegous.com/j/2013/01/26/layout-performance/)
  - [String vs Array Concat](http://jsperf.com/string-vs-array-concat/2)
  - [Try/Catch Cost In a Loop](http://jsperf.com/try-catch-in-loop-cost)
  - [Bang Function](http://jsperf.com/bang-function)
  - [jQuery Find vs Context, Selector](http://jsperf.com/jquery-find-vs-context-sel/13)
  - [innerHTML vs textContent for script text](http://jsperf.com/innerhtml-vs-textcontent-for-script-text)
  - [Long String Concatenation](http://jsperf.com/ya-string-concat)
  - Loading...

**[[⬆ กลับไปด้านบน]](#TOC)**


