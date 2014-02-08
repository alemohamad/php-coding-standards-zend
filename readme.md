# Zend PHP Coding Standards

## Overview

### Scope

[This document](http://framework.zend.com/wiki/display/ZFDEV2/Coding+Standards) 
provides guidelines for code formatting and documentation to individuals and 
teams contributing to Zend Framework. Many developers using Zend Framework 
have also found these coding standards useful because their code's style 
remains consistent with all Zend Framework code. It is also worth noting that 
it requires significant effort to fully specify coding standards.  

**Note:** Sometimes developers consider the establishment of a standard more 
important than what that standard actually suggests at the most detailed level 
of design. The guidelines in Zend Framework's coding standards capture 
practices that have worked well on the Zend Framework project. You may modify 
these standards or use them as is in accordance with the terms of our 
[license](http://framework.zend.com/license).  

Topics covered in Zend Framework's coding standards include:

* [PHP File Formatting](#php-file-formatting)
* [Naming Conventions](#naming-conventions)
* [Coding Style](#coding-style)
* [Inline Documentation](#inline-documentation)

### Goals

Coding standards are important in any development project, but they are 
particularly important when many developers are working on the same project. 
Coding standards help ensure that the code is high quality, has fewer bugs, 
and can be easily maintained.  

### Conventions

This document attempts to follow [RFC 2119](http://www.ietf.org/rfc/rfc2119.txt)'s 
verbiage for indicating requirements.  

* **MUST** and **MUST NOT** indicate non-optional requirements
* **SHOULD** and **SHOULD NOT** indicate recommendations for which exceptions 
may exist
* **MAY** indicates truly optional requirements

## PHP File Formatting

### General

For files that contain only PHP code, the closing tag (```?>```) **MUST NOT** 
be used. It is not required by PHP, and omitting it prevents the accidental 
injection of trailing white space into the response.  

**Important Note:** Inclusion of arbitrary binary data as permitted by 
```__HALT_COMPILER()``` **MUST NOT** be used in PHP files in the Zend Framework 
project or files derived from them. Use of this feature is only permitted for 
some installation scripts.  

### Indentation

Indentation **MUST** consist of 4 spaces. Tabs **MUST NOT** be used for indentation.

### Maximum Line Length

The target line length is 80 characters. That is to say, Zend Framework 
developers **SHOULD** strive keep each line of their code under 80 characters 
where possible and practical. However, longer lines are acceptable in some 
circumstances. The maximum length of any line of PHP code is 120 characters.  

### Line Termination

Line termination follows the Unix text file convention. Lines **MUST** end 
with a single linefeed (```LF```) character. Linefeed characters are 
represented as ordinal 10, or hexadecimal ```0x0A```.  

**Note:** Do not use carriage returns (```CR```) as is the convention in 
Apple OS's (```0x0D```) or the carriage return - linefeed combination 
(```CRLF```) as is standard for the Windows OS (```0x0D```, ```0x0A```).  

There is one exception to this rule: the last line of a file **MUST NOT** end 
in a linefeed.  

Finally, lines **MUST NOT** have whitespace characters preceding the linefeed 
character. This is to prevent versioning differences that affect only white 
characters at line endings.  

## Naming Conventions

Zend Framework standardizes on a class naming convention whereby:  

* Namespaces have a 1:1 relationship to the filesystem.
* Classes are stored within the directory determined by the namespace.

The root directory of a library contains a directory named after the top-level 
namespace; in the case of Zend Framework, the root directory would be 
"library/", as it contains the "Zend/" directory, which corresponds to the 
top-level namespace within the Zend Framework distribution.  

### Namespaces

Namespaces **MUST** contain only alphanumeric characters, the underscore, and, 
of course, the namespace separator (```{{}}```).  

Namespaces **SHOULD** be MixedCase, and acronyms used in namespaces **SHOULD** 
as well. As examples:  

* the namespace "Zend\PDF" would not be allowed, while "Zend\Pdf" is 
acceptable.
* the namespace "Zend\XMLRPC" would not be allowed, while "Zend\XmlRpc" is 
acceptable.

Underscores in namespaces have no special meaning, and will not be translated 
to the directory separator.  

Code deployed alongside Zend Framework libraries but which is not part of the 
Zend Framework standard distribution **SHOULD** utilize separate namespaces.  

### Namespace Aliases

When aliasing within ZF library code, the aliases **SHOULD** typically follow 
these patterns:  

* If aliasing a namespace, use the final segment of the namespace; this can be 
accomplished by simply omitting the "as" portion of the alias:

```php
use Zend\Filter;       // Alias is then "Filter"
use Zend\Form\Element; // Alias is "Element"
```

* If aliasing a class, either use the class name (no "as" clause), or suffix 
the class with the subcomponent namespace preceding it:

```php
use Zend\Controller\Action\HelperBroker; // aliased as "HelperBroker"
use Zend\Filter\Int as IntFilter;        // using suffix
```

See also the section on "Import Statements" for additional standards related 
to aliasing and imports.  

### Classes

Class names **MUST** contain only alphanumeric characters and the underscore. 
Numbers are permitted in class names but are discouraged in most cases. 
Underscores are only permitted in place of the path separator; a class named 
"Do_Something" would map to the filename "Do/Something.php" (and potentially 
under a hierarchy named after any.  

If a class name is comprised of more than one word, the first letter of each 
new word **MUST** be capitalized. Successive capitalized letters are not 
allowed, e.g. a class "Zend_PDF" is not allowed while "Zend_Pdf" is 
acceptable.  

See the class names in the standard and extras libraries for examples of this 
classname convention.  

### Abstract Classes

Abstract classes follow the same conventions as classes, with one additional 
rule: abstract class names **SHOULD** begin with the term, "Abstract". As 
examples, "AbstractAdapter" and "AbstractWriter" are both considered valid 
abstract class names.  

Abstract classes **SHOULD** be in the same namespace as concrete 
implementations. The following would be considered invalid usage:  

```php
namespace Zend\Log;

abstract class AbstractWriter implements Writer
{
}

namespace Zend\Log\Writer;

use Zend\Log\AbstractWriter;

class StreamWriter extends AbstractWriter
{
}
```

While the next example displays proper usage:  

```php
namespace Zend\Log\Writer;

use Zend\Log\Writer;

abstract class AbstractWriter implements Writer
{
}

class StreamWriter extends AbstractWriter
{
}
```

The "Abstract" prefix **MAY** be omitted from the class name in the case of 
abstract classes that implement only static functionality, such as factories. 
As an example, the following is valid usage:  

```php
namespace Zend\Uri;

abstract class UriFactory
{
    public static function factory($uri)
    {
        // work goes in here...
    }
}
```

### Interfaces

Interfaces follow the same conventions as classes, with two additional rules: 
interface **MUST** be nouns or adjectives and interface class names **SHOULD** 
end with the term, "Interface". As examples, "ServiceLocationInterface", 
"EventCollectionInterface", and "PluginLocatorInterface" are all considered 
appropriate interface names.  

Interfaces **MUST** be in the same namespace as concrete implementations.  

```php
namespace Zend\Log\Writer;

interface class WriterInterface
{
}
```

### Filenames

All other PHP files **MUST** only use alphanumeric characters, underscores, 
and the dash character ("-"). Spaces are strictly prohibited.  

Any file that contains PHP code **SHOULD** end with the extension ".php", with 
the notable exception of view scripts. The following examples show acceptable 
filenames for Zend Framework classes:  

```php
Zend/Db.php

Zend/Controller/Front.php

Zend/View/Helper/FormRadio.php
```

File names **MUST** map to class names as described above.  

### Functions and Methods

Function names **MUST** contain only alphanumeric characters. Underscores are 
not permitted. Numbers are permitted in function names but are discouraged.  

Function names **MUST** always start with a lowercase letter. When a function 
name consists of more than one word, the first letter of each new word **MUST** 
be capitalized. This is commonly called "camelCase" formatting.  

Verbosity is encouraged. Function names should be as verbose as is practical to 
fully describe their purpose and behavior.  

These are examples of acceptable names for functions:  

```php
filterInput()

getElementById()

widgetFactory()
```

For object-oriented programming, accessors for instance or static variables 
**SHOULD** be prefixed with "get" or "set". In implementing design patterns, 
such as the singleton or factory patterns, the name of the method **SHOULD** 
contain the pattern name where practical to more thoroughly describe behavior.  

For methods on objects that are declared with the "private" or "protected" 
modifier, the first character of the method name MAY be an underscore. This is 
the only acceptable application of an underscore in a method name, and is 
discouraged (as it makes refactoring to public visibility more difficult). 
Methods declared "public" **SHOULD NOT** contain an underscore.  

Functions in the global scope (a.k.a "floating functions") **MAY** be used, but 
are discouraged. Consider wrapping these functions in a static class, or within 
a specific namespace.  

### Variables

Variable names **MUST** contain only alphanumeric characters. Underscores are 
not permitted. Numbers are permitted in variable names but are discouraged in 
most cases.  

For variables that are declared with ```private``` or ```protected``` visibility, 
the first character of the variable name **MAY** be a single underscore. This 
is the only acceptable application of an underscore in a variable name, and is 
discouraged (as it makes refactoring to public visibility more difficult). 
Member variables declared with public visibility **SHOULD NOT** start with an 
underscore.  

As with function names, variable names **MUST** always start with a lowercase 
letter and follow the "camelCaps" capitalization convention.  

Verbosity is encouraged. Variables should always be as verbose as practical to 
describe the data that the developer intends to store in them. Terse variable 
names such as "```$i```" and "```$n```" are discouraged for all but the smallest loop 
contexts. If a loop contains more than 20 lines of code, the index variables 
should have more descriptive names.  

### Constants

Constant names **MAY** contain both alphanumeric characters and underscores.  

All letters used in a constant name **MUST** be capitalized, while all words 
in a constant name **MUST** be separated by underscore characters.  

For example, ```EMBED_SUPPRESS_EMBED_EXCEPTION``` is permitted but 
```EMBED_SUPPRESSEMBEDEXCEPTION``` is not.  

Constants **MUST** be defined as class members with the "const" modifier. 
Constants **MAY** be defined in the global scope or within namespaces.  

## Coding Style

### PHP Code Demarcation

PHP code **MUST** always be delimited by the full-form, standard PHP tags:

```php
<?php

?>
```
The closing PHP tag (```?>```) **MUST** be omitted if no markup or code 
follows it.  

Short tags **MUST NOT** be used within Zend Framework library code, but 
**MAY** be used within view scripts.  

### Strings

#### String Literals

When a string is literal (contains no variable substitutions), the apostrophe 
or "single quote" **SHOULD** be used to demarcate the string:  

```php
$a = 'Example String';
```

#### String Literals Containing Apostrophes

When a literal string itself contains apostrophes, you **MAY** demarcate the 
string with quotation marks or "double quotes". This is especially useful for 
SQL statements:  

```php
$sql = "SELECT {{id}}, {{name}} from {{people}} "
     . "WHERE {{name}}='Fred' OR {{name}}='Susan'";
```

This syntax is preferred over escaping apostrophes as it is much easier to 
read.  

#### Variable Substitution

Variable substitution **SHOULD** use either of the folowing forms:  

```php
$greeting = "Hello $name, welcome back!";

$greeting = "Hello {$name}, welcome back!";
```

For consistency, this form **SHOULD NOT** be used:  

```php
$greeting = "Hello ${name}, welcome back!";
```

#### String Concatenation

Strings **MUST** be concatenated using the "." operator. A space **MUST** 
always be added before and after the "." operator to improve readability:  

```php
$company = 'Zend' . ' ' . 'Technologies';
```

When concatenating strings with the "." operator, one **SHOULD** break the 
statement into multiple lines to improve readability. In these cases, each 
successive line **SHOULD** be padded with white space such that the "." 
operator is aligned under the "=" operator:  

```php
$sql = "SELECT {{id}}, {{name}} FROM {{people}} "
     . "WHERE {{name}} = 'Susan' "
     . "ORDER BY {{name}} ASC ";
```

#### Class Names In Strings

When referencing a class name in a string, the fully qualified class name 
**MUST** be provided, and **MUST NOT** include a preceding namespace separator. 
As an example, if in the namespace "Zend\Log", and referencing the class 
"StreamWriter" in the "Writer" subnamespace, you would use the string 
"Zend\Log\Writer\StreamWriter".  

### Arrays

#### Numerically Indexed Arrays

When declaring indexed arrays with the ```array``` function, a trailing space 
**MUST** be added after each comma delimiter to improve readability:  

```php
$sampleArray = array(1, 2, 3, 'Zend', 'Studio');
```

It is permitted to declare multi-line indexed arrays using the ```array``` 
construct. In this case, each successive line **MUST** be padded with spaces 
such that the beginning of each line is aligned with the initial element of 
the array:  

```php
$sampleArray = array(1, 2, 3, 'Zend', 'Studio',
                     $a, $b, $c,
                     56.44, $d, 500);
```

Alternately, the initial array item **MAY** begin on the following line. If 
so, it **MUST** be padded at one indentation level greater than the line 
containing the array declaration, and all successive lines **MUST** have the 
same indentation; the closing paren **MUST** be on a line by itself at the 
same indentation level as the line containing the array declaration:  

```php
$sampleArray = array(
    1, 2, 3, 'Zend', 'Studio',
    $a, $b, $c,
    56.44, $d, 500,
);
```

When using this latter declaration, you **SHOULD** use a trailing comma for 
the last item in the array; this minimizes the impact of adding new items on 
successive lines, and helps to ensure no parse errors occur due to a missing 
comma.  

#### Associative Arrays

When declaring associative arrays with the ```array``` construct, one 
**SHOULD** break the statement into multiple lines. In this case, each 
successive line **MUST** be padded with white space such that both the keys 
and the values are aligned:  

```php
$sampleArray = array('firstKey'  => 'firstValue',
                     'secondKey' => 'secondValue');
```

Alternately, the initial array item **MAY** begin on the following line. If so, 
it **MUST** be padded at one indentation level greater than the line containing 
the array declaration, and all successive lines **MUST** have the same 
indentation; the closing paren **MUST** be on a line by itself at the same 
indentation level as the line containing the array declaration. For 
readability, the various "```=>```" assignment operators **SHOULD** be padded 
such that they align.  

```php
$sampleArray = array(
    'firstKey'  => 'firstValue',
    'secondKey' => 'secondValue',
);
```

When using this latter declaration, one **SHOULD** use a trailing comma for 
the last item in the array; this minimizes the impact of adding new items on 
successive lines, and helps to ensure no parse errors occur due to a missing 
comma.  

### Namespaces

#### Namespace Declaration

Files **SHOULD** contain a single namespace; composing multiple namespaces in 
a single file is strongly discouraged.  

Namespace declarations **MUST** be the first statement in a file, unless 
multiple namespaces are declared; the only code that should precede the 
declaration should be a file-level documentation block, with a single empty 
line between the docblock and the namespace declaration.  

Namespace declarations done according to the above **MUST NOT** have any 
indentation.  

In the case that multiple namespace declarations must be placed in the same 
file, such declarations **MUST** utilize blocks and not single-line 
declarations. The opening brace **MUST** be placed on the following line at 
the same level of indentation, and all code within the block **MUST** receive 
an extra level of indentation. For example, the following is invalid:  

```php
namespace Foo;
 
// some code...
 
namespace Bar;
 
// more code...
```

But the following is correct:  

```php
namespace Foo
{
    // some code ...
}
 
namespace Bar
{
    // more code ...
}
```

#### Import Statements

All explicit dependencies used by a class **MUST** be imported. These include 
classes and interfaces used in method typehints and explicit ```instanceof``` 
type checks, classes directly instantiated, etc. Exceptions include:  

* Code within the current namespace or subnamespaces of the current namespace
* Class names that are dynamically resolved (e.g., from a plugin broker)

There **MUST** be one use keyword per declaration.  

There **MUST** be one blank line after the use block.  

```php
use Zend\Log\Logger;
use Zend\Event\Eventmanager as Events;
use Zend\View\PhpRenderer as View;
```

Additionally, import statements **SHOULD** be in alphabetical order, to make 
scanning for entries predictable.  

### Classes

#### Class Declaration

Classes **MUST** be named according to Zend Framework's naming conventions.  

The brace **MUST** be written on the line underneath the class name, at the 
same level of indentation as the class declaration.  

Every class **MUST** have a documentation block that conforms to the 
PHPDocumentor standard.  

All code in a class **MUST** be indented with four spaces additional to the 
level of indentation of the class declaration.  

PHP files declaring classes **MUST** contain a single PHP class only.  

Placing additional code in class files **MAY** be done, but is discouraged. In 
such files, two blank lines **MUST** separate the class from any additional 
PHP code in the class file.  

The following is an example of an acceptable class declaration:  

```php
/**
 * Documentation Block Here
 */
class SampleClass
{
    // all contents of class
    // must be indented four spaces
}
```

Classes that extend other classes or which implement interfaces **SHOULD** 
declare their dependencies on the same line when possible.  

```php
class SampleClass extends AbstractFoo implements Bar
{
}
```

If as a result of such declarations, the line length exceeds the maximum line 
length, break the line after ```implements``` keywords, and pad those lines by 
one indentation level.  

```php
class SampleClass extends AbstractFoo implements
    Bar
{
}
```

If the class implements multiple interfaces and the declaration exceeds the 
maximum line length, break after each comma separating the interfaces, and 
indent the interface names such that they align.  

```php
class SampleClass implements
    BarInterface,
    BazInterface
{
}
```

#### Class Member Variables

Member variables **MUST** be named according to Zend Framework's variable 
naming conventions.  

Any variables declared in a class **MUST** be listed at the top of the class, 
above the declaration of any methods.  

The var construct **MUST NOT** be used. Member variables **MUST** declare 
their visibility by using one of the ```private```, ```protected```, or 
```public``` visibility modifiers. Giving access to member variables directly 
by declaring them as public **MAY** be done, but is discouraged in favor of 
accessor methods (```set``` & ```get```).  

### Exceptions

Each component **MUST** have a marker "ExceptionInterface" interface.  

Concrete exceptions for a component **MUST** live in an "Exception" 
subnamespace. Concrete exception classes **MUST** implement the component's 
Exception interface, and **SHOULD** extend one of PHP's SPL exception types. 
Concrete exception classes **SHOULD** be named after the SPL exception type 
they extend, but **MAY** have a more descriptive name if warranted.  

Subcomponents **MAY** define their own Exception marker interface and concrete 
implementations, following the same rules as at the root component level. The 
subcomponent exception interface **SHOULD** extend the parent component's 
exception interface.  

As an example:  

```php
/**
 * Component exceptions
 */
// In Zend/Foo/Exception/ExceptionInterface.php:
namespace Zend\Foo\Exception;
 
interface ExceptionInterface
{
}
 
// In Zend/Foo/Exception/RuntimeException.php
namespace Zend\Foo\Exception;
 
class RuntimeException extends \RuntimeException implements ExceptionInterface
{
}
 
// In Zend/Foo/Exception/TripwireException.php
namespace Zend\Foo\Exception;
 
use RuntimeException;
 
class TripwireException extends RuntimeException implements ExceptionInterface
{
}
 
/**
 * Subcomponent exceptions
 */
// In Zend/Foo/Bar/Exception/ExceptionInterface.php
namespace Zend\Foo\Bar\Exception;
 
use Zend\Foo\Exception\ExceptionInterface as FooExceptionInterface;
 
interface ExceptionInterface extends FooExceptionInterface
{
}
 
// Subcomponent concrete exception
// In Zend/Foo/Bar/Exception/DomainException.php
namespace Zend\Foo\Bar\Exception;
 
class DomainException extends \DomainException implements ExceptionInterface
{
}
```

#### Using Exceptions

Concrete exceptions **SHOULD NOT** be imported; instead, either use exception 
classes within your namespace, or import the Exception namespace you will use.  

```php
namespace Zend\Foo;

class Bar
{
    public function trigger()
    {
        // Resolves to Zend\Foo\Exception\RuntimeException
        throw new Exception\RuntimeException();
    }
}

// Explicit importing:

namespace Zend\Foo\Bar;

use Zend\Foo\Exception;

class Baz
{
    public function trigger()
    {
        // Resolves to Zend\Foo\Exception\RuntimeException
        throw new Exception\RuntimeException();
    }
}
```

When throwing an exception, you **SHOULD** provide a useful exception message. 
Such messages **SHOULD** indicate the root cause of an issue, and provide 
meaningful diagnostics. As an example, you may want to include the following 
information:  

* The method throwing the exception (```_METHOD_```)
* Any parameters that were involved in calculations that led to the exception 
(often class names or variable types will be sufficient)

We recommend using ```sprintf()``` to format your exception messages.  

```php
throw new Exception\InvalidArgumentException(sprintf(
    '%s expects a string argument; received "%s"',
    __METHOD__,
    (is_object($param) ? get_class($param) : gettype($param))
));
```

### Functions and Methods

#### Function and Method Declaration

Functions **MUST** be named according to Zend Framework's function naming 
conventions.  

Methods inside classes **MUST** always declare their visibility by using one 
of the ```private```, ```protected```, or ```public``` visibility modifiers.  

As with classes, the brace **MUST** always be written on the line underneath 
the function name. Space **MUST NOT** be inserted between the function name 
and the opening parenthesis for the arguments.  

Functions **SHOULD NOT** be declared in the global scope.  

The following is an example of an acceptable function declaration in a class:  

```php
/**
 * Documentation Block Here
 */
class Foo
{
    /**
     * Documentation Block Here
     */
    public function bar()
    {
        // all contents of function
        // must be indented four spaces
    }
}
```

In cases where the argument list exceeds the maximum line length, you **MAY** 
introduce line breaks. Additional arguments to the function or method **MUST** 
be indented one additional level beyond the function or method declaration. A 
line break **MUST** occur before the closing argument paren, which **MUST** be 
placed on the same line as the opening brace of the function or method with one 
space separating the two, and at the same indentation level as the function or 
method declaration. The following is an example of one such situation:  

```php
/**
 * Documentation Block Here
 */
class Foo
{
    /**
     * Documentation Block Here
     */
    public function bar($arg1, $arg2, $arg3,
        $arg4, $arg5, $arg6
    ) {
        // all contents of function
        // must be indented four spaces
    }
}
```

**Note:** Pass-by-reference is the only parameter passing mechanism permitted 
in a method declaration.  

```php
/**
 * Documentation Block Here
 */
class Foo
{
    /**
     * Documentation Block Here
     */
    public function bar(&$baz)
    {}
}
```

Call-time pass-by-reference **MUST NOT** be used.  

The return value **MUST NOT** be enclosed in parentheses. This can hinder 
readability, in addition to breaking code if a method is later changed to 
return by reference.  

```php
/**
 * Documentation Block Here
 */
class Foo
{
    /**
     * WRONG
     */
    public function bar()
    {
        return($this->bar);
    }

    /**
     * RIGHT
     */
    public function bar()
    {
        return $this->bar;
    }
}
```

#### Closure Definitions

Closure definitions generally follow the same rules as for functions and 
methods:  

* Space **MUST NOT** be inserted between the "function" keyword and the 
opening parenthesis for the arguments.
* A space **MUST** be added between any "use" statement used in declaration 
and the opening parenthesis for its arguments.
* In cases where the argument list exceeds the maximum line length, you **MAY** 
introduce line breaks. Additional arguments to the function or method **MUST** 
be indented one additional level beyond the function or method declaration. A 
line break **MUST** occur before the closing argument paren, which **MUST** be 
placed on the same line as the opening brace of the function or method with 
one space separating the two, and at the same indentation level as the 
function or method declaration.
* Call-time pass-by-reference **MUST NOT** be used.
* The return value **MUST NOT** be enclosed in parentheses. This can hinder 
readability, in addition to breaking code if a method is later changed to 
return by reference.

The primary difference is that closures **MUST** retain the initial brace on 
the same line in which they are defined (not the following line). Code 
**MUST** be indented one additional level.  

```php
/**
 * Opening brace:
 */
$foo = function($x)
{
    // WRONG
};

$foo = function($x) {
    // RIGHT
};

/**
 * Use statement declaration
 */
$foo = function($x) use($y) {
    // WRONG
};

$foo = function($x) use ($y) {
    // RIGHT
};

/**
 * Indentation
 */
$foo = array_map(function($x) {
// WRONG
return strtolower($x);
}, $array);

$foo = array_map(function($x) {
    // RIGHT
    return strtolower($x);
}, $array);
```

#### Function and Method Usage

Function arguments **MUST** be separated by a single trailing space after the 
comma delimiter. The following is an example of an acceptable invocation of a 
function that takes three arguments:  

```php
threeArguments(1, 2, 3);
```

Call-time pass-by-reference **MUST NOT** be used. See the function 
declarations section for the proper way to pass function arguments 
by-reference.  

In passing arrays as arguments to a function, the function call **MAY** 
include the ```array``` declaration and **MAY** be split into multiple lines 
to improve readability. In such cases, the normal guidelines for writing 
arrays still apply:  

```php
threeArguments(array(1, 2, 3), 2, 3);

threeArguments(array(1, 2, 3, 'Zend', 'Studio',
                     $a, $b, $c,
                     56.44, $d, 500), 2, 3);

threeArguments(array(
    1, 2, 3, 'Zend', 'Studio',
    $a, $b, $c,
    56.44, $d, 500
), 2, 3);
```

### Control Statements

#### If/Else/Elseif

Control statements based on the ```if``` and ```elseif``` constructs **MUST** 
have a single space before the opening parenthesis of the conditional and a 
single space after the closing parenthesis.  

Within the conditional statements between the parentheses, operators **MUST** 
be separated by spaces for readability. Inner parentheses **SHOULD** be used 
to improve logical grouping for larger conditional expressions.  

The opening brace **MUST** be written on the same line as the conditional 
statement if the conditional statement does not contain a line feed. The 
closing brace **MUST** be written on its own line. Any content within the 
braces **MUST** be indented using four spaces.  

```php
if ($a != 2) {
    $a = 2;
}
```

If the conditional statement causes the line length to exceed the maximum line 
length and has several clauses, you **MUST** break the conditional into 
multiple lines. In such a case, break the line prior to a logic operator, and 
pad the line such that it aligns under the first character of the conditional 
clause. The closing paren in the conditional will then be placed on a line 
with the opening brace, with one space separating the two, at an indentation 
level equivalent to the opening control statement.  

```php
if (($a == $b)
    && ($b == $c)
    || (Foo::CONST == $d)
) {
    $a = $d;
}
```

The intention of this latter declaration format is to prevent issues when 
adding or removing clauses from the conditional during later revisions.  

For ```if``` statements that include ```elseif``` or ```else```, the 
formatting conventions are similar to the if construct. The following examples 
demonstrate proper formatting for ```if``` statements with ```else``` and/or ```elseif``` constructs:  

```php
if ($a != 2) {
    $a = 2;
} else {
    $a = 7;
}

if ($a != 2) {
    $a = 2;
} elseif ($a == 3) {
    $a = 4;
} else {
    $a = 7;
}

if (($a == $b)
    && ($b == $c)
    || (Foo::CONST == $d)
) {
    $a = $d;
} elseif (($a != $b)
        || ($b != $c)
) {
    $a = $c;
} else {
    $a = $b;
}
```

PHP allows statements to be written without braces in some circumstances. This 
coding standard makes no differentiation; all ```if```, ```elseif``` or 
```else``` statements **MUST** use braces.  

#### Switch

Control statements written with the ```switch``` statement **MUST** have a 
single space before the opening parenthesis of the conditional statement and 
after the closing parenthesis.  

All content within the ```switch``` statement **MUST** be indented using four 
spaces. Content under each case statement **MUST** be indented using an 
additional four spaces.  

```php
switch ($numPeople) {
    case 1:
        break;

    case 2:
        break;

    default:
        break;
}
```

The construct ```default``` **MAY** be omitted from a ```switch``` statement, 
but the code **MUST** contain a comment indicating deliberate omission in such 
cases.  

**Note:** It is sometimes useful to write a ```case``` statement which falls 
through to the next case by not including a ```break``` or ```return``` within 
that case. To distinguish these cases from bugs, any ```case``` statement 
where ```break``` or ```return``` are omitted **SHOULD** contain a comment 
indicating that the ```break``` was intentionally omitted if the case 
statement contains code that would execute. As an example:  

```php
// Makes no sense insert comment after case A and B
switch ($foo) {
    case 'a':
    case 'b':
    case 'c':
        $bar = 'baz';
        break;
}

// Here it makes sense, because it could be seen as bug
switch ($foo) {
    case 'a':
        $baz = 'baz';
        // break intentionally omitted

    case 'b':
    case 'c':
        $bar = 'baz';
        break;
}
```

## Inline Documentation

### Documentation Format

All documentation blocks ("docblocks") **MUST** be compatible with the 
phpDocumentor format. Describing the phpDocumentor format is beyond the scope 
of this document. For more information, visit: 
[http://phpdoc.org/](http://phpdoc.org/)  

All class files **MUST** contain a "file-level" docblock at the top of each 
file and a "class-level" docblock immediately above each class. Examples of 
such docblocks can be found below.  

### General Notes

Classes and interfaces referenced by annotations **MUST** follow the same 
resolution order as PHP. In other words:  

* If the class is in the same namespace, simply refer to the class name 
without the namespace:

```php
namespace Foo\Component;

class Bar
{
    /**
     * Assumes Foo\Component\Baz:
     * @param Baz $baz
     */
    public function doSomething(Baz $baz)
    {
    }
}
```

* If the class is in a subnamespace of the current namespace, refer to it 
relative to the current namespace:

```php
namespace Foo\Component;

class Bar
{
    /**
     * Assumes Foo\Component\Adapter\Baz:
     * @param Adapter\Baz $baz
     */
    public function doSomething(Adapter\Baz $baz)
    {
    }
}
```

* If the class is imported, either via a namespace or explicit class name, use 
the name as specified by the import:

```php
namespace Foo\Component;

use Zend\EventManager\EventManager as Events,
    Zend\Log;

class Bar
{
    /**
     * Assumes Zend\EventManager\EventManager and Zend\Log\Logger:
     * @param Events $events
     * @param Log\Logger $log
     */
    public function doSomething(Events $events, Log\Logger $log)
    {
    }
}
```

* If the class is from another namespace, but not explicitly imported, provide 
a globally resolvable name:

```php
namespace Foo\Component;

class Bar
{
    /**
     * Assumes \Zend\EventManager\EventManager:
     * @param \Zend\EventManager\EventManager $events
     */
    public function doSomething(\Zend\EventManager\EventManager $events)
    {
    }
}
```

**Note:** This last case should rarely happen, primarily since you should be 
importing any dependencies. One case where it may happen, however, is in 
```@return``` annotations, as return types might be determined outside the 
class scope.  

### Files

Every file that contains PHP code **MUST** have a docblock at the top of the 
file that contains these phpDocumentor tags at a minimum:  

```php
/**
 * Zend Framework (http://framework.zend.com/)
 *
 * @link      http://github.com/zendframework/zf2 for the canonical source repository
 * @copyright Copyright (c) 2005-2014 Zend Technologies USA Inc. (http://www.zend.com)
 * @license   http://framework.zend.com/license/new-bsd New BSD License
 */
```

### Classes

Every class **MUST** have a docblock that contains these phpDocumentor tags at 
a minimum:  

```php
/**
 * Short description for class
 *
 * Long description for class (if any)...
 */
```

### Functions

Every function, including object methods, **MUST** have a docblock that 
contains at a minimum:  

* A description of the function
* All of the arguments
* All of the possible return values (even "void")

It is not necessary to use the ```@access``` annotation because the access 
level is already known from the ```public```, ```private```, or 
```protected``` visibility modifier used to declare the function.  

If a function or method may throw an exception, use ```@throws``` for all 
known exception classes:  

```php
@throws exceptionclass [description]
```

