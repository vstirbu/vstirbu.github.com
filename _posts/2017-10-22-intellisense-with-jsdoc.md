---
layout: post
title: "When JSDoc gives you IntelliSense powers"
tags: intellisense typescript jsdoc javascript
---

During the past week I stumbled upon a blog post by [Ryan Paul](http://seg.phault.net/blog/2017/10/typescript-without-transpiling/) on getting the advantages of [TypeScript](https://www.typescriptlang.org) without transpiling. The _magic_ is made possible by TypeScript 2.3, which introduced support for [standard JavaScript](https://github.com/Microsoft/TypeScript/wiki/Type-Checking-JavaScript-Files). The type information contained in [JSDoc-like](https://github.com/Microsoft/TypeScript/wiki/JSDoc-support-in-JavaScript) annotations is analysed and used in a similar fashion as TypeScript's own type system.

Too good to be true? You bet!

# So, time to conduct my own experiment...

I'm maintaining [fsm-as-promised](https://github.com/vstirbu/fsm-as-promised), a minimalistic finite state machine library using ES6 promises. The library has a small API surface, making it ideal for conducting this experiment.

{% highlight javascript %}
const StateMachine =  require('fsm-as-promised');

const fsm = StateMachine({
  // configuration object
});
{% endhighlight %}

As seen in the above example, creating a new state machine object is as simple as invoking the `StateMachine` function. The not so nice part is the configuration object, which actually describes the state machine using a DSL-like object literal.

{% highlight javascript %}
const configuration = {
  initial: 'initial',
  final: 'final',
  events: [
    {
      name: 'go',
      from: 'initial',
      to: 'final'
    }
  ],
  callbacks: {
    ongo: function () {}
  }
}
{% endhighlight %}

All properties of the configuration object are straightforward to document using JSDoc. One notable exception is `callbacks`, which is an object literal for which all properties are of type `Function`. What makes it different is that its properties are not known in advance.

A bit of try and error cycles, going through multiple stackoverflow questions, ended up with this result, which tells that the object is a map where the key is a `String` and the value is `Function`:

{% highlight javascript %}
/**
 * @prop {Object<String, Function>} callbacks - Callbacks
 */
{% endhighlight %}

Now that everything is [set](https://github.com/vstirbu/fsm-as-promised/blob/9197ee7eb2ff695d6014719ca21dfdfaed40149f/lib/index.js#L452-L470), lets see the results...

# IntelliSense in action

[Visual Studio Code](https://code.visualstudio.com) or VSCode is a code editor with the super powers that we need:

> Go beyond syntax highlighting and autocomplete with IntelliSense, which provides smart completions based on variable types, function definitions, and imported modules.

VSCode supports several ways to [enable](https://github.com/Microsoft/vscode/issues/25322) TypeScript checking for JavaScript files. To keep things simple, I've added `//@ts-check` to the top of the file where the state machine is defined.

Now VSCode is ready to unleash all the power of IntelliSense, among which are...

{% include figure.html src="/assets/20171020/autocompletion.gif" caption="autocompletion" %}

{% include figure.html src="/assets/20171020/type-information.gif" caption="or type mismatch information when the property should be a Function instead of Number" %}

The IntelliSense features are available despite both the source file and the library being plain JavaScript. Everything is enabled by VSCode which runs Typescript behind the scene.

# What about tests

Now that the library's JSDoc type declarations are accessible to IntelliSense, the next question is: _How to be sure that they do not drift from the actual implementation?_

The basic approach is to have a set of example files, using the library, with good and wrong types and validate them using the TypeScript compiler. At the time of writing, compiler cli does not provide an option to [validate typings](https://github.com/Microsoft/TypeScript/issues/7661) in JavaScript files. Luckily, the compiler can be used programmatically, and there is good [API documentation](https://github.com/Microsoft/TypeScript/wiki/Using-the-Compiler-API).

The primary concern, as part of an experiment, is to determine the feasibility of testing the typings rather than completeness of the test suite. Therefore, I've proceeded by testing the happy path, having a state machine definition that covers all combinations of using the types correctly. In this instance, the file is parsed by the compiler and no errors or warnings should be encountered.

The compiler must be instructed what the file to be analysed is JavaScript, written according to EcmaScript 5 standard, is using CommonJS module style, and the runtime environment is node. The result is the following:

{% highlight javascript %}
describe('Intellisense', function () {
  it('should not give errors', function () {
    this.timeout(5000);

    const options = {
      allowJs: true,
      noEmit: true,
      target: ts.ScriptTarget.ES5,
      module: ts.ModuleKind.CommonJS,
      moduleResolution: ts.ModuleResolutionKind.NodeJs
    };
    const program = ts.createProgram(['/path/to/your/fsm-file.js'], options);
    const results = program.emit();

    const diagnostics = ts.getPreEmitDiagnostics(program).concat(results.diagnostics);

    expect(diagnostics).to.have.length(0);
  });
});
{% endhighlight %}

This can be easily expanded to test for errors if wrong types are used. You just need to feed JavaScript files with errors and expect the compiler to find them.

# Conclusions

I've started this experiment with the simple goal of checking if the IntelliSense power can be derived from plain JavaScript files with JSDoc comments. The experiment's result prove that not only it is possible but you can easily build the necessary tooling to include the type checking in the test suite. As a library maintainer, this is very important as I can provide IntelliSense support, a feature that increases the developer experience of the library's users. More, IntelliSense being a feature of the code editor, the whole development process is light weight as I don't have to rely on TypeScript as development dependency.
