# fraction-calculator
This is a library to calculate fraction. This library : 

1. can calculate(plus, minus, times, div, mod...) a fraction
2. can power or sqrt a fraction.
3. can convert a recurring decimal to a fraction or convert it back
4. supports IE
5. has 100% UT coverage

## Usage

### Use it in ES6 or Node.js

Install:

```bash
npm install fraction-calculator --save
```

Then:

```javascript
// ES6 import
import fc from 'fraction-calculator';

// Node.js
const fc = require('fraction-calculator');

// Use
fc(0.5).plus('12/25').minus('1/3').times('1 7/9').div('2 1/3').pow(6).sqrt().toString();      // 467288576/3906984375
```

### Use it in modern browsers

This library has used some ES6 features, if you only target modern browsers, you can use `./dist/fraction-calculator.min.js`, this file is only 7kb. This script will set a varable `fc` in the window, you can call it directly.

```html
<html>
  <script src="./dist/fraction-calculator.min.js"></script>
  <script>
   var res = fc(0.5).plus('12/25').minus('1/3').times('1 7/9').div('2 1/3').pow(6).sqrt().toString();
   console.log(res);   // 467288576/3906984375
  </script>
</html>
```

### Use it in old browsers

If you also target some old browsers, like IE, you can use `./dist/fraction-calculator.polyfill.min.js`, it is built with `babel-polyfill`。

## API

### All API

When you use this library, you shouldn't use `new`, just `fc()`. Let's take a look at all API:

```javascript
// constractor
fc('1/2')
fc('1 1/2')
fc(1, 2)
fc(4.5)
fc("1.'1'")
fc("1.(1)")
fc(fc(4.5))

// calculation api
fc().plus()
fc().minus()
fc().times()
fc().div()
fc().mod()
fc().pow()
fc().sqrt()
fc().abs()
fc().neg()
fc().inverse()
fc().ceil()
fc().floor()
fc().round()

// compare api
fc().equals()
fc().greaterThan()
fc().lessThan()

// display api
fc().toString()
fc().toFixed()
fc().toNumber()
fc().toRecurringDecimal()

// others
fc().clone()

// static api
fc.gcd()
fc.lcm()

// config
fc.DISABLE_REDUCE
```

### constractor

Fraction calculator constractor supports many kinds of parameter, but if you pass an invalid parameter, you will get an error:

```javascript
fc({});    // Error: Unsupported parameter [object Object]
```

#### String

```javascript
fc('1/2');     // It is 1/2
```

If you pass 0 or `NaN` as `denominator`, you will get an error:

```javascript
fc('1/0');  // Error: Denominator can't be 0 or NaN
```

#### Sting with whole part

```javascript
fc('1 1/2');     // It is 3/2
```

#### Number

```javascript
fc(1.5);     // It is still 3/2
```

If you pass a `NaN` or `Infinity`, you will get an error:

 ```javascript
fc('aaa');   // Error: Unsupported number NaN or Infinity
 ```

#### Two numbers

```javascript
fc(3, 2);   // It is still 3/2
```

#### Recurring decimal

You can use `''`or`()`to mark recurring part.

```javascript
fc("0.1'258'");   // 419/3330
fc("0.1(258)");   // 419/3330
```

#### Another instance

```javascript
fc(fc("0.1'258'"));   // 419/3330
```

### Calculation API

All calculation api are chainable, all return value is `this`. It means you can call it like this:

```javascript
fc(0.5).plus('12/25').minus('1/3').times('1 7/9').div('2.1(3)').pow(6).sqrt().ceil().floor().round().toString();  // 1
```

#### plus, minus, times, div,mod

You can know what they are doing from their names. They can support all kinds of parameter that constractor supports.

```javascript
fc(0.5)
  .plus('12/25')
  .minus('1/3')
  .times('1 7/9')
  .div('2.1(3)')
  .mod('9/8')
  .toString(); // 97/180
```

#### pow(n)

`pow` can only support a `number` parameter, it is `Math.pow(numerator, n)/Math.pow(denominator, n)`, if `denominator` reached Infinity, fraction will be reset to 0. If `numerator` reached `Infinity`, it will throw an error:

```javascript
fc('99999999999/888888888888').pow(9999999); // Error: Numerator reached Infinity
```

#### sqrt()

It is `pow('1/2')`.

If fraction is negative, you call `sqrt`, you will get a error:

```javascript
fc('-5/3').sqrt();    // Error: Sqrt number cannot less than 0
```

#### abs()

Get the absolute value.

```javascript
fc(-0.5).abs().toString();   // "1/2"
```

#### neg()

Get the negation.

```javascript
fc(-0.5).neg().toString();   // "1/2"

fc(0.5).neg().toString();   // "-1/2"
```

#### inverse()

Get the inverse.

```javascript
fc(0.5).inverse().toString();   // "2"
```

#### ceil()

Based on `Math.ceil()`.

#### floor()

Based on `Math.floor()`.

#### round()

Based on `Math.round()`.

### Compare API

There are 3 compare API,`equals`,`greaterThan`,`lessThan`, they also support all kinds of parameter constractor supports. Their return valus are boolean.

```javascript
fc('3/2').equals('6/4');   // true
fc('-3/2').greaterThan('-4/2');  // true
fc('-3/2').lessThan('-4/2');  // false
```

### Display API

#### toString(withWholePart = false)

This API will show fraction in string, but if it is an integer, it will only show integer in string.This api has an optional parameter`withWholePart`, default is `false`. You can set it to `true` to show whole part.

```javascript
fc(5).toString(); // "5"
fc(5.5).toString(); // "11/2"
fc(5.5).toString(true); // "5 1/2"
```

#### toFixed(n)

Based on js `toFixed`:

```javascript
fc('2.5(678)').toFixed(2);   // 2.57
```

#### toNumber()

This will show all decimals JS can handle:

```javascript
fc('2.5(678)').toNumber(2);   // 2.567867867867868
```

#### toRecurringDecimal()

This API will show fraction in recurring decimal, recurring part will marked with `()`:

```javascript
fc('2/9').toRecurringDecimal();  // "0.(2)"
fc('123/456').toRecurringDecimal();  // "0.269(736842105263157894)"
```

### Static API

#### fc.gcd(a, b)

This API will return GCD(greatest common divisor) of a and b. 

```javascript
fc.gcd(18, 27);  // 9
```

But if a or b is NaN or Infinity, it will throw an error:

```javascript
fc.gcd('aaa', 2);  // Error: Invalid Parameter
```

#### fc.lcm(a,b)

This API will return LCM(Lowest Common Multiple) of a and b. 

```javascript
fc.lcm(18, 27);  // 54
```

But if a or b is NaN or Infinity, it will throw an error:

```javascript
fc.lcm('aaa', 2);  // Error: Invalid Parameter
```

### Other API

#### clone()

This API will return a copy of current instance:

```javascript
let fc1 = fc(0.5);
let fc2 = fc1.clone();

fc2.minus('1/3').toString();   //  "1/6"
fc1.toString();   //  "1/2"
```

### Config

This library can work well without any config. But there is still a global config, you can disable reducing.

```javascript
let fc1 = fc('25/45'); 
fc1.toString();     // 5/9

fc.DISABLE_REDUCE = true;  
fc1.toString();     // 25/45

fc.DISABLE_REDUCE = false;  
fc1.toString();     // 5/9
```

If you disable reducing, then call `pow` or `sqrt`, you may get decimals in numerator and denominator:

```javascript
fc.DISABLE_REDUCE = true;  
fc('5/3').pow(1.5).toString();     // "11.180339887498949/5.196152422706632" 
```

