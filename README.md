# cu
A characteristica universalis: a universal programming language, a superset of all possible programming languages

More and more programming languages are being "invented". And more and more "frameworks". But all these languages and frameworks have two very important things in common.

1. They can all be understood by humans.
2. They can all be interpreted by computers.
 
At some level, every program simply is a single complex idea: an idea composed of other ideas. A program composed of other programs. You can represent an idea in writing, and if you do it in a systematic way, the writing is called code and the process of writing is called encoding. Ideas can be encoded by what needs to be done (declarative), how to accomplish it (imperative), or what conditions imply the thing (logic), and probably other ways.

We have a post-Tower-of-Babel thing going on in computer science right now. Everybody is talking about the same things, but using different languages to do so. Increasingly, we see people approach to solving this problem by creating transpilers - compilers that translate from one programming language to another - but they are rarely bi-directional. The target language is usually x86 assembly, LLVM, C, JVM bytecode, JavaScript. Another trend is the rise of domain specific languages (DSLs), or mini-languages inside languages. jQuery popularized the style of writing(code).like(this). Of particular interest is languages like Markdown and HTML which allow embedding other languages inside them. This has led to interesting hacks like literate CoffeeScript, in which documentation is foremost and code is indented a level. Then there's a converse trend: interpollated strings like "this is a ${var} or even ${'a' + expression()}". Now take a look at this:

```html
  <script>
    var ipsumText = 'Lorem ipsum dolor sit amet, consectetur adipiscing elit. Integer nec odio. Praesent libero.';
    React.render(
      <div>
        <a href="http://www.example.com/" className="button">Button</a>
        <div>{ipsumText}</div>
      </div>,
      document.getElementById('impl')
    );
    
    // Remember that we use className in React instead of class.
    // Because there are braces around `ipsumText`, it is evaluated as a JavaScript expression. Try removing the braces, what happens?
  </script>
```

That, my friends, is an ordinary JSX snippet. It is also:

- HTML (this web page)
  - with embedded Markdown (what I'm typing in)
    - with embedded HTML (the `<script>` tag)
      - with embedded JavaScript (`var ipsumText =... React.render(...`)
        - with embedded pseudo-Latin (`Lorem ipsum dolor...`)
        - with embedded HTML (`<div>...</div>`)
          - with embedded URI (`http://www.example.com`)
          - with embedded JavaScript (`ipsumText`)
        - with embedded English (`Remember that we use... Because there are braces...`)

That is 9 different language changes in 13 LOC. It's the new normal! We don't use one language, we use dozens in the same file.

# The Problem
I actually just ran into it up above. See this snippet?
```
  - with embedded HTML (<div>...</div>)
```

That won't render correctly. The entire word "div" disappears:

- with embedded HTML (<div>...</div>)

This is because &lt; and &gt; need to be escaped like so:

```
 - with embedded HTML (&lt;div&gt;...&lt;/div&gt;)
```

Now at least all the words are visible:

 - with embedded HTML (&lt;div&gt;...&lt;/div&gt;)

But then I decided I wanted "&lt;div&gt;...&lt;/div&gt;" to be monospaced like `<div>...</div>` so I added backtick quotes. 

```
 - with embedded HTML (`&lt;div&gt;...&lt;/div&gt;`)
```

But... that broke the rendering again:

 - with embedded HTML (`&lt;div&gt;...&lt;/div&gt;`)

Because now the less-than and greater-than signs do NOT need to be escaped. So one more fix:

```
 - with embedded HTML (`<div>...</div>`)
```

gives us:

 - with embedded HTML (`<div>...</div>`)

It turns out this quoting problem is nearly universal. If the child language (the one you are quoting) has the same characters as the parent language (the one you are writing in) then how do you prevent the child language from being mistaken for the parent language? It has been solved a number of different ways, all of which are bad:

TODO: fill in this part
- backslash
- multiple quotation marks
- heredocs
- MIME separators

Agh! This is crazy! Can something be done? Is there some way to quote a language that doesn't involve modifying it in any way? Some kind of universal transform that is easy to perform, doesn't harm readability, and is easy to reverse? Why yes, there is...

# Why indentation solves everything
TODO: to be continued
