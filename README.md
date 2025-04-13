<!-- Chapter 8 - JavaScript - The Good Parts -->
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h1>Chapter 8</h1>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h2>Methods</h2>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<blockquote cite="—William Shakespeare, The Tragedy of Hamlet, Prince of Denmark">
Though this be madness, yet there is method in ’t.
</blockquote>
<p>JavaScript includes a small set of standard methods that are available on
the standard types.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h2>Array</h2>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>array</i>.concat(<i>item…</i>)</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The concat method produces a new array containing a shallow copy of
this array with the item s appended to it. If an item is an array, then each
of its elements is appended individually. Also see array.push( item...) later
in this chapter.</p>
<!-- <pre> -->
var a = ['a', 'b', 'c'];
var b = ['x', 'y', 'z'];
var c = a.concat(b, true);
// c is ['a', 'b', 'c', 'x', 'y', 'z', true]
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>array</i>.join(<i>separator</i>)</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The join method makes a string from an <i>array</i>. It does this by making a
string of each of the <i>array</i>’s elements, and then concatenating them all
together with a <i>separator</i> between them. The default <i>separator</i> is ','. To join
without separation, use an empty string as the <i>separator</i>.</p>
<p>If you are assembling a string from a large number of pieces, it is usually
faster to put the pieces into an array and join them than it is to
concatenate the pieces with the + operator:</p>
<pre>
var a = ['a', 'b', 'c'];
a.push('d');
var c = a.join(''); // c is 'abcd';
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>array</i>.pop( )</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The pop and push methods make an <i>array</i> work like a stack. The pop
method removes and returns the last element in this <i>array</i>. If the <i>array</i> is
empty, it returns undefined.</p>
<pre>
var a = ['a', 'b', 'c'];
var c = a.pop( ); // a is ['a', 'b'] & c is 'c'
</pre>
<p>pop can be implemented like this:</p>
<pre>
Array.method('pop', function ( ) {
  return this.splice(this.length - 1, 1)[0];
});
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>array</i>.push(<i>item</i>…)</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The push method appends <i>items</i> to the end of an <i>array</i>. Unlike the concat
method, it modifies the array and appends <i>array</i> items whole. It returns
the new length of the <i>array</i>:</p>
<pre>
var a = ['a', 'b', 'c'];
var b = ['x', 'y', 'z'];
var c = a.push(b, true);
// a is ['a', 'b', 'c', ['x', 'y', 'z'], true]
// c is 5;
</pre>
<p>push can be implemented like this:</p>
<pre>
Array.method('push', function ( ) {
  this.splice.apply(
    this,
    [this.length, 0].
    concat(Array.prototype.slice.apply(arguments)));
      return this.length;
});
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>array</i>.reverse( )</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The reverse method modifies the <i>array</i> by reversing the order of the
elements. It returns the <i>array</i>:</p>
<pre>
var a = ['a', 'b', 'c'];
var b = a.reverse( );
// both a and b are ['c', 'b', 'a']
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>array</i>.shift( )</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The shift method removes the first element from an <i>array</i> and returns it. If
the <i>array</i> is empty, it returns undefined. shift is usually much slower than pop:</p>
<pre>
var a = ['a', 'b', 'c'];
var c = a.shift( ); // a is ['b', 'c'] & c is 'a'
</pre>
<p>shift can be implemented like this:</p>
<pre>
Array.method('shift', function ( ) {
  return this.splice(0, 1)[0];
});
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>array</i>.slice(<i>start</i>, <i>end</i>)</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The slice method makes a shallow copy of a portion of an array. The first
element copied will be array[ start]. It will stop before copying array[
end]. The end parameter is optional, and the default is array.length. If
either parameter is negative, array.length will be added to them in an
attempt to make them nonnegative. If start is greater than or equal to
array. length, the result will be a new empty array. Do not confuse slice
with splice. Also see string.slice later in this chapter.</p>
<pre>
var a = ['a', 'b', 'c'];
var b = a.slice(0, 1); // b is ['a']
var c = a.slice(1); // c is ['b', 'c']
var d = a.slice(1, 2); // d is ['b']
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>array</i>.sort(<i>comparefn</i>)</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The sort method sorts the contents of an <i>array</i> in place. It sorts arrays of
numbers incorrectly:</p>
<pre>
var n = [4, 8, 15, 16, 23, 42];
n.sort( );
// n is [15, 16, 23, 4, 42, 8]
</pre>
<p>JavaScript’s default comparison function assumes that the elements to be
sorted are strings. It isn’t clever enough to test the type of the elements
before comparing them, so it converts the numbers to strings as it
compares them, ensuring a shockingly incorrect result.<br>
Fortunately, you may replace the comparison function with your own.
Your comparison function should take two parameters and return 0 if the
two parameters are equal, a negative number if the first parameter should
come first, and a positive number if the second parameter should come
first. (Old-timers might be reminded of the FORTRAN II arithmetic IF
statement.)</p>
<pre>
n.sort(function (a, b) {
  return a - b;
});
// n is [4, 8, 15, 16, 23, 42];
</pre>
<p>That function will sort numbers, but it doesn’t sort strings. If we want to
be able to sort any array of simple values, we must work harder:</p>
<pre>
var m = ['aa', 'bb', 'a', 4, 8, 15, 16, 23, 42];
m.sort(function (a, b) {
  if (a === b) {
    return 0;
  }
  if (typeof a === typeof b) {
    return a &lt;  b ? -1 : 1;
    }
    return typeof a &lt; typeof b ? -1 : 1;
});
// m is &lbrack;4, 8, 15, 16, 23, 42, 'a', 'aa', 'bb'&rbrack;
</pre>
<p>If case is not significant, your comparison function should convert the
operands to lowercase before comparing them. Also see
<i>string</i>.localeCompare later in this chapter.</p>
<p>With a smarter comparison function, we can sort an array of objects. To
make things easier for the general case, we will write a function that will
make comparison functions:</p>
<pre>
// Function by takes a member name string and returns
// a comparison function that can be used to sort an
// array of objects that contain that member.
var by = function (name) {
  return function (o, p) {
    var a, b;
    if (typeof o === 'object' && typeof p ===
      'object' && o && p) {
        a = o[name];
        b = p[name];
        if (a === b) {
          return 0;
        }
        if (typeof a === typeof b) {
          return a &lt; b ? -1 : 1;
        }
        return typeof a &lt; typeof b ? -1 :
          1;
      } else {
        throw {
          name: 'Error',
          message: 'Expected an
          object when sorting by ' + name;
        };
      }
    };
};
var s = [
{first: 'Joe', last: 'Besser'},
{first: 'Moe', last: 'Howard'},
{first: 'Joe', last: 'DeRita'},
{first: 'Shemp', last: 'Howard'},
{first: 'Larry', last: 'Fine'},
{first: 'Curly', last: 'Howard'}
];
s.sort(by('first')); // s is [
// {first: 'Curly', last: 'Howard'},
// {first: 'Joe', last: 'DeRita'},
// {first: 'Joe', last: 'Besser'},
// {first: 'Larry', last: 'Fine'},
// {first: 'Moe', last: 'Howard'},
// {first: 'Shemp', last: 'Howard'}
// ]
</pre>
<p>The sort method is not stable, so:</p>
<pre>
s.sort(by('first')).sort(by('last'));
</pre>
<p>is not guaranteed to produce the correct sequence. If you want to sort on
multiple keys, you again need to do more work. We can modify by to take
a second parameter, another compare method that will be called to break
ties when the major key produces a match:</p>
<pre>
// Function by takes a member name string and an
// optional minor comparison function and returns
// a comparison function that can be used to sort an
// array of objects that contain that member. The
// minor comparison function is used to break ties
// when the o[name] and p[name] are equal.
var by = function (name, minor) {
  return function (o, p) {
    var a, b;
    if (o && p && typeof o === 'object' &&
      typeof p === 'object') {
        a = o[name];
        b = p[name];
        if (a === b) {
          return typeof minor ===
            'function' ? minor(o, p) : 0;
        }
        if (typeof a === typeof b) {
          return a &lt; b ? -1 : 1;
        }
        return typeof a &lt; typeof b ? -1 :
          1;
      } else {
        throw {
          name: 'Error',
          message: 'Expected an
          object when sorting by ' + name;
        };
      }
  };
};
s.sort(by('last', by('first'))); // s is [
// {first: 'Joe', last: 'Besser'},
// {first: 'Joe', last: 'DeRita'},
// {first: 'Larry', last: 'Fine'},
// {first: 'Curly', last: 'Howard'},
// {first: 'Moe', last: 'Howard'},
// {first: 'Shemp', last: 'Howard'}
// ]
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>array</i>.splice(<i>start</i>, <i>deleteCount</i>, <i>item</i>…)</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The <i>splice</i> method removes elements from an <i>array</i>, replacing them with new item 
s. The start parameter is the number of a position within the <i>array</i>. The <i>deleteCount</i> 
parameter is the number of elements to delete starting from that position.
If there are additional parameters, those <i>item</i> s will be inserted at the
position. It returns an array containing the deleted elements.</p>
<p>The most popular use of splice is to delete elements from an array. Do not
confuse splice with slice:</p>
<pre>
var a = ['a', 'b', 'c'];
var r = a.splice(1, 1, 'ache', 'bug');
// a is ['a', 'ache', 'bug', 'c']
// r is ['b']
</pre>
<p>splice can be implemented like this:</p>
<pre>
Array.method('splice', function (start, deleteCount) {
  var max = Math.max,
    min = Math.min,
    delta,
    element,
    insertCount = max(arguments.length - 2,
    0),
    k = 0,
    len = this.length,
    new_len,
    result = [],
    shift_count;
  start = start || 0;
  if (start &lt; 0) {
    start += len;
  }
  start = max(min(start, len), 0);
  deleteCount = max(min(typeof deleteCount ===
    'number' ?
    deleteCount : len, len - start),
    0);
  delta = insertCount - deleteCount;
    new_len = len + delta;
    while (k &lt; deleteCount) {
      element = this[start + k];
      if (element !== undefined) {
        result[k] = element;
      }
      k += 1;
    }
    shift_count = len - start - deleteCount;
    if (delta &lt; 0) {
      k = start + insertCount;
      while (shift_count) {
        this[k] = this[k - delta];
        k += 1;
        shift_count -= 1;
      }
      this.length = new_len;
    } else if (delta &gt; 0) {
      k = 1;
      while (shift_count) {
        this[new_len - k] = this[len -
        k];
        k += 1;
        shift_count -= 1;
      }
    }
    for (k = 0; k &lt; insertCount; k += 1) {
      this[start + k] = arguments[k + 2];
    }
    return result;
});
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>array</i>.unshift(<i>item</i>…)</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The unshift method is like the push method except that it shoves the <i>item</i> s
onto the front of this <i>array</i> instead of at the end. It returns the <i>array</i>’s new
length:</p>
<pre>
var a = ['a', 'b', 'c'];
var r = a.unshift('?', '@');
// a is ['?', '@', 'a', 'b', 'c']
// r is 5
</pre>
<p>unshift can be implemented like this:</p>
<pre>
Array.method('unshift', function ( ) {
  this.splice.apply(this,
  [0,
  0].concat(Array.prototype.slice.apply(arguments)));
  return this.length;
});
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h2>Function</h2>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>function</i>.apply(<i>thisArg</i>, <i>argArray</i>)</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The apply method invokes a function, passing in the object that will be
bound to this and an optional array of arguments. The apply method is
used in the apply invocation pattern (Chapter 4):</p>
<pre>
Function.method('bind', function (that) {
  // Return a function that will call this function as
  // though it is a method of that object.
  var method = this,
    slice = Array.prototype.slice,
    args = slice.apply(arguments, [1]);
    return function ( ) {
      return method.apply(that,
      args.concat(slice.apply(arguments, [0])));
    };
});
var x = function ( ) {
  return this.value;
}.bind({value: 666});
alert(x( )); // 666
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h2>Number</h2>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>number</i>.toExponential(<i>fractionDigits</i>)</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The toExponential method converts this <i>number</i> to a string in the
exponential form. The optional <i>fractionDigits</i> parameter controls the
number of decimal places. It should be between 0 and 20:</p>
<pre>
document.writeln(Math.PI.toExponential(0));
document.writeln(Math.PI.toExponential(2));
document.writeln(Math.PI.toExponential(7));
document.writeln(Math.PI.toExponential(16));
document.writeln(Math.PI.toExponential( ));
</pre>
<p>// Produces</p>
<pre>
3e+0
3.14e+0
3.1415927e+0
3.1415926535897930e+0
3.141592653589793e+0
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>number</i>.toFixed(<i>fractionDigits</i>)</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The toFixed method converts this <i>number</i> to a string in the decimal form.
The optional <i>fractionDigits</i> parameter controls the number of decimal
places. It should be between 0 and 20. The default is 0:</p>
<pre>
document.writeln(Math.PI.toFixed(0));
document.writeln(Math.PI.toFixed(2));
document.writeln(Math.PI.toFixed(7));
document.writeln(Math.PI.toFixed(16));
document.writeln(Math.PI.toFixed( ));
</pre>
<p>// Produces</p>
<pre>
3
3.14
3.1415927
3.1415926535897930
3
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>number</i>.toPrecision(<i>precision</i>)</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The toPrecision method converts this <i>number</i> to a string in the decimal
form. The optional <i>precision</i> parameter controls the number of digits of
precision. It should be between 1 and 21:</p>
<pre>
document.writeln(Math.PI.toPrecision(2));
document.writeln(Math.PI.toPrecision(7));
document.writeln(Math.PI.toPrecision(16));
document.writeln(Math.PI.toPrecision( ));
</pre>
<p>// Produces</p>
<pre>
3.1
3.141593
3.141592653589793
3.141592653589793
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>number</i>.toString(<i>radix</i>)</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The toString method converts this <i>number</i> to a string. The optional <i>radix</i>
parameter controls radix, or base. It should be between 2 and 36. The
default <i>radix</i> is base 10. The <i>radix</i> parameter is most commonly used with
integers, but it can be used on any number.</p>
<p>The most common case, number.toString( ), can be written more simply as</p>
String(<i>number</i>):</p>
<pre>
document.writeln(Math.PI.toString(2));
document.writeln(Math.PI.toString(8));
document.writeln(Math.PI.toString(16));
document.writeln(Math.PI.toString( ));
</pre>
<p>// Produces</p>
<pre>
11.001001000011111101101010100010001000010110100011
3.1103755242102643
3.243f6a8885a3
3.141592653589793
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h2>Object</h2>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>object</i>.hasOwnProperty(<i>name</i>)</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The hasOwnProperty method returns true if the <i>object</i> contains a property
having the <i>name</i>. The prototype chain is not examined. This method is
useless if the <i>name</i> is hasOwnProperty:</p>
<pre>
var a = {member: true};
var b = Object.create(a); // from Chapter 3
var t = a.hasOwnProperty('member'); // t is true
var u = b.hasOwnProperty('member'); // u is false
var v = b.member; // v is true
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h2>RegExp</h2>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>regexp</i>.exec(<i>string</i>)</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The exec method is the most powerful (and slowest) of the methods that
use regular expressions. If it successfully matches the <i>regexp</i> and the <i>string</i>,
it returns an array. The 0 element of the array will contain the substring
that matched the <i>regexp</i>. The 1 element is the text captured by group 1,
the 2 element is the text captured by group 2, and so on. If the match fails,
it returns null.</p>
<p>If the <i>regexp</i> has a g flag, things are a little more complicated. The
searching begins not at position 0 of the string, but at position
<i>regexp</i>.lastIndex (which is initially zero). If the match is successful, then
regexp.lastIndex will be set to the position of the first character after the
match. An unsuccessful match resets <i>regexp</i>.lastIndex to 0.</p>
<p>This allows you to search for several occurrences of a pattern in a string by
calling exec in a loop. There are a couple things to watch out for. If you
exit the loop early, you must reset <i>regexp</i>.lastIndex to 0 yourself before
entering the loop again. Also, the ^ factor matches only when
<i>regexp</i>.lastIndex is 0:</p>
<pre>
// Break a simple html text into tags and texts.
// (See string.replace for the entityify method.)

// For each tag or text, produce an array containing
// [0] The full matched tag or text
// [1] The tag name
// [2] The /, if there is one
// [3] The attributes, if any
var text = '&lt;html&gt;&lt;body bgcolor=linen&gt;&lt;p&gt;' +
  'This is &lt;b&gt;bold&lt;\/b&gt;!&lt;\/p&gt;&lt;\/body&gt;&lt;\/
html>';

var tags = /[^&lt;&gt;]+|&lt;(\/?)([A-Za-z]+)([^&lt;&gt;]*)&gt;/g;
var a, i;

while ((a = tags.exec(text))) {
  for (i = 0; i &lt; a.length; i += 1) {
    document.writeln(('// [' + i + '] ' +
      a[i]).entityify( ));
  }
  document.writeln( );
}
</pre>
<p>// Result:</p>
<pre>
// [0] &lt;html&gt;
// [1]
// [2] html
// [3]
// [0] &lt;body bgcolor=linen&gt;
// [1]
// [2] body
// [3] bgcolor=linen
// [0] &lt;p&gt;
// [1]
// [2] p
// [3]
// [0] This is
// [1] undefined
// [2] undefined
// [3] undefined
// [0] &lt;b&gt;
// [1]
// [2] b
// [3]
// [0] bold
// [1] undefined
// [2] undefined
// [3] undefined
// [0] &lt;/b&gt;
// [1] /
// [2] b
// [3]
// [0] !
// [1] undefined
// [2] undefined
// [3] undefined
// [0] </p>
// [1] /
// [2] p
// [3]
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>regexp</i>.test(<i>string</i>)</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The test method is the simplest (and fastest) of the methods that use
regular expressions. If the regexp matches the string, it returns true;
otherwise, it returns false. Do not use the g flag with this method:</p>
<pre>
var b = /&.+;/.test('frank &amp; beans');
// b is true
</pre>
<p>test could be implemented as:</p>
<pre>
RegExp.method('test', function (string) {
  return this.exec(string) !== null;
});
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h2>String</h2>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>string</i>.charAt(<i>pos</i>)</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The charAt method returns the character at position pos in this string. If
pos is less than zero or greater than or equal to string.length, it returns the
empty string. JavaScript does not have a character type. The result of this
method is a string:</p>
<pre>
var name = 'Curly';
var initial = name.charAt(0); // initial is 'C'
</pre>
<p>charAt could be implemented as:</p>
<pre>
String.method('charAt', function (pos) {
  return this.slice(pos, pos + 1);
});
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>string</i>.charCodeAt(<i>pos</i>)</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The charCodeAt method is the same as charAt except that instead of
returning a string, it returns an integer representation of the code point
value of the character at position pos in that string. If pos is less than zero
or greater than or equal to string.length, it returns NaN:</p>
<pre>
var name = 'Curly';
var initial = name.charCodeAt(0); // initial is 67
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>string</i>.concat(<i>string</i>…)</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The concat method makes a new string by concatenating other strings
together. It is rarely used because the + operator is more convenient:</p>
<pre>
var s = 'C'.concat('a', 't'); // s is 'Cat'
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>string</i>.indexOf(<i>searchString</i>, <i>position</i>)</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The indexOf method searches for a searchString within a string. If it is
found, it returns the position of the first matched character; otherwise, it
returns –1. The optional position parameter causes the search to begin at
some specified position in the string:</p>
<pre>
var text = 'Mississippi';
var p = text.indexOf('ss'); // p is 2
p = text.indexOf('ss', 3); // p is 5
p = text.indexOf('ss', 6); // p is -1
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>string</i>.lastIndexOf(<i>searchString</i>, <i>position</i>)</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The lastIndexOf method is like
the indexOf method, except that it searches from the end of the string
instead of the front:</p>
<pre>
var text = 'Mississippi';
var p = text.lastIndexOf('ss'); // p is 5
p = text.lastIndexOf('ss', 3); // p is 2
p = text.lastIndexOf('ss', 6); // p is 5
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>string</i>.localeCompare(<i>that</i>)</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The localCompare method compares two strings. The rules for how the
strings are compared are not specified. If this string is less than that string,
the result is negative. If they are equal, the result is zero. This is similar to
the convention for the array.sort comparison function:</p>
<pre>
var m = ['AAA', 'A', 'aa', 'a', 'Aa', 'aaa'];
  m.sort(function (a, b) {
    return a.localeCompare(b);
  });
// m (in some locale) is
// ['a', 'A', 'aa', 'Aa', 'aaa', 'AAA']
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>string</i>.match(<i>regexp</i>)</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The match method matches a string and a regular expression. How it does
this depends on the g flag. If there is no g flag, then the result of calling
string.match( regexp) is the same as calling regexp.exec( string). However, if
the regexp has the g flag, then it produces an array of all the matches but
excludes the capturing groups:</p>
<pre>
var text = '&lt;html&gt;&lt;body bgcolor=linen&gt;&lt;p&gt;' +
'This is &lt;b&gt;bold&lt;\/b&gt;!&lt;\/p&gt;&lt;\/body&gt;&lt;\/
html&gt;';
var tags = /[^&lt;&gt;]+|&lt;(\/?)([A-Za-z]+)([^&lt;&gt;]*)&gt;/g;
var a, i;
a = text.match(tags);
for (i = 0; i &lt; a.length; i += 1) {
document.writeln(('// [' + i + '] ' +
a[i]).entityify( ));
}
// The result is
// [0] &lt;html&gt;
// [1] &lt;body bgcolor=linen&gt;
// [2] &lt;p&gt;
// [3] This is
// [4] &lt;b&gt;
// [5] bold
// [6] &lt;/b&gt;
// [7] !
// [8] &lt;/p&gt;
// [9] &lt;/body&gt;
// [10] &lt;/html&gt;
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>string</i>.replace(<i>searchValue</i>, <i>replaceValue</i>)</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The replace method does a search and replace operation on this string,
producing a new string. The <i>searchValue</i> argument can be a string or a
regular expression object. If it is a string, only the first occurrence of the
<i>searchValue</i> is replaced, so:</p>
<pre>
var result = "mother_in_law".replace('_', '-');
</pre>
<p>will produce "mother-in_law", which might be a disappointment. If
<i>searchValue</i> is a regular expression and if it has the g flag, then it will
replace all occurrences. If it does not have the g flag, then it will replace
only the first occurrence.</p>
<p>The <i>replaceValue</i> can be a string or a function. If <i>replaceValue</i> is a string,
the character $ has special meaning:</p>
<pre>
// Capture 3 digits within parens
var oldareacode = /\((\d{3})\)/g;
var p = '(555)666-1212'.replace(oldareacode, '$1-');
// p is '555-555-1212'
</pre>

| Dollar sequence | Replacement |
|-----------------|-----------------|
| $$              | $ |
| $&              | The matched text |
| $number         | Capture group text |
| $`              | The text preceding the match |
| $'              | The text following the match |

<p>If the replaceValue is a function, it will be called for each match, and the
string returned by the function will be used as the replacement text. The
first parameter passed to the function is the matched text. The second
parameter is the text of capture group 1, the next parameter is the text of
capture group 2, and so on:</p>
<pre>
String.method('entityify', function ( ) {
  var character = {
    '&lt;' : '&lt;',
    '&gt;' : '&gt;',
    '&' : '&amp;',
    '"' : '&quot;'
  };
// Return the string.entityify method, which
// returns the result of calling the replace method.
// Its replaceValue function returns the result of
// looking a character up in an object. This use of
// an object usually outperforms switch statements.
return function ( ) {
  return this.replace(/[<>&"]/g, function
    (c) {
      return character[c];
    });
 };
}( ));
alert("&lt;&&gt;".entityify( )); // &lt;&amp;&gt;
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>string.search(<i>regexp</i>)</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The search method is like the indexOf method, except that it takes a
regular expression object instead of a string. It returns the position of the
first character of the first match, if there is one, or –1 if the search fails.
The g flag is ignored. There is no position parameter:</p>
<pre>
var text = 'and in it he says "Any damn fool could';
var pos = text.search(/["']/); // pos is 18
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>string</i>.slice(<i>start</i>, <i>end</i>)</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The slice method makes a new string by copying a portion of another
string. If the start parameter is negative, it adds string.length to it. The end
parameter is optional, and its default value is string.length. If the end
parameter is negative, then string.length is added to it. The end parameter
is one greater than the position of the last character. To get n characters
starting at position p, u se string.slice(p,p + n). Also see string.substring
and array.slice, later and earlier in this chapter, respectively.</p>
<pre>
var text = 'and in it he says "Any damn fool could';
var a = text.slice(18);
// a is '"Any damn fool could'
var b = text.slice(0, 3);
// b is 'and'
var c = text.slice(-5);
// c is 'could'
var d = text.slice(19, 32);
// d is 'Any damn fool'
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>string</i>.split(<i>separator</i>, <i>limit</i>)</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The split method creates an array of strings by splitting this <i>string</i> into
pieces. The optional <i>limit</i> parameter can limit the number of pieces that
will be split. The <i>separator</i> parameter can be a string or a regular
expression.</p>
<p>If the <i>separator</i> is the empty string, an array of single characters is
produced:</p>
<pre>
var digits = '0123456789';
var a = digits.split('', 5);
// a is ['0', '1', '2', '3', '456789']
</pre>
<p>Otherwise, the <i>string</i> is searched for all occurrences of the <i>separator</i>. Each
unit of text between the separators is copied into the array. The g flag is
ignored:</p>
<pre>
var ip = '192.168.1.0';
var b = ip.split('.');
// b is ['192', '168', '1', '0']
var c = '|a|b|c|'.split('|');
// c is ['', 'a', 'b', 'c', '']
var text = 'last, first ,middle';
var d = text.split(/\s*,\s*/);
// d is [
// 'last',
// 'first',
// 'middle'
// ]
</pre>
<p>There are some special cases to watch out for. Text from capturing groups
will be included in the split:</p>
<pre>
var e = text.split(/\s*(,)\s*/);
// e is [
// 'last',
// ',',
// 'first',
// ',',
// 'middle'
// ]
</pre>
<p>Some implementations suppress empty strings in the output array when
the separator is a regular expression:</p>
<pre>
var f = '|a|b|c|'.split(/\|/);
// f is ['a', 'b', 'c'] on some systems, and
// f is ['', 'a', 'b', 'c', ''] on others
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>string</i>.substring(<i>start</i>, <i>end</i>)</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The substring method is the same as the slice method except that it doesn’t
handle the adjustment for negative parameters. There is no reason to use
the substring method. Use slice instead.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>string</i>.toLocaleLowerCase( )</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The toLocaleLowerCase method produces a new string that is made by
converting this <i>string</i> to lowercase using the rules for the locale. This is
primarily for the benefit of Turkish because in that language ‘I’ converts to
ı, not ‘i’.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>string</i>.toLocaleUpperCase( )</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
The toLocaleUpperCase method produces a new string that is made by
converting this <i>string</i> to uppercase using the rules for the locale. This is
primarily for the benefit of Turkish, because in that language ‘i’ converts
to ‘ ’, not ‘I’.
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>string</i>.toLowerCase( )</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The toLowerCase method produces a new string that is made by
converting this <i>string</i> to lowercase.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>string</i>.toUpperCase( )</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The toUpperCase method produces a new string that is made by
converting this <i>string</i> to uppercase.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>String</i>.fromCharCode(<i>char</i>…)</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The String.fromCharCode function produces a string from a series of
numbers.</p>
<pre>
var a = String.fromCharCode(67, 97, 116);
// a is 'Cat'
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h1>CHAPTER 9</h1>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h2>Style</h2>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
