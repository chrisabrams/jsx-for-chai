# JSX for Chai

Fork of [jsx-chai](https://github.com/bkonkle/jsx-chai)

## Installation

Then install *jsx-chai*:

    npm install jsx-chai --save-dev

## Assertions

JSX comparison will kick in on deep equality checks, but normal strict equality
will apply when the 'deep' flag is not used.

```javascript
    expect(<Component/>).to.be.jsx
    expect('Component').to.not.be.jsx
    expect(<Component/>).to.deep.equal(<Component/>)
    expect(<Component prop='value'/>).to.not.deep.equal(<Component prop='other-value'/>)
    expect(<Component/>).to.eql(<Component/>)
    expect(<Component prop='value'/>).to.not.eql(<Component otherProp='value'/>)
    expect(<Component><h1>Title</h1></Component>).to.include(<h1>Title</h1>)
    expect(<Component><h1>Title</h1></Component>).to.not.include(<div/>)
```

Note: `include.keys()` calls will look for normal object properties, and will
not use JSX comparison.

## Usage

Here's an example using [mochajs/mocha](https://github.com/mochajs/mocha).

```js
import chai, {expect} from 'chai'
import jsxChai from 'jsx-chai'
import React from 'react'

chai.use(jsxChai)

class TestComponent extends React.Component {}

describe('jsx-chai', () => {

  it('works', () => {
    expect(<div/>).to.deep.equal(<div/>)
    // ok

    expect(<div a="1" b="2"/>).to.deep.equal(<div/>)
    // Error: Expected '<div\n  a="1"\n  b="2"\n/>' to equal '<div />'

    expect(<span/>).to.not.deep.equal(<div/>)
    // ok

    expect(<div><TestComponent/></div>).to.include(<TestComponent/>)
    // ok
  })

})
```

It looks like this when ran:

![Screenshot when using mocha][screenshot]

## A note about functions

`to.deep.equal` and `to.eql` will not check for function references, it only
checks that if a `function` was expected somewhere, there's also a function in
the actual data.

It's your responsibility to then unit test those functions.
