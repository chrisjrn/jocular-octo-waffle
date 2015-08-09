# Python's New Type Hints in Action … in JavaScript!
Christopher Neugebauer <br />
mail<b>@chrisjrn</b>.com

[Video](https://www.youtube.com/watch?v=_PPQLeimyOM)

# History
## How did we get here?

##### Notes
We begin with a brief history of type hinting in Python. It's been a long time coming.


# 1991-2008
## Python has duck typing. Everyone is happy.
### Quack.

##### Notes
Python started off using Duck Typing. This is generally good.


# 2008
## Python 3 introduces function annotations

```
def haul(item: Haulable, *vargs: PackAnimal) -> Distance:
    ...
    ...
```
Introduced in [PEP 3107](https://www.python.org/dev/peps/pep-3107/)

##### Notes
Function annotations -- you specify argument annotations using a colon inside the argument list, and return annotations using that arrow at the end of the declaration.


## Function Annotations

```
def compile(source: "something compilable",
            filename: "where the compilable thing comes from",
            mode: "is this a single statement or a suite?"):
    pass
```

##### Notes
Function annotations did not need to be type annotations. They could be *anything*.


# 2009-2014
## ???

##### Notes
After introducing annotations in Python 3, there wasn't really anything done with the feature.

Type checking was definitely in the rationale for function annotations, but there was no good way to do it.
The feature sat mostly unused for the next 6 years of Python 3's development.


# 2015: PEP 484
## Type Hints

```
def greeting(name: str) -> str:
    return 'Hello ' + name
```

##### Notes
PEP 484 finally proposes using the annotations in Python to do type checking.

These annotations show a function that takes a string, and returns a string.


## PEP 484
* Type Hints in annotations
* A `typing` module for making type variables
* An external type checker/type linter
* All in existing Python 3 constructs

##### Notes
PEP 484's approach is to use annotations, and a bunch of hint objects defined in an stdlib module.

There's a provided type-checker to provide the actual linting functionality.

All type hinting functionality has been implemented without changing the language.


## The Python feature addition process
* Stage 1: PEP is proposed and discussed
* Stage 2: PEP is adopted
* Stage 3: Feature is added to the next Python release

##### Notes
Python features officially come into existence in a three-stage process.

First there's the PEP process that gets the feature discussed.

Then it gets added to Python.


## The actual process
* Stage 4: Everyone ignores it for 3-5 years
* Stage 5: Raymond Hettinger or David Beazley figures out how to use it properly
* Stage 6: Everyone uses it

##### Notes
Now, what actually happens is the most important step -- where everyone forgets a thing ever got added for the first few years of its existence.

This means that by the time people are using a thing, it's mature, and someone in core-python has figured out how to *properly* use it, and because people are out there using it, nobody really questions it.


## PyCon US 2015, Montréal
<div class="stretch">
![Guido's Type Hinting Plenary](images/guido-keynote.jpg)
</div>

##### Notes
Unfortunately, this time it went slightly wrong. Guido was given a keynote slot at PyCon to discuss the feature this year.


## PEP 484
* Stage 4: Everyone ignores it for 3-5 years

##### Notes
And so this part of the feature release process never happened.


## Despite This…
> It should also be emphasized that Python will remain a dynamically typed language, and the authors have no desire to ever make type hints mandatory, even by convention.

– PEP 484


## … We Still Get this
> … This is much worse than that – it is Python4 hidden away inside a PEP …

–[Jack Diederich](http://lwn.net/Articles/643269/), Grumpy Old Man

##### Notes
General misunderstanding of the reach of the type annotations -- and comparisons with statically typed languages -- have turned discussions about PEP 484 into a tarpit.


# Let's Use PEP 484 Now!

##### Notes
Which brings us to this talk -- I think the best way to get an understanding of Python's type checking proposal is to go out in use it.
Unfortunately, it's not out there yet.

Let's take a look at it in a language you hopefully already know.



# TypeScript
## It's Just JavaScript With Types™


## TypeScript
* Microsoft
* Open Source (Apache licence)
* Available since 2012


## JavaScript
```javascript
console.log("Hello, World!");
```

##### Notes
This is hello world in JavaScript -- simple.


## TypeScript
```typescript
console.log("Hello, World!");
```

##### Notes
This is hello world in TypeScript -- spot the difference.


## Problem?
<div class="stretch">
![Troll Face](images/trollface.jpg)
</div>

##### Notes
So in its simplest form, TypeScript is *just* JavaScript. You need to start writing more complicated code to see the difference.


## JavaScript
```javascript
function hello(name) {
    return "Hello, " + name + "!";
}

console.log(hello("PyCon AU"));
```

##### Notes
A JavaScript function that constructs a string from a string parameter.


## TypeScript
```typescript
function hello(name : string) : string {
    return "Hello, " + name + "!";
}

console.log(hello("PyCon AU"));
```

##### Notes
This shows TypeScript -- we have a type hint for the input function and a type hint for the output.

Calling `hello` with a non-string parameter will break. Likewise, assigning the output to anything that isn't expecting a string will also fail.


## JavaScript from the `tsc` Compiler
```typescript
function hello(name) {
    return "Hello, " + name + "!";
}

console.log(hello("PyCon AU"));
```

##### Notes
Type erasure! If things compile OK, you'll end up with just the same functions without the type annotations. Under the hood, it's just JavaScript. It runs the same way as your JavaScript would have.


## Type Inference
```typescript
function hello(name : string) {
  console.log("Hello, " + name);
}

var a : string = "PyCon AU";
var b = a.toUpperCase();

hello(b); // Valid TypeScript
```

##### Notes
Because `a` is a string, and `toUpperCase` returns a string, we can provide `b` to hello, and the type checker knows it's a string.

This is Type Inference. It means you don't need to manually hint every function and every variable you have. Sometimes the compiler can figure it out for you.


## Type Inference

```typescript
function hello(name : string) {
  console.log("Hello, " + name);
}

var c : number = 42;
var d = Math.cos(42);

hello(d); // Invalid TypeScript
```

##### Notes
This is invalid TypeScript because Math.cos returns a number -- even though we didn't annotate the type of `d` -- the compiler knows that d will be a number.


## Consume a type-checked API
### get free type checking on your own code

##### Notes
If you write code that only interfaces with type-checked functions, then you get type checking for free.


## Type Inference
```typescript
function forty_two() {
  return 42;
}

var d = Math.cos(forty_two()); // This is valid
```

##### Notes
If every return point in a function is of a given type, then you get type checking for free on that function. `forty_two()` clearly only returns a number, so type inference can pick that up.


## Type Inference
```typescript
function a_string() {
  return "hello!";
}

var d = Math.cos(a_string()); // This is not valid
```

##### Notes
Here's a function that can only return a string - it's an invalid argument to `cos`.


## If the only thing you do is consume a typescript API,
### there's still benefit to type checking.

##### Notes
Thanks to type inference, even if you don't care to annotate your own functions, rather just consume a type-hinted API, you will benefit from testing your own code against the third party's constraints.



# Buying Into TypeScript

##### Notes
So type checking is excellent! But how is it useful if not everyone else making JavaScript code isn't using TypeScript?
Fundamental things like JQuery don't use TypeScript, so doesn't that mean I can't use TypeScript if I want JQuery?


## Gradual Typing
### It's Not Like Java!™

##### Notes
The answer comes from a technique called *Gradual Typing*. Gradual Typing refers to type systems that do not require every function, every variable to participate in the type system.

Gradual Typing means only some code needs to participate in the type system.


## Opt-In
No type checking until you declare a type explicitly:

```typescript
var spam : string = "Bacon";
```

##### Notes
There are two ways to get in on type checking. The first is to explicitly declare a type.

In this code snippet, the variable `spam` is being explicitly declared to be a `string`.


## Opt-In
or until the compiler can figure it out:

```typescript
var spam = Math.cos(42): // spam is a number
```

##### Notes
The other is to ignore type hinting and see if the compiler can make a guess -- assigning a number to an unhinted variable means that the variable is inferred to be a number.


## Opt-Out
If you don't want type checking, you don't have to have it:

```typescript
var bacon = "42";
Math.cos(bacon); // invalid
```

##### Notes
JavaScript has type coercion behaviour that you may want sometimes. If you want to opt out of the type system, you can!


## Opt-Out
To opt out, use `any`!

```typescript
var bacon : any = "42";
Math.cos(bacon); // valid!
```

##### Notes
Declaring an variable to be an `any` means it does not participate in the type system. It could be any type of value. You can use it anywhere.


# Everything is `any`
## until TypeScript knows better

##### Notes
`any` is the thing that makes TypeScript's type system actually work.

Everything is assumed to be `any` -- that is, there are no type constraints -- until the compiler is certain that type constrants can apply.


# No type constraints until you're confident the code meets them.

##### Notes
This is the big difference between Gradual typing and static typing. You don't need to introduce any type constraints until you are ready to meet them. They're not compulsory, and they don't take effect until they're ready.


## `any` can cast to anything
```typescript
var spam : any = "42";

var eggs : number = spam;
```

##### Notes
The most important aspect of `any`, though, is that you can cast from `any` to *any* specific type.


# Type constraints can be broken
## if you're confident they need to be.

##### Notes
Gradual typing also means that you can break the constraints of the type system if you need it. This means that TypeScript can keep the flavour of JavaScript's weak typing in cases where it's needed.



# The Power of `any`

##### Notes
This means that `any` is actually the most powerful, and liberating aspect of TypeScript. Let's have a look at why:


## Interface Mangling
```typescript
interface JakeWheery {

  attr(attributeName : string) : string;

  children(selector? : string) : JakeWheery;

}
```

##### Notes
Say you have a definition that defines the definition of some interface. Let's call it `JakeWheery` (no reason).


## Interface Mangling

```typescript
var $ : JakeWheery = JQuery();
```

##### Notes
Once you get an object from plain-old JavaScript, you can cast it to meet a typescript interface you've defined. From here on in, every call to JQuery will be type checked against the interface we defined.

JQuery's authors didn't need to define the spec. We've done that, and the type system is fully happy. Of course the onus is on us to make sure that the interface is accurate and doesn't get in our way.


## Interface Mangling
```typescript
/// <reference path="jquery.d.ts" />
```

##### Notes
Taking advantage of such an interface in your code is easy. You just point out a reference path at the top of your file. Very similar to doing an import in Python, or an include in C/C++.


## Third Party Interfaces
http://github.com/borisyankov/DefinitelyTyped

##### Notes
There are entire communities making repositories of TypeScript interfaces for popular JS libraries. DefinitelyTyped has several hundred interfaces, for basically every important JS library (Node, JQuery, etc)


# You can use TypeScript right now…
## …and it's useful for real, non-trivial code.

##### Notes
Thanks to being able to provide interface declarations, *any* code written in vanilla JavaScript can take advantage of TypeScript's type hinting features. You just need someone to write the declaration.

This means that basically any JavaScript library you care to use already has type declarations.


# Typing in Python
## (How do I get me some of those type hints?)

##### Notes
So how does this apply to Python?

PEP 484


## PEP 484
### Implemented in Python 3.5b1
* Type hints in function annotations
* Implemented using Python-The-Language features
* An external type checking program ("linter")

##### Notes
PEP 484 proposes using function annotations as a way to express type hints.

All of the type hinting features are implemented *within the existing language*. The only concrete addition is a file that defines type hinting primitives and a type checker.

(This is different to TypeScript, which is a language extension and requires preprocessing before you can run it in a browser.)


## PEP 484
### Gradual Typing, just like TypeScript
* Py3 code will still work as Py3 code
* Code with annotations will still work
* Code will benefit from type annotations as type annotations are added

##### Notes
PEP 484 is a relatively non-invasive way to get access to gradual typing in Python.


## External Interfaces
### `spam.py`
```
def spam(some_food):
  return "You ordered spam and " + some_food + "!""
```
### `spam.pyi`
```
def spam(some_food : str) -> str:
  pass
```

##### Notes
If you don't want to "pollute" your main body of code with type hints, you don't have to. You can put type hints in a separate `.pyi` file with a structure that matches your `.py` file.

This makes your code able to work on Py2 (no annotations). It also mimics a test structure for your code -- testable information (the type hints) go into a separate file.

This is basically the same as having C headers, or TypeScript `ts.d` files.


## Type Checking
### Done at lint time… for now.

##### Notes
Once you run your code in Python, the type hints are effectively ignored. This is basically type erasure. There's no effect on how your code will actually run.

Doing type checking at runtime is not something you'd want to do in prod. That said, actually instrumenting your code to validate type constraints at runtime may be a very effective way to debug your code. I would expect to see an experimental runtime type checker spring into existence one of these days.

It won't be compulsory. That would be stupid.



# Benefits

##### Notes
Why do you care?


## Type resilience tests emerge naturally
### (Fewer tests for better coverage!)

##### Notes
If you unit test for correct behaviour when passing around silly types into your functions, type hinting's an easy way to get *comprehensive* versions of those tests for minimal effort.


## Type checking is another linting stage

##### Notes
Even if you don't hint your own functions, checking against other hinted functions is a linting sanity check.


## Verifiable parameter/return type documentation
### Like doctests, but formal!

##### Notes
You no longer need to write separate documentation about what params your functions take, or what they return. You can auto-generate documentation. You can test that documentation and that it's up-to-date.

If that documentation is *wrong*, your tests will fail.


## Your IDE will work better.

##### Notes
Auto Complete -- 50-60% of statements correctly inferred only. Eew.


# On Duck Typing
<div class="stretch">
![Duck Typing](images/duck-typing.jpg)
</div>

##### Notes
Python is famous for duck typing. Being able to rely on individual parts of an interface rather than expecting an object of a specific type is what makes Python so very flexible.

Writing in a duck-typed language means that you can write code that feels completely natural, but is *very* difficult to express formally using a type system.


## `requests` probably won't ever have type annotations
> Requests relies _heavily_ on duck-typing … it will be _very_ difficult for us to implement a maintainable and accurate set of stub files for the module.

–[Ian Cordasco](http://www.coglib.com/~icordasc/blog/2015/04/type-hints-in-python-35.html), `requests` Core Developer

##### Notes
`requests` relies heavily on duck-typing to provide flexibility...


## Sometimes it doesn't make sense
<small><pre>
Optional[
    Union[
        Mapping[
            basestring,
            Union[
                Tuple[basestring, Optional[Union[basestring, file]]],
                Tuple[basestring, Optional[Union[basestring, file]],
Optional[basestring]],
                Tuple[basestring, Optional[Union[basestring, file]],
Optional[basestring], Optional[Headers]]
            ]
        ],
        Iterable[
            Tuple[
                basestring,
                Union[
                    Tuple[basestring, Optional[Union[basestring, file]]],
                    Tuple[basestring, Optional[Union[basestring,
file]], Optional[basestring]],
                    Tuple[basestring, Optional[Union[basestring,
file]], Optional[basestring], Optional[Headers]]
            ]
        ]
    ]
]
Headers = Union[
    Mapping[basestring, basestring],
    Iterable[Tuple[basestring, basestring]],
]
</pre></small>

-[Cory Benfield](http://lwn.net/Articles/643399/), `requests` Core Developer

##### Notes
The `files` parameter in `requests` lets you specify filenames or content blobs or actual file-like handles. As well as HTTP headers. It may let you provide a single item, or a sequence of items. Or a dictionary of them. It gives you all of the options so that you can construct data to fit your own use case.

This is the shortest possible annotation for the `files` parameter to `requests.post`. Eew.

This gives no clarity. It is *much* easier to write prose to describe this than it is to write a type hint.


## So how do you do type assertions?

##### Notes
Projects like `requests` will be full of type hints like that one.


# You don't!

##### Notes
End-users *do not need* `requests` to be fully type-hinted in order to take advantage of `requests`. There is *excellent* documentation that tells you how to use it, and the behaviour of requests is opaque enough to such type hints that they'd never actually be helpful.

`any` is about as useful an assertion as the `files` annotation.


## You probably don't need that flexibility!
```
def call_json_api(url : str) -> dict:
    response = requests.get(url)
    data = response.json()
    return data
```
`data` could be *any* JSON type. We assert that it's a `dict`.

##### Notes
Whilst requests makes no guarantees about what comes out of it, *we* know the behaviour of the things that we're calling -- gradual typing means that we can make assertions about libraries with weaker type constraints.

This type hinting makes it easy to make stronger assertions when we, the programmer, know that they apply.


## Make type hints that are useful to your own context.

##### Notes
Type hints aren't about making everything super specific and verifiable like Java -- that's just not how Python works, and using type hints like Java will make your Python code worse.

Use hints where they help your code, and you'll enjoy them.


# Conclusion
* It's not the end of the world
* You don't have to if you don't want to
* There are benefits


# Try Out TypeScript!
[typescriptlang.org](typescriptlang.org)


# The End
## Python's New Type Hints In Action … In JavaScript!
Christopher Neugebauer <br />
mail<b>@chrisjrn</b>.com
