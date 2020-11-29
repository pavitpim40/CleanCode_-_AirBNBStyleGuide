# Airbnb CSS / Sass Styleguide

*A mostly reasonable approach to CSS and Sass*

## Table of Contents

1. [Terminology](#terminology)
    - [Rule Declaration](#rule-declaration)
    - [Selectors](#selectors)
    - [Properties](#properties)
1. [CSS](#css)
    - [Formatting](#formatting)
    - [Comments](#comments)
    - [OOCSS and BEM](#oocss-and-bem)
    - [ID Selectors](#id-selectors)
    - [JavaScript hooks](#javascript-hooks)
    - [Border](#border)
1. [Sass](#sass)
    - [Syntax](#syntax)
    - [Ordering](#ordering-of-property-declarations)
    - [Variables](#variables)
    - [Mixins](#mixins)
    - [Extend directive](#extend-directive)
    - [Nested selectors](#nested-selectors)
1. [Translation](#translation)
1. [License](#license)

## Terminology - การใช้คำศัพท์ต่างๆ

### Rule declaration - การประกาศกฏใน CSS 

วิธีการคือการประกาศ Selector หรือ กลุ่มของ Selector แล้วตามด้วยกลุ่มของ Property ที่ต้องการ ตัวอย่างเช่น

```css
.listing {
  font-size: 18px;
  line-height: 1.2;
}
```

### Selectors - การใช้ Selector 
ใช้เป็นตัวจับคู่กฏที่เราประกาศใน CSS กับ element ของ HTML 


```css
.my-element-class {
  /* ... */
}

[aria-hidden] {
  /* ... */
}
```

### Properties
คือค่าต่างๆที่เราต้องการตกแต่ง element ทื่เราเลือกโดย Selector โดยเก็บในลักษณะของคู่ key-value


```css
/* some selector */ {
  background: #f1f1f1;
  color: #333;
}
```

**[⬆ back to top](#table-of-contents)**

## CSS

### Formatting

* ใช้การย่อหน้าแบบ Soft tabs (2 spaces)
* การตั้งชื่อคลาสให้ใช้ dash(-) จะดีกว่าการตั้งชือแบบ camelCase
  - หรือจะใช้ Underscores คู่กับการตั้งชื่อแบบ PascalCase ก็ได้หากคุณใช้ BEM (see [OOCSS and BEM](#oocss-and-bem) below).
* ไม่ใช้ ID selector
* เมื่อต้องการใช้งาน selector หลายๆตัวในใช้ rule declaration 1 ครั้ง  ทำให้ selector แต่ละตัวมีบรรทัดของตัวเอง (บรรทัดใครบรรทัดมัน จะได้อ่านง่าย)
* เว้น 1 ช่องว่างก่อนถึงปีกกกาเปิด `{` ของ rule declarations
* In properties, put a space after, but not before, the `:` character.

* ขึ้นบนบรรทัดใหม่ก่อน ค่อยตามด้วยปีกกาปิด `}` เมื่อจบ rule declarations
* เว้นบรรทัดว่างระหว่าง rule declarations

**Bad**

```css
.avatar{
    border-radius:50%;
    border:2px solid white; }
.no, .nope, .not_good {
    // ...
}
#lol-no {
  // ...
}
```

**Good**

```css
.avatar {
  border-radius: 50%;
  border: 2px solid white;
}

.one,
.selector,
.per-line {
  // ...
}
```

### Comments

* แนะนำให้ใช้ line comment  (`//` in Sass-land)  ในการบลอค comment
* แนะนำให้ comment แยกเป็นบรรทัดของตัวเอง หลีกเลี่ยงการคอมเม้นท้ายบรรทัด
* เขียนรายละเอียดลงในคอมเม้นเฉพาะในส่วนที่โค้ดเองอธิบายตัวเองไม่ได้

### OOCSS and BEM

เราสนับสนุนให้ใช้ทั้ง OOCSS และ BEM ร่วมกัน(บ้าง)เพราะว่า :

  * ช่วยให้เราสร้าง relationship ระหว่าง HTML กับ CSS ได้ชัดเจนและรัดกุม
  * ช่วยให้เราสามารถสร้าง,ประกอบ,ใช้ซ้ำ Component ต่างๆได้
  * ช่วยลดการซ้อนกัน (nest) และมีค่า specificity ที่ต่ำ
  * ช่วยให้การสเกล Stylesheet ง่ายขึ้น

**OOCSS**  หรือ “Object Oriented CSS”, คือวิธีการเขียน CSS ที่พยายามทำให้เรามองตัว Stylesheet ของเราคล้ายๆกับ กลุ่มก้อนของ "Object"    ทำให้สามารถเรียกใช้งานซ้ำได้อย่างอิสระทั่วทั้งเว็บไซต์ 

  * Nicole Sullivan's [OOCSS wiki](https://github.com/stubbornella/oocss/wiki)
  * Smashing Magazine's [Introduction to OOCSS](http://www.smashingmagazine.com/2011/12/12/an-introduction-to-object-oriented-css-oocss/)

**BEM** หรือ “Block-Element-Modifier” คือวิธีที่นิยมใช้ในการตั้งชื่อคลาสใน HTML และ CSS  
วิธีการนี้ถูกพัฒนาโดย Yandex โดยมีฐานการคิดมากจากความต้องการทำงานกับ Codebase ที่ใหญ่และอยากให้สเกลง่าย รวมไปถึงการรักษาการทำงานร่วมกับแนวทางของ OOCSS 

  * CSS Trick's [BEM 101](https://css-tricks.com/bem-101/)
  * Harry Roberts' [introduction to BEM](http://csswizardry.com/2013/01/mindbemding-getting-your-head-round-bem-syntax/)

เราแนะนำให้ใช้ BEM กับ PascalCase "blocks" ซึ่งทำให้สามารถทำงานเฉพาะตำแหน่งได้ดี เช่นการใช้กับ component ใดๆโดยเฉพาะ (เช่น React)  
- ใช้ Underscore สำหรับ Child Element  
- ใช้ dash สำหรับ Modifier 

**Example**

```jsx
// ListingCard.jsx
function ListingCard() {
  return (
    <article class="ListingCard ListingCard--featured">

      <h1 class="ListingCard__title">Adorable 2BR in the sunny Mission</h1>

      <div class="ListingCard__content">
        <p>Vestibulum id ligula porta felis euismod semper.</p>
      </div>

    </article>
  );
}
```

```css
/* ListingCard.css */
.ListingCard { }
.ListingCard--featured { }
.ListingCard__title { }
.ListingCard__content { }
```

  * `.ListingCard` คือ “block” มองเป็น higher-level component
  * `.ListingCard__title` คือ “element” มองเป็นตัว Child ของ `.ListingCard` 
  * `.ListingCard--featured` คือ “modifier”  มองเป็นตัวบอก state ที่หลากหลาย หรือ  `.ListingCard` block ที่หลากหลาย.

### ID selectors

While it is possible to select elements by ID in CSS, it should generally be considered an anti-pattern. ID selectors introduce an unnecessarily high level of [specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) to your rule declarations, and they are not reusable.

For more on this subject, read [CSS Wizardry's article](http://csswizardry.com/2014/07/hacks-for-dealing-with-specificity/) on dealing with specificity.

สรุป : ID มองเป็น Anti-pattern เพราะทำให้ค่า specificity สูง และ reuse ไม่ได้

### JavaScript hooks

Avoid binding to the same class in both your CSS and JavaScript. Conflating the two often leads to, at a minimum, time wasted during refactoring when a developer must cross-reference each class they are changing, and at its worst, developers being afraid to make changes for fear of breaking functionality.

We recommend creating JavaScript-specific classes to bind to, prefixed with `.js-`:

สรุป
- เลี่ยงการใช้ class เดียวกันใน CSS และ JavaScript เพื่อไม่ให้เสียเวลา Refractor  
- Dev บางคนอาจจะไม่กล้าปรับปรุงโค้ดเพราะเกรงว่าจะทำให้การทำงานของฟังก์ชันไม่เหมือนเดิม
- สร้างคลาสที่จะใช้กับ JavaScript โดยมีการขึ้นต้นด้วย `.js-`

```html
<button class="btn btn-primary js-request-to-book">Request to Book</button>
```

### Border

ใช้ `0` แทน `none` ในการระบุุว่าไม่ต้องการให้มี Border

**Bad**

```css
.foo {
  border: none;
}
```

**Good**

```css
.foo {
  border: 0;
}
```
**[⬆ back to top](#table-of-contents)**

## Sass

### Syntax

* ใช้ `.scss` syntax, ไม่ใช้ `.sass` syntax
* เขียน CSS ปกติ และตามด้วย `@include` (see below)

### Ordering of property declarations

1. Property declarations

    ประกาศ standard property ก่อน ตัวที่ไม่ได้่อยู่ใน `@include` หรือ nested selector 
    

    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      // ...
    }
    ```

2. ประกาศการใช้งาน `@include` 

   จัดกลุ่มของ `@include` ไว้ที่่ส่วนท้ายทำให้อ่านง่ายขึ้น.

    ```scss
    .btn-green {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);
      // ...
    }
    ```

3. Nested selectors

    - Nested selector(ถ้าจำเป็นต้องมี) ให้เอาไว้ส่วนท้ายที่สุด และไม่มีอะไรต่อท้ายแล้ว
    - ใช้การขึ้นบรรทัดสำหรับ rule declaration กับ nested selector 
   

    ```scss
    .btn {
      background: green;
      font-weight: bold;
      @include transition(background 0.5s ease);

      .icon {
        margin-right: 10px;
      }
    }
    ```

### Variables

- แนะนำให้ใช้การตั้งชื่อตัวแปรแบบ dash-cased (เช่น `$my-variable`) มากกว่า camelCased หรือ snake_cased
- แต่ถ้าต้องการใช้เป็น Prefix(คำขึ้นต้น) ของตัวแปร ที่ตั้งใจจะใช้ทำงานแค่ในไฟล์เดียวกันเท่านั้นให้ใช้ underscore (เช่น `$_my-variable`).

### Mixins

- Mixins ควรจะทำให้โค้ดของคุณ DRY เพิ่มความชัดเจน ซ่อนความซับซ้อนโดยการทำให้ abstract เช่นเดียวกันกับการตั้งชื่อฟังก์ชันที่ดี  
- Mixins จะหรืิอไม่รับ Argument ก็สามารถใช้งานได้
- พึงระลึกว่าหากคุณไม่ได้มี payload แล้วนำมาใช้งาน อาจจะทำให้คุณทำ duplicate code โดยไม่รู้ตัว


### Extend directive

- ควรหลีกเลี่ยงการใช้งาน เพราะมันดูเข้าใจยาก และมีความอันตรายพอสมควรในการใช้งาน โดยเฉพาะเมื่อใช้งานร่วมกับ nested selector 
- แม้แต่การใช้งานที่ top-level placeholder selector ก็อาจจะเกิดปัญหาได้หากมีการสลับลำดับตัว selector ในภายหลัง


- 
Gzipping ควรจัดการแทนการใช้ `@extend` ได้เกือบทั้งหมดshould และคุณสามารถทำให้โค้ดของคุณ DRY ขึ้นด้วย mixins.

### Nested selectors

**Do not nest selectors more than three levels deep!**
**ไม่ใช้การซ้อนกันเกินกว่า 3 ระดับ**
```scss
.page-container {
  .content {
    .profile {
      // STOP!
    }
  }
}
```

เมื่อ selector ของเราเริ่มยาวเป็นไปได้ว่าคุณพยายามเขียน CSS ออกมาแบบนี้

* พยายามจับคู่กับ HTML มากเกินไป (fragile) *—OR—*
* เฉพาะเจาะจงมากเกินไป (powerful) *—OR—*
* ไม่สามารถใช้ซ้ำได้


ย้ำอีกครั้ง **ไม่ใช้การซ้อนกันขออง ID Selector**

- ถ้าคุณต้องใช้ ID Selector ครั้งแรก (ซึ่งคุณควรพยายามที่จะไม่ใช้) มันควรจะต้องไม่ถูก Nest 
- ถ้าคุณพบว่าตัวเองกำลังทำแบบนั้น คุณควรย้อนกลับไปดู HTML ของคุณ หรือตอบคำถามตัวเองให้ได้ว่าทำไมคุณต้องต้องการค่า Specificity ที่สูงขนาดนั้น
- ถ้าคุณเขียน HTML และ CSS ได้ดี คุณควรที่จะ**ไม่มีความจำเป็น**ต้องใช้ ID selector


**[⬆ back to top](#table-of-contents)**

## Translation

  This style guide is also available in other languages:

  - ![id](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Indonesia.png) **Bahasa Indonesia**: [mazipan/css-style-guide](https://github.com/mazipan/css-style-guide)
  - ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Chinese (Traditional)**: [ArvinH/css-style-guide](https://github.com/ArvinH/css-style-guide)
  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chinese (Simplified)**: [Zhangjd/css-style-guide](https://github.com/Zhangjd/css-style-guide)
  - ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **French**: [mat-u/css-style-guide](https://github.com/mat-u/css-style-guide)
  - ![ja](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [nao215/css-style-guide](https://github.com/nao215/css-style-guide)
  - ![ko](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [CodeMakeBros/css-style-guide](https://github.com/CodeMakeBros/css-style-guide)
  - ![PT-BR](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Portuguese (Brazil)**: [felipevolpatto/css-style-guide](https://github.com/felipevolpatto/css-style-guide)
  - ![pt-PT](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Portugal.png) **Portuguese (Portugal)**: [SandroMiguel/airbnb-css-style-guide](https://github.com/SandroMiguel/airbnb-css-style-guide)
  - ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**: [rtplv/airbnb-css-ru](https://github.com/rtplv/airbnb-css-ru)
  - ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [ismamz/guia-de-estilo-css](https://github.com/ismamz/guia-de-estilo-css)
  - ![vn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **Vietnamese**: [trungk18/css-style-guide](https://github.com/trungk18/css-style-guide)
  - ![vn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **Italian**: [antoniofull/linee-guida-css](https://github.com/antoniofull/linee-guida-css)
  - ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **German**: [tderflinger/css-styleguide](https://github.com/tderflinger/css-styleguide)

**[⬆ back to top](#table-of-contents)**

## License

(The MIT License)

Copyright (c) 2015 Airbnb

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the 'Software'), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[⬆ back to top](#table-of-contents)**
