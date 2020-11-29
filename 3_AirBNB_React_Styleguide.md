# Airbnb React/JSX Style Guide


## <a name='TOC'>สารบัญ</a>

  1. [Basic Rules](#basic-rules)
  1. [Class vs `React.createClass` vs stateless](#class-vs-reactcreateclass-vs-stateless)
  1. [Naming](#naming)
  1. [Declaration](#declaration)
  1. [Alignment](#alignment)
  1. [Quotes](#quotes)
  1. [Spacing](#spacing)
  1. [Props](#props)
  1. [Parentheses](#parentheses)
  1. [Tags](#tags)
  1. [Methods](#methods)
  1. [Ordering](#ordering)
  1. [`isMounted`](#ismounted)

## Basic Rules

  - หนึ่งไฟล์ให้มีแค่หนึ่งคอมโพเนนท์เท่านั้น 
  - ยกเว้นคอมโพเนนท์ประเภท [Stateless หรือ Pure Components](https://facebook.github.io/react/docs/reusable-components.html#stateless-functions) ให้เขียนรวมในไฟล์เดียวกันได้
  - ใช้ JSX Syntax ร่วมกับ React เสมอ
  - ไม่ใช้ `React.createElement` ยกเว้นในกรณีที่ต้องจัดการกับที่มีไฟล์ที่ไม่ได้ใช้ JSX 

## Class vs `React.createClass` vs stateless

  - ถ้าภายในคอมโพเนนท์มีการจัดการ State หรือ refs ให้ใช้ `class extends React.Component` แทนการใช้ `React.createClass`  
  - ยกเว้นเราต้องการใช้งาน Mixins (Mixins ใช้งานได้กับ React.createClass เท่านั้น ไม่สามารถใช้ร่วมกับคลาสของ ES6 ได้) อ่านเพิ่มเติมจากกฎของ Eslint: [`react/prefer-es6-class`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-es6-class.md) [`react/prefer-stateless-function`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/prefer-stateless-function.md)

    ```javascript
    // bad
    const Listing = React.createClass({
      // ...
      render() {
        return <div>{this.state.hello}</div>;
      }
    });

    // good
    class Listing extends React.Component {
      // ...
      render() {
        return <div>{this.state.hello}</div>;
      }
    }
    ```

    แต่ถ้าภายในคอมโพเน้นท์ไม่มีการจัดการ State หรือ refs เราควรจะใช้ Function declaration แทนการสร้างคลาส

    ```javascript

    // bad
    class Listing extends React.Component {
      render() {
        return <div>{this.props.hello}</div>;
      }
    }

    // bad (เพราะ Arrow function ไม่สามารถกำหนดชื่อฟังก์ชันได้)
    const Listing = ({ hello }) => (
      <div>{hello}</div>
    );

    // good
    function Listing({ hello }) {
      return <div>{hello}</div>;
    }
    ```

## Naming

  - **นามสกุลไฟล์**: ใช้ `.jsx` เสมอ สำหรับคอมโพเน้นท์ของ React.
  - **ชื่อไฟล์**: ตั้งชื่อโดยใช้ PascalCase (ขึ้นต้นทุกคำด้วยตัวใหญ่) ตัวอย่างเช่น `ReservationCard.jsx`
  - **ชื่อตัวแปร**: ตั้งชื่อโดยใช้ PascalCase สำหรับคอมโพเน้นท์ของ React และ camelCase  สำหรับ Instance  

    ```javascript
    // bad
    import reservationCard from './ReservationCard'; 

    // good
    import ReservationCard from './ReservationCard';

    // bad
    const ReservationItem = <ReservationCard />; 

    // good
    const reservationItem = <ReservationCard />;
    ```

  - **ชื่อคอมโพเน้นท์**: ควรตั้งเหมือนชื่อไฟล์ เช่น ไฟล์ชื่อ `ReservationCard.jsx` ควรตั้งชื่อคอมโพเน้นท์เป็น `ReservationCard`   
  - อย่างไรก็ตาม คอมโพเน้นท์ที่เป็นคอมโพเน้นท์หลักของแต่ละโฟลเดอร์ให้ตั้งชื่อไฟล์ว่า `index.jsx` และนำชื่อคอมโพเน้นท์หลักไปตั้งเป็นชื่อของโฟเดอร์แทน (ง่ายต่อการอิมพอร์ต เพราะสามารถอิมพอร์ตจากชื่อโฟเดอร์ได้เลย)

    ```javascript
    // bad
    import Footer from './Footer/Footer';

    // bad
    import Footer from './Footer/index';

    // good
    import Footer from './Footer'; // ระบบจะอิมพอร์ตไฟล์ ./Footer/index.js ให้อัตโนมัติ
    ```

## Declaration

  - ไม่ใช้พรอพเพอร์ตี้ `displayName` สำหรับการตั้งชื่อคอมโพเน้นท์  
  - แต่ให้ตั้งชื่อคอมโพเน้นท์ตอนที่ทำการเอ็กพอร์ตแทน

    ```javascript
    // bad
    export default React.createClass({
      displayName: 'ReservationCard',
      // ....
    });

    // good 
    export default class ReservationCard extends React.Component {
    }
    ```

## Alignment

  - การจัดรูปแบบโค้ดของ JSX นั้นให้ทำตามกฎของ Eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

    ```javascript
    // bad
    <Foo superLongParam="bar"
         anotherSuperLongParam="baz" />

    // good
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    />

    // ถ้าพรอพเพอร์ตี้ไม่ยาวมาก สามารถใส่ไว้ในบรรทัดเดียวกันได้
    <Foo bar="bar" />

    // อิลิเม้นท์ภายในคอมโพเน้นท์ให้จัดย่อหน้าตามปกติ
    <Foo
      superLongParam="bar"
      anotherSuperLongParam="baz"
    >
      <Quux /> // อิลิเม้นท์ย่อหน้าเข้ามาตามปกติ
    </Foo>
    ```

## Quotes

  - ใช้ Double quotes `""` สำหรับการเขียน JSX เสมอ  
  - สำหรับโค้ดจาวาสคริปต์ทั่วไปให้ใช้ Single quotes `''`  
  - อ่านเพิ่มเติมจากกฎของ Eslint: [`jsx-quotes`](http://eslint.org/docs/rules/jsx-quotes)

  > - เพราะว่า ในกรณีที่มีอักขระพิเศษภายใน JSX [จะไม่สามารถใส่ Escaped quotes ได้](http://eslint.org/docs/rules/jsx-quotes) (ปกติในภาษาจาวาสคริปต์จะสามารถใส่สัญลักษณ์ \\ เพื่อทำการ Escape อักขระพิเศษนั้น ๆ แต่ใน JSX ไม่สามารถใช้ได้)  
  > - และในภาษาอังกฤษมีคำที่มีอักขระพิเศษเขี้ยวเดี่ยวอยู่เยอะพอสมควร ตัวอย่างเช่น `"don't"`
  > - ปกติแล้ว Attribute ของ HTML จะใช้ Single quotes เสมอดังนั้น JSX ควรจะทำตามกฎนั้นเช่นกัน

    ```javascript
    // bad
    <Foo bar='bar' />

    // good
    <Foo bar="bar" />

    // bad
    <Foo style={{ left: "20px" }} />

    // good
    <Foo style={{ left: '20px' }} />
    ```

## Spacing

  - ควรเว้นวรรคหนึ่งทีก่อนทำการปิดแท็ก (Tag)

    ```javascript
    // bad
    <Foo/>

    // bad
    <Foo                 />

    // bad
    <Foo
     />

    // good
    <Foo />
    ```

## Props

  - การตั้งชื่อพรอพเพอร์ตี้ให้ใช้ camelCase 

    ```javascript
    // bad
    <Foo
      UserName="hello"
      phone_number={12345678}
    />

    // good
    <Foo
      userName="hello"
      phoneNumber={12345678}
    />
    ```

  - พรอพเพอร์ตี้ที่มีค่าเป็น `true` ควรใส่แค่ชื่อพรอพเพอร์ตี้อย่างเดียวโดยไม่ต้องระบุค่า - เขียนเป็น Shorthand (React จะใส่ค่า `true` ให้อัตโนมัติ)   
  - อ่านเพิ่มเติมจากกฎของ Eslint: [`react/jsx-boolean-value`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-boolean-value.md)

    ```javascript
    // bad
    <Foo
      hidden={true}
    />

    // good
    <Foo
      hidden
    />
    ```

  ## Parentheses

  - ควรใส่วงเล็บครอบตัว JSX ไว้ ในกรณีที่โค้ดมีการ return มากกว่าหนึ่งบรรทัด  
  - อ่านเพิ่มเติมจากกฎของ Eslint: [`react/wrap-multilines`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/wrap-multilines.md)

    ```javascript
    // bad
    render() {
      return <MyComponent className="long body" foo="bar">
               <MyChild />
             </MyComponent>;
    }

    // good
    render() {
      return (
        <MyComponent className="long body" foo="bar">
          <MyChild />
        </MyComponent>
      );
    }

    // good
    render() {
      const body = <div>hello</div>; // มีแค่บรรทัดเดียว ไม่ใส่วงเล็บจะทำให้อ่านง่ายขึ้น
      return <MyComponent>{body}</MyComponent>;
    }
    ```

## Tags

  - ควรปิดแท็ก (Tag) ในตัวเองโดยใช้ `/>` ในกรณีที่ภายในแท็กนั้นไม่ประกอบไปด้วย Content อื่นๆ เช่น แท็กอื่นๆหรือข้อความต่างๆ เป็นต้น  
  - อ่านเพิ่มเติมจากกฎของ Eslint: [`react/self-closing-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/self-closing-comp.md)

    ```javascript
    // bad
    <Foo className="stuff"></Foo> // ไม่มีแท็กอื่นภายใน Foo เพราะฉะนั้นควรจะปิดแท็กในตัวมันเอง

    // good
    <Foo className="stuff" />
    ```

  - ถ้าคอมโพเน้นท์มีพรอพเพอร์ตี้หลายบรรทัด  
  - ควรจะปิดแท็กในบรรทัดใหม่เสมอ อ่านเพิ่มเติมจากกฎของ Eslint: [`react/jsx-closing-bracket-location`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-closing-bracket-location.md)

    ```javascript
    // bad
    <Foo
      bar="bar"
      baz="baz" />

    // good
    <Foo
      bar="bar"
      baz="baz"
    />
    ```

## Methods

  - ใช้ Arrow function เพื่อครอบตัวแปรภายใน (Local variable)

    ```javascript
    function ItemList(props) {
      return (
        <ul>
          {props.items.map((item, index) => (
            <Item
              key={item.key}
              onClick={() => doSomethingWith(item.name, index)}
            />
          ))}
        </ul>
      );
    }
    ```

  - เมื่อต้องการใช้ฟังก์ชัน `bind()` ในการผูกอีเว้นท์ ให้นำมาผูกในฟังก์ชันในคอนสตรัคเตอร์  
  - อ่านเพิ่มเติมจากกฎของ Eslint: [`react/jsx-no-bind`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/jsx-no-bind.md)

  > ทำไม? การเรียกฟังก์ชัน bind() ภายในเมท็อต `render()`จะสร้างฟังชันท์ใหม่ทุกครั้งเมื่อมีการเรียกใช้เมท็อต ซึ่งส่งผลกระทบต่อประสิทธิภาพของแอพพลิเคชั่น

    ```javascript
    // bad
    class extends React.Component {
      onClickDiv() {
        // ...
      }

      render() {
        return <div onClick={this.onClickDiv.bind(this)} />
      }
    }

    // good
    class extends React.Component {
      constructor(props) {
        super(props);

        this.onClickDiv = this.onClickDiv.bind(this);
      }

      onClickDiv() {
        // ...
      }

      render() {
        return <div onClick={this.onClickDiv} />
      }
    }
    ```

  - ไม่ใส่ undeserScore นำหน้าชื่อเมท็อตในคอมโพเน้นท์ของ React

    ```javascript
    // bad
    React.createClass({
      _onClickSubmit() {
        // ...
      },

      // ...
    });

    // good
    class extends React.Component {
      onClickSubmit() {
        // ...
      }

      // ...
    }
    ```

## Ordering

  - เรียงลำดับเมท็อตภายใน `class extends React.Component` ตามลำดับต่อไปนี้:

  1. optional `static` methods
  1. `constructor`
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. *clickHandlers or eventHandlers* like `onClickSubmit()` or `onChangeDescription()`
  1. *getter methods for `render`* like `getSelectReason()` or `getFooterContent()`
  1. *Optional render methods* like `renderNavigation()` or `renderProfilePicture()`
  1. `render`

  - วิธีการประกาศ `propTypes`, `defaultProps`, `contextTypes` และอื่น ๆ

    ```javascript
    import React, { PropTypes } from 'react';

    const propTypes = {
      id: PropTypes.number.isRequired,
      url: PropTypes.string.isRequired,
      text: PropTypes.string,
    };

    const defaultProps = {
      text: 'Hello World',
    };

    class Link extends React.Component {
      static methodsAreOk() {
        return true;
      }

      render() {
        return <a href={this.props.url} data-id={this.props.id}>{this.props.text}</a>
      }
    }

    Link.propTypes = propTypes;
    Link.defaultProps = defaultProps;

    export default Link;
    ```

  - หากสร้างคอมโพนเน้นท์ด้วย `React.createClass` ให้เรียงลำดับพรอพเพอร์ตี้ตามลำดับต่อไปนี้  
  - อ่านเพิ่มเติมจากกฎของ Eslint: [`react/sort-comp`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/sort-comp.md)

  1. `displayName`
  1. `propTypes`
  1. `contextTypes`
  1. `childContextTypes`
  1. `mixins`
  1. `statics`
  1. `defaultProps`
  1. `getDefaultProps`
  1. `getInitialState`
  1. `getChildContext`
  1. `componentWillMount`
  1. `componentDidMount`
  1. `componentWillReceiveProps`
  1. `shouldComponentUpdate`
  1. `componentWillUpdate`
  1. `componentDidUpdate`
  1. `componentWillUnmount`
  1. *clickHandlers or eventHandlers* like `onClickSubmit()` or `onChangeDescription()`
  1. *getter methods for `render`* like `getSelectReason()` or `getFooterContent()`
  1. *Optional render methods* like `renderNavigation()` or `renderProfilePicture()`
  1. `render`

## `isMounted`

  - อย่าใช้ฟังก์ชัน `isMounted` อ่านเพิ่มเติมจากกฎของ Eslint: [`react/no-is-mounted`](https://github.com/yannickcr/eslint-plugin-react/blob/master/docs/rules/no-is-mounted.md)

  > ทำไม? เพราะว่า [`isMounted` เป็นแพทเทิร์นที่ควรหลีกเลี่ยง][anti-pattern] ซึ่งมันไม่สามารถใช้ได้ในคลาสของ ES6 นอกจากนั้นฟังก์ชันนี้จะถูกลบในอนาคต

  [anti-pattern]: https://facebook.github.io/react/blog/2015/12/16/ismounted-antipattern.html

**[[⬆ กลับไปด้านบน]](#TOC)**
