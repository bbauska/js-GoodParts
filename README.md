<!-- JavaScript - The Good Parts js-GoodParts -->
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h1>Chapter 4</h1>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h2>Functions</h2>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
The best thing about JavaScript is its implementation of functions. It got
almost everything right. But, as you should expect with JavaScript, it
didn’t get everything right.
A function encloses a set of statements. Functions are the fundamental
modular unit of JavaScript. They are used for code reuse, information
hiding, and composition. Functions are used to specify the behavior of
objects. Generally, the craft of programming is the factoring of a set of
requirements into a set of functions and data structures.
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Function Objects</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
Functions in JavaScript are objects. Objects are collections of name/value
pairs having a hidden link to a prototype object. Objects produced from
object literals are linked to Object.prototype. Function objects are linked
to Function.prototype (which is itself linked to Object.prototype). Every
function is also created with two additional hidden properties: the
function’s context and the code that implements the function’s behavior.
Every function object is also created with a prototype property. Its value is
an object with a constructor property whose value is the function. This is
distinct from the hidden link to Function.prototype. The meaning of this
convoluted construction will be revealed in the next chapter.
Since functions are objects, they can be used like any other value.
Functions can be stored in variables, objects, and arrays. Functions can be
passed as arguments to functions, and functions can be returned from
functions. Also, since functions are objects, functions can have methods.
The thing that is special about functions is that they can be invoked.
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Function Literal</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Function objects are created with function literals:</p>
<pre>
// Create a variable called add and store a function
// in it that adds two numbers.
var add = function (a, b) {
  return a + b;
};
</pre>
<p>A function literal has four parts. The first part is the reserved word
function.</p>
<p>The optional second part is the function’s name. The function can use its
name to call itself recursively. The name can also be used by debuggers
and development tools to identify the function. If a function is not given a
name, as shown in the previous example, it is said to be <i>anonymous</i>.</p>
<p>The third part is the set of parameters of the function, wrapped in
parentheses. Within the parentheses is a set of zero or more parameter
names, separated by commas. These names will be defined as variables in
the function. Unlike ordinary variables, instead of being initialized to
undefined, they will be initialized to the arguments supplied when the
function is invoked.</p>
<p>The fourth part is a set of statements wrapped in curly braces. These
statements are the body of the function. They are executed when the
function is invoked.</p>
<p>A function literal can appear anywhere that an expression can appear.
Functions can be defined inside of other functions. An inner function of
course has access to its parameters and variables. An inner function also
enjoys access to the parameters and variables of the functions it is nested
within. The function object created by a function literal contains a link to
that outer context. This is called closure. This is the source of enormous
expressive power.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Invocation</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Invoking a function suspends the execution of the current function,
passing control and parameters to the new function. In addition to the
declared parameters, every function receives two additional parameters:
this and arguments. The this parameter is very important in object
oriented programming, and its value is determined by the invocation
pattern. There are four patterns of invocation in JavaScript: the method
invocation pattern, the function invocation pattern, the constructor
invocation pattern, and the apply invocation pattern. The patterns differ in
how the bonus parameter this is initialized.</p>
<p>The invocation operator is a pair of parentheses that follow any expression
that produces a function value. The parentheses can contain zero or more
expressions, separated by commas. Each expression produces one
argument value. Each of the argument values will be assigned to the
function’s parameter names. There is no runtime error when the number
of arguments and the number of parameters do not match. If there are too
many argument values, the extra argument values will be ignored. If there
are too few argument values, the undefined value will be substituted for
the missing values. There is no type checking on the argument values: any
type of value can be passed to any parameter.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>The Method Invocation Pattern</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>When a function is stored as a property of an object, we call it a method.
When a method is invoked, this is bound to that object. If an invocation
expression contains a refinement (that is, a . dot expression or [ subscript]
expression), it is invoked as a method:</p>
<pre>
// Create myObject. It has a value and an increment
// method. The increment method takes an optional
// parameter. If the argument is not a number, then 1
// is used as the default.

var myObject = {
  value: 0,
  increment: function (inc) {
    this.value += typeof inc === 'number' ?
    inc : 1;
  }
};
myObject.increment( );
document.writeln(myObject.value); // 1

myObject.increment(2);
document.writeln(myObject.value); // 3
</pre>
<p>A method can use this to access the object so that it can retrieve values
from the object or modify the object. The binding of this to the object
happens at invocation time. This very late binding makes functions that
use this highly reusable. Methods that get their object context from this
are called public methods.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>The Function Invocation Pattern</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>When a function is not the property of an object, then it is invoked as a
function:</p>
<pre>
var sum = add(3, 4); // sum is 7
</pre>
<p>When a function is invoked with this pattern, this is bound to the global
object. This was a mistake in the design of the language. Had the language
been designed correctly, when the inner function is invoked, this would
still be bound to the this variable of the outer function. A consequence of
this error is that a method cannot employ an inner function to help it do
its work because the inner function does not share the method’s access to
the object as its this is bound to the wrong value. Fortunately, there is an
easy workaround. If the method defines a variable and assigns it the value
of this, the inner function will have access to this through that variable. By
convention, the name of that variable is that:</p>
<pre>
// Augment myObject with a double method.
myObject.double = function ( ) {
  var that = this; // Workaround.
  var helper = function ( ) {
    that.value = add(that.value, that.value);
  };
  helper( ); // Invoke helper as a function.
};
// Invoke double as a method.
myObject.double( );
document.writeln(myObject.getValue( )); // 6
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>The Constructor Invocation Pattern</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>JavaScript is a prototypal inheritance language. That means that objects
can inherit properties directly from other objects. The language is class-
free.</p>
<p>This is a radical departure from the current fashion. Most languages today
are classical. Prototypal inheritance is powerfully expressive, but is not
widely understood. JavaScript itself is not confident in its prototypal
nature, so it offers an object-making syntax that is reminiscent of the
classical languages. Few classical programmers found prototypal
inheritance to be acceptable, and classically inspired syntax obscures the
language’s true prototypal nature. It is the worst of both worlds.</p>
<p>If a function is invoked with the new prefix, then a new object will be
created with a hidden link to the value of the function’s prototype
member, and this will be bound to that new object.</p>
<p>The new prefix also changes the behavior of the return statement. We will
see more about that next.</p>
<pre>
// Create a constructor function called Quo.
// It makes an object with a status property.
var Quo = function (string) {
  this.status = string;
};
// Give all instances of Quo a public method
// called get_status.
Quo.prototype.get_status = function ( ) {
  return this.status;
};
// Make an instance of Quo.
var myQuo = new Quo("confused");
document.writeln(myQuo.get_status( )); // confused
</pre>
<p>Functions that are intended to be used with the new prefix are called
constructors. By convention, they are kept in variables with a capitalized
name. If a constructor is called without the new prefix, very bad things
can happen without a compile-time or runtime warning, so the
capitalization convention is really important.</p>
<p>Use of this style of constructor functions is not recommended. We will see
better alternatives in the next chapter.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>The Apply Invocation Pattern</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Because JavaScript is a functional object-oriented language, functions can
have methods.</p>
<p>The apply method lets us construct an array of arguments to use to invoke
a function. It also lets us choose the value of this. The apply method takes
two parameters. The first is the value that should be bound to this. The
second is an array of parameters.</p>
<pre>
// Make an array of 2 numbers and add them.
var array = [3, 4];
var sum = add.apply(null, array); // sum is 7
// Make an object with a status member.
var statusObject = {
  status: 'A-OK'
};
// statusObject does not inherit from Quo.prototype,
// but we can invoke the get_status method on
// statusObject even though statusObject does not have
// a get_status method.
var status =
Quo.prototype.get_status.apply(statusObject);
// status is 'A-OK'
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Arguments</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>A bonus parameter that is available to functions when they are invoked is
the arguments array. It gives the function access to all of the arguments
that were supplied with the invocation, including excess arguments that
were not assigned to parameters. This makes it possible to write functions
that take an unspecified number of parameters:</p>
<pre>
// Make a function that adds a lot of stuff.
// Note that defining the variable sum inside of
// the function does not interfere with the sum
// defined outside of the function. The function
// only sees the inner one.
var sum = function ( ) {
var i, sum = 0;
for (i = 0; i &lt; arguments.length; i += 1) {
sum += arguments[i];
 }
return sum;
};
document.writeln(sum(4, 8, 15, 16, 23, 42)); // 108
</pre>
<p>This is not a particularly useful pattern. In Chapter 6, we will see how we
can add a similar method to an array.</p>
<p>Because of a design error, arguments is not really an array. It is an array-
like object. arguments has a length property, but it lacks all of the array
methods. We will see a consequence of that design error at the end of this
chapter.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Return</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>When a function is invoked, it begins execution with the first statement,
and ends when it hits the } that closes the function body. That causes the
function to return control to the part of the program that invoked the
function.</p>
<p>The return statement can be used to cause the function to return early.
When return is executed, the function returns immediately without
executing the remaining statements.</p>
<p>A function always returns a value. If the return value is not specified, then
undefined is returned.</p>
<p>If the function was invoked with the new prefix and the return value is not
an object, then this (the new object) is returned instead.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Exceptions</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>JavaScript provides an exception handling mechanism. Exceptions are
unusual (but not completely unexpected) mishaps that interfere with the
normal flow of a program. When such a mishap is detected, your program
should throw an exception:</p>
<pre>
var add = function (a, b) {
  if (typeof a !== 'number' || typeof b !==
    'number') {
    throw {
      name: 'TypeError',
      message: 'add needs numbers'
    };
  }
  return a + b;
}
</pre>
<p>The throw statement interrupts execution of the function. It should be
given an exception object containing a name property that identifies the
type of the exception, and a descriptive message property. You can also
add other properties.</p>
<p>The exception object will be delivered to the catch clause of a try
statement:</p>
<pre>
// Make a try_it function that calls the new add
// function incorrectly.
var try_it = function ( ) {
  try {
    add("seven");
  } catch (e) {
    document.writeln(e.name + ': ' +
    e.message);
  }
}
try_it( );
</pre>
<p>If an exception is thrown within a try block, control will go to its catch
clause.</p>
<p>A try statement has a single catch block that will catch all exceptions. If
your handling depends on the type of the exception, then the exception
handler will have to inspect the name to determine the type of the
exception.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Augmenting Types</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>JavaScript allows the basic types of the language to be augmented. In
Chapter 3, we saw that adding a method to Object.prototype makes that
method available to all objects. This also works for functions, arrays,
strings, numbers, regular expressions, and booleans.</p>
<p>For example, by augmenting Function.prototype, we can make a method
available to all functions:</p>
<pre>
Function.prototype.method = function (name, func) {
  this.prototype[name] = func;
  return this;
};
</pre>
<p>By augmenting Function.prototype with a method method, we no longer
have to type the name of the prototype property. That bit of ugliness can
now be hidden.</p>
<p>JavaScript does not have a separate integer type, so it is sometimes
necessary to extract just the integer part of a number. The method
JavaScript provides to do that is ugly. We can fix it by adding an integer
method to Number.prototype. It uses either Math.ceiling or Math.floor,
depending on the sign of the number:</p>
<pre>
Number.method('integer', function ( ) {
return Math[this &lt; 0 ? 'ceiling' : 'floor']
(this);
});
document.writeln((-10 / 3).integer( )); // -3
</pre>
<p>JavaScript lacks a method that removes spaces from the ends of a string.
That is an easy oversight to fix:</p>
<pre>
String.method('trim', function ( ) {
return this.replace(/^\s+|\s+$/g, '');
});
document.writeln('"' + " neat ".trim( ) + '"');
</pre>
<p>Our trim method uses a regular expression. We will see much more about
regular expressions in Chapter 7.</p>
<p>By augmenting the basic types, we can make significant improvements to
the expressiveness of the language. Because of the dynamic nature of
JavaScript’s prototypal inheritance, all values are immediately endowed
with the new methods, even values that were created before the methods
were created.
The prototypes of the basic types are public structures, so care must be
taken when mixing libraries. One defensive technique is to add a method
only if the method is known to be missing:</p>
<pre>
// Add a method conditionally.
Function.prototype.method = function (name, func) {
if (!this.prototype[name]) {
 this.prototype[name] = func;
}
};
</pre>
<p>Another concern is that the for in statement interacts badly with
prototypes. We saw a couple of ways to mitigate that in Chapter 3: we can
use the hasOwnProperty method to screen out inherited properties, and
we can look for specific types.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Recursion</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>A recursive function is a function that calls itself, either directly or
indirectly. Recursion is a powerful programming technique in which a
problem is divided into a set of similar subproblems, each solved with a
trivial solution. Generally, a recursive function calls itself to solve its
subproblems.</p>
<p>The Towers of Hanoi is a famous puzzle. The equipment includes three
posts and a set of discs of various diameters with holes in their centers.
The setup stacks all of the discs on the source post with smaller discs on
top of larger discs. The goal is to move the stack to the destination post by
moving one disc at a time to another post,<br>
never placing a larger disc on a smaller disc. This puzzle has a trivial
recursive solution:</p>
<pre>
var hanoi = function (disc, src, aux, dst) {
if (disc &gt; 0) {
hanoi(disc - 1, src, dst, aux);
document.writeln('Move disc ' + disc +
' from ' + src + ' to ' + dst);
hanoi(disc - 1, aux, src, dst);
}
};
hanoi(3, 'Src', 'Aux', 'Dst');
</pre>
<p>It produces this solution for three discs:</p>
<pre>
Move disc 1 from Src to Dst
Move disc 2 from Src to Aux
Move disc 1 from Dst to Aux
Move disc 3 from Src to Dst
Move disc 1 from Aux to Src
Move disc 2 from Aux to Dst
Move disc 1 from Src to Dst
</pre>
<p>The hanoi function moves a stack of discs from one post to another, using
the auxiliary post if necessary. It breaks the problem into three
subproblems. First, it uncovers the bottom disc by moving the substack
above it to the auxiliary post. It can then move the bottom disc to the
destination post. Finally, it can move the substack from the auxiliary post
to the destination post. The movement of the substack is handled by
calling itself recursively to work out those subproblems.</p>
<p>The hanoi function is passed the number of the disc it is to move and the
three posts it is to use. When it calls itself, it is to deal with the disc that is
above the disc it is currently working on. Eventually, it will be called with
a nonexistent disc number. In that case, it does nothing. That act of
nothingness gives us confidence that the function does not recurse forever.</p>
<p>Recursive functions can be very effective in manipulating tree structures
such as the browser’s Document Object Model (DOM). Each recursive call
is given a smaller piece of the tree to work on:</p>
<pre>
// Define a walk_the_DOM function that visits every
// node of the tree in HTML source order, starting
// from some given node. It invokes a function,
// passing it each node in turn. walk_the_DOM calls
// itself to process each of the child nodes.
var walk_the_DOM = function walk(node, func) {
func(node);
node = node.firstChild;
while (node) {
walk(node, func);
node = node.nextSibling;
}
};
// Define a getElementsByAttribute function. It
// takes an attribute name string and an optional
// matching value. It calls walk_the_DOM, passing it a
// function that looks for an attribute name in the
// node. The matching nodes are accumulated in a
// results array.
var getElementsByAttribute = function (att, value) {
var results = [];
walk_the_DOM(document.body, function (node) {
var actual = node.nodeType === 1 &&
node.getAttribute(att);
if (typeof actual === 'string' &&
 (actual === value || typeof value
!== 'string')) {
results.push(node);
}
});
return results;
};
</pre>
<p>Some languages offer the tail recursion optimization. This means that if a
function returns the result of invoking itself recursively, then the
invocation is replaced with a loop, which can significantly speed things
up. Unfortunately, JavaScript does not currently provide tail recursion
optimization. Functions that recurse very deeply can fail by exhausting the
return stack:</p>
<pre>
// Make a factorial function with tail
// recursion. It is tail recursive because
// it returns the result of calling itself.
// JavaScript does not currently optimize this form.
var factorial = function factorial(i, a) {
a = a || 1;
if (i &lt; 2) {
return a;
}
return factorial(i - 1, a * i);
};
document.writeln(factorial(4)); // 24
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Scope</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Scope in a programming language controls the visibility and lifetimes of
variables and parameters. This is an important service to the programmer
because it reduces nam-ing collisions and provides automatic memory
management:</p>
<pre>
var foo = function ( ) {
var a = 3, b = 5;
var bar = function ( ) {
var b = 7, c = 11;
// At this point, a is 3, b is 7, and c is 11
 a += b + c;
// At this point, a is 21, b is 7, and c is 11
};
// At this point, a is 3, b is 5, and c is not defined
bar( );
// At this point, a is 21, b is 5
};
</pre>
<p>Most languages with C syntax have block scope. All variables defined in a
block (a list of statements wrapped with curly braces) are not visible from
outside of the block. The variables defined in a block can be released
when execution of the block is finished. This is a good thing.</p>
<p>Unfortunately, JavaScript does not have block scope even though its block
syntax suggests that it does. This confusion can be a source of errors.</p>
<p>JavaScript does have function scope. That means that the parameters and
variables defined in a function are not visible outside of the function, and
that a variable defined anywhere within a function is visible everywhere
within the function.</p>
<p>In many modern languages, it is recommended that variables be declared
as late as possible, at the first point of use. That turns out to be bad advice
for JavaScript because it lacks block scope. So instead, it is best to declare
all of the variables used in a function at the top of the function body.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Closure</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The good news about scope is that inner functions get access to the
parameters and variables of the functions they are defined within (with
the exception of this and arguments). This is a very good thing.</p>
<p>Our getElementsByAttribute function worked because it declared a results
variable, and the inner function that it passed to walk_the_DOM also had
access to the results variable.</p>
<p>A more interesting case is when the inner function has a longer lifetime
than its outer function.</p>
<p>Earlier, we made a myObject that had a value and an increment method.
Suppose we wanted to protect the value from unauthorized changes.</p>
<p>Instead of initializing myObject with an object literal, we will initialize
myObject by calling a function that returns an object literal. That function
defines a value variable. That variable is always available to the increment
and getValue methods, but the function’s scope keeps it hidden from the
rest of the program:</p>
<pre>
var myObject = function ( ) {
var value = 0;
return {
increment: function (inc) {
value += typeof inc === 'number'
? inc : 1;
},
getValue: function ( ) {
return value;
}
};
}( );
</pre>
<p>We are not assigning a function to myObject. We are assigning the result
of invoking that function. Notice the ( ) on the last line. The function
returns an object containing two methods, and those methods continue to
enjoy the privilege of access to the value variable.</p>
<p>The Quo constructor from earlier in this chapter produced an object with a
status property and a get_status method. But that doesn’t seem very
interesting. Why would you call a getter method on a property you could
access directly? It would be more useful if the status property were
private. So, let’s define a different kind of quo function to do that:</p>
<pre>
// Create a maker function called quo. It makes an
// object with a get_status method and a private
// status property.
var quo = function (status) {
return {
get_status: function ( ) {
return status;
}
};
};
// Make an instance of quo.
var myQuo = quo("amazed");
document.writeln(myQuo.get_status( ));
</pre>
<p>This quo function is designed to be used without the new prefix, so the
name is not capitalized. When we call quo, it returns a new object
containing a get_status method. A reference to that object is stored in
myQuo. The get_status method still has privileged access to quo’s status
property even though quo has already returned. get_status does not have
access to a copy of the parameter; it has access to the parameter itself.
This is possible because the function has access to the context in which it
was created. This is called closure.</p>
<p>Let’s look at a more useful example:</p>
<pre>
// Define a function that sets a DOM node's color
// to yellow and then fades it to white.
var fade = function (node) {
var level = 1;
var step = function ( ) {
var hex = level.toString(16);
node.style.backgroundColor = '#FFFF' +
hex + hex;
if (level &lt; 15) {
level += 1;
setTimeout(step, 100);
}
};
setTimeout(step, 100);
};
fade(document.body);
</pre>
<p>We call fade, passing it document.body (the node created by the HTML
&lt;body&gt; tag). fade sets level to 1. It defines a step function. It calls
setTimeout, passing it the step function and a time (100 milliseconds). It
then returns—fade has finished.</p>
<p>Suddenly, about a 10th of a second later, the step function gets invoked. It
makes a base 16 character from fade’s level. It then modifies the
background color of fade’s node. It then looks at fade’s level. If it hasn’t
gotten to white yet, it then increments fade’s level and uses setTimeout to
schedule itself to run again.</p>
<p>Suddenly, the step function gets invoked again. But this time, fade’s level
is 2. fade returned a while ago, but its variables continue to live as long as
they are needed by one or more of fade’s inner functions.
It is important to understand that the inner function has access to the
actual variables of the outer functions and not copies in order to avoid the
following problem:</p>
<pre>
// BAD EXAMPLE
// Make a function that assigns event handler functions
to an array of nodes the wrong way.
// When you click on a node, an alert box is supposed to
display the ordinal of the node.
// But it always displays the number of nodes instead.
var add_the_handlers = function (nodes) {
var i;
for (i = 0; i &lt; nodes.length; i += 1) {
nodes[i].onclick = function (e) {
alert(i);
};
}
};
// END BAD EXAMPLE
</pre>
<p>The add_the_handlers function was intended to give each handler a unique
number (i). It fails because the handler functions are bound to the variable
i, not the value of the variable i at the time the function was made:</p>
<pre>
// BETTER EXAMPLE
// Make a function that assigns event handler functions
to an array of nodes the right way.
// When you click on a node, an alert box will display
the ordinal of the node.
var add_the_handlers = function (nodes) {
var i;
for (i = 0; i &lt; nodes.length; i += 1) {
nodes[i].onclick = function (i) {
return function (e) {
alert(e);
};
}(i);
}
};
</pre>
<p>Now, instead of assigning a function to onclick, we define a function and
immediately invoke it, passing in i. That function will return an event
handler function that is bound to the value of i that was passed in, not to
the i defined in add_the_ handlers. That returned function is assigned to
onclick.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Callbacks</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Functions can make it easier to deal with discontinuous events. For
example, suppose there is a sequence that begins with a user interaction,
making a request of the server, and finally displaying the server’s
response. The naïve way to write that would be:</p>
<pre>
request = prepare_the_request( );
response = send_request_synchronously(request);
display(response);
</pre>
<p>The problem with this approach is that a synchronous request over the
network will leave the client in a frozen state. If either the network or the
server is slow, the degradation in responsiveness will be unacceptable.</p>
<p>A better approach is to make an asynchronous request, providing a
callback function that will be invoked when the server’s response is
received. An asynchronous function returns immediately, so the client isn’t
blocked:</p>
<pre>
request = prepare_the_request( );
{send_request_asynchronously(request, function (response)
display(response);
});
</pre>
<p>We pass a function parameter to the send_request_asynchronously function
that will be called when the response is available.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Module</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>We can use functions and closure to make modules. A module is a function
or object that presents an interface but that hides its state and
implementation. By using functions to produce modules, we can almost
completely eliminate our use of global variables, thereby mitigating one of
JavaScript’s worst features.</p>
<p>For example, suppose we want to augment String with a deentityify
method. Its job is to look for HTML entities in a string and replace them
with their equivalents. It makes sense to keep the names of the entities
and their equivalents in an object. But where should we keep the object?
We could put it in a global variable, but global variables are evil. We
could define it in the function itself, but that has a runtime cost because
the literal must be evaluated every time the function is invoked. The ideal
approach is to put it in a closure, and perhaps provide an extra method
that can add additional entities:</p>
<pre>
String.method('deentityify', function ( ) {
// The entity table. It maps entity names to
// characters.
var entity = {
quot: '"',
lt: '&lt;',
gt: '&gt;'
};
// Return the deentityify method.
return function ( ) {
// This is the deentityify method. It calls the string
// replace method, looking for substrings that start
// with '&' and end with ';'. If the characters in
// between are in the entity table, then replace the
// entity with the character from the table. It uses
// a regular expression (Chapter 7).
return this.replace(/&([^&;]+);/g,
function (a, b) {
var r = entity[b];
return typeof r ===
'string' ? r : a;
}
);
};
}( ));
</pre>
<p>Notice the last line. We immediately invoke the function we just made
with the ( ) operator. That invocation creates and returns the function that
becomes the deentityify method.</p>
<pre>
document.writeln(
'&lt;&quot;&gt;'.deentityify( )); // &lt;"&gt;
</pre>
<p>The module pattern takes advantage of function scope and closure to
create relationships that are binding and private. In this example, only the
deentityify method has access to the entity data structure.</p>
<p>The general pattern of a module is a function that defines private variables
and functions; creates privileged functions which, through closure, will
have access to the private variables and functions; and that returns the
privileged functions or stores them in an accessible place.</p>
<p>Use of the module pattern can eliminate the use of global variables. It
promotes information hiding and other good design practices. It is very
effective in encapsulating applications and other singletons.</p>
<p>It can also be used to produce objects that are secure. Let’s suppose we
want to make an object that produces a serial number:</p>
<pre>
var serial_maker = function ( ) {
// Produce an object that produces unique strings. A
// unique string is made up of two parts: a prefix
// and a sequence number. The object comes with
// methods for setting the prefix and sequence
// number, and a gensym method that produces unique
// strings.
var prefix = '';
var seq = 0;
return {
set_prefix: function (p) {
prefix = String(p);
},
set_seq: function (s) {
seq = s;
},
gensym: function ( ) {
var result = prefix + seq;
seq += 1;
return result;
}
};
};
var seqer = serial_maker( );
seqer.set_prefix = ('Q';)
seqer.set_seq = (1000);
var unique = seqer.gensym( ); // unique is "Q1000"
</pre>
<p>The methods do not make use of this or that. As a result, there is no way
to compromise the seqer. It isn’t possible to get or change the prefix or seq
except as permitted by the methods. The seqer object is mutable, so the
methods could be replaced, but that still does not give access to its secrets.
seqer is simply a collection of functions, and those functions are
capabilities that grant specific powers to use or modify the secret state.</p>
<p>If we passed seqer.gensym to a third party’s function, that function would
be able to generate unique strings, but would be unable to change the
prefix or seq.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Cascade</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Some methods do not have a return value. For example, it is typical for
methods that set or change the state of an object to return nothing. If we
have those methods return this instead of undefined, we can enable
cascades. In a cascade, we can call many methods on the same object in
sequence in a single statement. An Ajax library that enables cascades
would allow us to write in a style like this:</p>
<pre>
getElement('myBoxDiv').
move(350, 150).
width(100).
height(100).
color('red').
border('10px outset').
padding('4px').
appendText("Please stand by").
on('mousedown', function (m) {
this.startDrag(m, this.getNinth(m));
}).
on('mousemove', 'drag').
on('mouseup', 'stopDrag').
later(2000, function ( ) {
this.
color('yellow').
setHTML("What hath God
wraught?").
slide(400, 40, 200, 200);
}).
tip('This box is resizeable');
</pre>
<p>In this example, the getElement function produces an object that gives
functionality to the DOM element with id="myBoxDiv". The methods
allow us to move the element, change its dimensions and styling, and add
behavior. Each of those methods returns the object, so the result of the
invocation can be used for the next invocation.</p>
<p>Cascading can produce interfaces that are very expressive. It can help
control the tendency to make interfaces that try to do too much at once.</p>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Curry</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Functions are values, and we can manipulate function values in interesting
ways. Currying allows us to produce a new function by combining a
function and an argument:</p>
<pre>
var add1 = add.curry(1);
document.writeln(add1(6)); // 7
</pre>
<p>add1 is a function that was created by passing 1 to add’s curry method.</p>
<p>The add1 function adds 1 to its argument. JavaScript does not have a
curry method, but we can fix that by augmenting Function.prototype:</p>
<pre>
Function.method('curry', function ( ) {
var args = arguments, that = this;
return function ( ) {
return that.apply(null,
args.concat(arguments));
};
}); // Something isn't right...
</pre>
<p>The curry method works by creating a closure that holds that original
function and the arguments to curry. It returns a function that, when
invoked, returns the result of calling that original function, passing it all of
the arguments from the invocation of curry and the current invocation. It
uses the Array concat method to concatenate the two arrays of arguments
together.</p>
<p>Unfortunately, as we saw earlier, the arguments array is not an array, so it
does not have the concat method. To work around that, we will apply the
array slice method on both of the arguments arrays. This produces arrays
that behave correctly with the concat method:</p>
<pre>
Function.method('curry', function ( ) {
var slice = Array.prototype.slice,
args = slice.apply(arguments),
that = this;
return function ( ) {
return that.apply(null,
args.concat(slice.apply(arguments)));
};
});
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3>Memoization</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>Functions can use objects to remember the results of previous operations,
making it possible to avoid unnecessary work. This optimization is called
memoization. JavaScript’s objects and arrays are very convenient for this.</p>
<p>Let’s say we want a recursive function to compute Fibonacci numbers. A
Fibonacci number is the sum of the two previous Fibonacci numbers. The
first two are 0 and 1:</p>
<pre>
var fibonacci = function (n) {
return n &lt; 2 ? n : fibonacci(n - 1) + fibonacci(n
- 2);
};
for (var i = 0; i &lt;= 10; i += 1) {
document.writeln('// ' + i + ': ' +
fibonacci(i));
}
// 0: 0
// 1: 1
// 2: 1
// 3: 2
// 4: 3
// 5: 5
// 6: 8
// 7: 13
// 8: 21
// 9: 34
// 10: 55
</pre>
<p>This works, but it is doing a lot of unnecessary work. The fibonacci
function is called 453 times. We call it 11 times, and it calls itself 442
times in computing values that were probably already recently computed.
If we memoize the function, we can significantly reduce its workload.</p>
<p>We will keep our memoized results in a memo array that we can hide in a
closure.</p>
<p>When our function is called, it first looks to see if it already knows the
result. If it does, it can immediately return it:</p>
<pre>
var fibonacci = function ( ) {
 var memo = [0, 1];
var fib = function (n) {
var result = memo[n];
if (typeof result !== 'number') {
result = fib(n - 1) + fib(n - 2);
memo[n] = result;
}
return result;
};
return fib;
}( );
</pre>
<p>This function returns the same results, but it is called only 29 times. We
called it 11 times. It called itself 18 times to obtain the previously
memoized results.</p>
<p>We can generalize this by making a function that helps us make memoized
functions. The memoizer function will take an initial memo array and the
fundamental function. It returns a shell function that manages the memo
store and that calls the fundamental function as needed. We pass the shell
function and the function’s parameters to the fundamental function:</p>
<pre>
var memoizer = function (memo, fundamental) {
var shell = function (n) {
var result = memo[n];
if (typeof result !== 'number') {
result = fundamental(shell, n);
memo[n] = result;
}
return result;
};
return shell;
};
</pre>
<p>We can now define fibonacci with the memoizer, providing the initial
memo array and fundamental function:</p>
<pre>
var fibonacci = memoizer([0, 1], function (shell, n) {
return shell(n - 1) + shell(n - 2);
});
</pre>
<p>By devising functions that produce other functions, we can significantly
reduce the amount of work we have to do. For example, to produce a
memoizing factorial function, we only need to supply the basic factorial
formula:</p>
<pre>
var factorial = memoizer([1, 1], function (shell, n) {
 return n * shell(
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
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
// [0] &lt;/p&gt;
// [1] /
// [2] p
// [3]
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>regexp</i>.test(<i>string</i>)</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The test method is the simplest (and fastest) of the methods that use
regular expressions. If the <i>regexp</i> matches the <i>string</i>, it returns true;
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
<p>The charAt method returns the character at position <i>pos</i> in this <i>string</i>. If
<i>pos</i> is less than zero or greater than or equal to <i>string</i>.length, it returns the
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
value of the character at position <i>pos</i> in that <i>string</i>. If <i>pos</i> is less than zero
or greater than or equal to <i>string</i>.length, it returns NaN:</p>
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
<p>The indexOf method searches for a <i>searchString</i> within a <i>string</i>. If it is
found, it returns the position of the first matched character; otherwise, it
returns –1. The optional <i>position</i> parameter causes the search to begin at
some specified position in the <i>string</i>:</p>
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
strings are compared are not specified. If this <i>string</i> is less than <i>that</i> string,
the result is negative. If they are equal, the result is zero. This is similar to
the convention for the <i>array</i>.sort comparison function:</p>
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
string.match( regexp) is the same as calling <i>regexp</i>.exec(<i>string</i>). However, if
the <i>regexp</i> has the g flag, then it produces an array of all the matches but
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
</pre>
<p>// The result is</p>
<pre>
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
<p>The replace method does a search and replace operation on this <i>string</i>,
producing a new string. The <i>searchValue</i> argument can be a string or a
regular expression object. If it is a string, <i>only</i> the first occurrence of the
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

<p>If the <i>replaceValue</i> is a function, it will be called for each match, and the
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
The g flag is ignored. There is no <i>position</i> parameter:</p>
<pre>
var text = 'and in it he says "Any damn fool could';
var pos = text.search(/["']/); // pos is 18
</pre>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<h3><i>string</i>.slice(<i>start</i>, <i>end</i>)</h3>
<!--~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~-->
<p>The slice method makes a new string by copying a portion of another
<i>string</i>. If the <i>start</i> parameter is negative, it adds <i>string</i>.length to it. 
The <i>end</i> parameter is optional, and its default value is <i>string</i>.length. If the <i>end</i> 
parameter is negative, then <i>string</i>.length is added to it. The <i>end</i> parameter
is one greater than the position of the last character. To get n characters
starting at position p, use <i>string</i>.slice(p,p + n). Also see <i>string</i>.substring
and <i>array</i>.slice, later and earlier in this chapter, respectively.</p>
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
