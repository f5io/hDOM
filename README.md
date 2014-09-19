#hDOM

##A lightweight JavaScript library for DOM manipulation.

**Author:** *Joe Harlow* (<joe@f5.io>)

---
`hDOM` is a subset of `jQuery` for modern browsers. It was developed as a internal tool for [Holler London](http://www.holler.co.uk) and includes extensions often used from project to project.

###Browser Support
---
For full capabilities, the following browsers are supported:

- Microsoft Internet Explorer 8+
- Mozilla Firefox 21.0+
- Google Chrome 27.0+
- Apple Safari 5.1+
- Opera 15.0+

###Installation
---

`hDOM` can be installed from `npm` using:

`npm install hdom`

###Usage
---

`hDOM` can be loaded using `CommonJS`-style module definitions using `browserify`, if the package has been installed via `npm`.

    var ƒ = require('hdom');

If loaded via a `<script>` tag, `hDOM` can be accessed using either `hDOM` or `ƒ` on the `window` object.

###DOM Ready
---

By passing a `Function` directly into `hDOM` you can set up a callback for the `DOMContentLoaded` event.

_Example_:
    
        hDOM(function() {
        	console.log('DOM Ready');
        });


###Selection Engine
---

`hDOM` provides a selection engine built directly on `querySelectorAll`, therefore most modern CSS selectors are supported.

After using a CSS selector, `hDOM` returns an array-like `ElementCollection` which is iterable via a 0-based index.

####`hDOM`(`selector /* String */`) OR `hDOM`(`element /* HTMLElement */`)

The returned `ElementCollection` has the following methods, which are chainable:

- ####`each`(`fn /* Function */`)
    Used to iterate over an `ElementCollection`, the supplied `Function` receives the raw `HTMLElement` as the first argument and the `index` within the collection as the second. Each element will also be the scope (`this`) of the supplied `Function`.
    
    _Example_:
    
        hDOM('div').each(function(el, index) {
        	console.log(el, index);
        });

- ####`get`(`index /* int */`)
	Returns element at index within the `ElementCollection` wrapped as it's own `ElementCollection`. Supplying an index of `-1` will return the last element.

    _Example_:
    
        var ƒDiv = hDOM('div');
        ƒDiv.get(0).addClass('first');
        ƒDiv.get(-1).addClass('last');

    
- ####`find`(`selector /* String */`)
    Search for a `selector` within the current `ElementCollection`. This method returns a new `ElementCollection` with any elements which match the `selector`.
    
    _Example_:
    
        hDOM('div').find('p');

- ####`bind`(`event /* String */`, `fn /* Function */`)
    Bind an `event` to each element within the current `ElementCollection`. The `Function` will have the element as it's scope (`this`) as well as receive the `Event` object as its only argument.
    
    _Example_:
    
        hDOM('div').bind('mouseout', function(event) {
        	console.log(this); // Element that the event was bound to.
        });
        
- ####`unbind`(`event /* String */`, `fn /* Function */`)
	Unbind an `event` from each element within the current `ElementCollection`.
	
	_Example_:
	    
	    function evHandler(event) {
	    	console.log(event);
	    	hDOM(this).unbind('click', evHandler);
	    }
	    
	    hDOM('div').bind('click', evHandler);
	    
- ####`addClass`(`class /* String */`)
    Adds a class name to all elements within the current `ElementCollection`. Add multiple classes by using a space-separated string.
    
    _Example_:
    
        hDOM('div').addClass('block block--element block--element__modifier');
        
- ####`removeClass`(`class /* String */`)
    Remove a class name from all elements within the current `ElementCollection`. Remove multiple classes by using a space-seperated string.
    
    _Example_:
    
        hDOM('div').removeClass('block--element__modifier');
        
- ####`hasClass`(`class /* String */`)
	Returns a boolean as to whether the first element in the current `ElementCollection` contains the supplied class name.
	
	_Example_:
	
	    var ƒdiv = hDOM('div');
	    console.log(ƒdiv.hasClass('block'));
	    
- ####`toggleClass`(`class /* String */`)
	A convenience method to toggle (add or remove) a class name from all elements within the current `ElementCollection`.
	
	_Example_:
	
	    hDOM('div').toggleClass('block--element');
	    
- ####`attr`(`name /* String */`[, `value /* String */`])
    Can be used to either `get` or `set` an attribute value on all elements within the current `ElementCollection`. If you are `set`ting a value, this function will return the `ElementCollection` for continued chaining. If you do not provide a `value` to set, this function will return the `value` of attribute specified.
    
    _Example_:
    
        hDOM('div').attr('data-foo', 'bar');
        console.log(hDOM('div').attr('data-foo'));
        
- ####`offset`()
	This function will return the `BoundingClientRect` of the first element within the `ElementCollection`.
	
	_Example_:
	
	    hDOM('div').offset();
	    
- ####`clone`([`deep /* Boolean */`])
    Used to `clone` all elements within an `ElementCollection` and return a new `ElementCollection`.
    
    _Example_:
    
    	var ƒclone = hDOM('.clone').clone(true);

- ####`append`(`...args /* HTMLElement */`)
	Append each supplied `HTMLElement` to the end of each element in the current `ElementCollection`.
	
	_Example_:

	    var div = document.createElement('div');
	    div.className = 'block--element__modifier';
	    div.innerText = 'Hello World!';
	    
	    hDOM('p').append(div);
	    
	    /*
	     * Multiple appends example
	     */
	     
	    var ƒp = hDOM('p');
	    
	    var elems = [];
	    for (var i = 0, len = 10; i < len; i++) {
	    	var div = document.createElement('div');
	    	div.className = 'element__' + (i + 1);
	    	elems.push(div);
	    }
	    
	    ƒp.append.apply(ƒp, elems);


- ####`prepend`(`...args /* HTMLElement */`)
    Prepend each supplied `HTMLElement` to the beginning of each element in the current `ElementCollection`.
    
    _Example_:
    
        var div = document.createElement('div');
	    div.className = 'block--element__modifier';
	    div.innerText = 'Hello World!';
	    
	    hDOM('p').prepend(div);


- ####`remove`()
	Remove all elements within the current `ElementCollection` from the `DOM`.
	
	_Example_:
	
	    hDOM('p.footnote').remove();

- ####`html`([`value /* String */`])
	If a `value` is supplied, set all elements `innerHTML` property to `value`. Will return the `innerHTML` of the first element in the `ElementCollection` if no `value` is supplied.
	
	_Example_:
	
		hDOM('p').html('Hello World!');
		console.log(hDOM('p').html());
	
- ####`width`([`value /* Number */`])
	If the `ElementCollection` contains the `window` object, returns the width of the Window.
	If the `ElementCollection` contains the `document` object, returns the width of the Document.
	Otherwise, uses the supplied `value` to set the style `width` of each element in pixels. If no `value` is supplied, returns `BoundingClientRect` width of the first element.
	
	_Example_:
	
	    /*
	     * Get the window width
	     */
	    console.log(hDOM(window).width());
	    
	    /*
	     * Get the document width
	     */
	    console.log(hDOM(document).width());
	    
	    /*
	     * Normal usage
	     */
	    hDOM('p').width(125);
	    console.log(hDOM('p').width());


- ####`height`([`value /* Number */`])
    If the `ElementCollection` contains the `window` object, returns the height of the Window.
	If the `ElementCollection` contains the `document` object, returns the height of the Document.
	Otherwise, uses the supplied `value` to set the style `height` of each element in pixels. If no `value` is supplied, returns `BoundingClientRect` height of the first element.
	
	_Example_:
	
	    hDOM('p').height(125);
	    console.log(hDOM('p').height());


- ####`index`()
    Return the `index` of an element relative to it's parent. Useful in finding the `index` of a list item within it's parent.
    
    _Example_:
    
    	console.log(hDOM('li.selected').index());

- ####`click`()
	Trigger a `click` event on each element within the `ElementCollection`.
	
	_Example_:
	
	    hDOM('button.submit').click();


###Custom Events
---

`hDOM` supplies the following custom events which can be bound to:

- `tap`/`fastclick`
- `swipeleft`
- `swiperight`
- `swipeup`
- `swipedown`

_For Example_:

    hDOM('div.slider').bind('swipeleft', move('right'));
    hDOM('div.slider').bind('swiperight', move('left'));
    
###Other Utilities
---

`hDOM` has various built-in utilities for ease-of-use:

- ####`hDOM`.`ready`(`fn /* Function */`)
    Add `Function` to `DOMContentLoaded` listener to be called when the `DOM` is ready.
    
    _Example_:
    
        hDOM.ready(function() {
        	console.log('DOM Ready');
        });

- ####`hDOM`.`each`(`obj /* Object */`, `fn /* Function */`[, `context`])
    Iterate over each property in `obj` and call `fn`. If `context` is supplied, this is used as the scope of the supplied `Function`. The supplied `Function` is called with the `item` as the first argument and the `key` as the second.
    
    _Example_:
    
        var obj = {
        	one: '1-one',
        	two: '2-two',
        	three: '3-three'
        };
        
        hDOM.each(obj, function(item, key) {
        	console.log(item, key); // '1-one', 'one'
        });
    
- ####`hDOM`.`extend`(`obj /* Object */`, `...args /* Object */`)
	Extend each supplied object into the first argument and return.
	
	_Example_:

	    var obj1 = {
	    	one: 1,
	    	two: 2
	    };
	    
	    var obj2 = {
	    	two: 3,
	    	three: 3
	    };
	    
	    var obj3 = {
	    	three: 4,
	    	four: 4
	    };
	    
	    var obj4 = hDOM.extend(obj1, obj2, obj3);
	    
	    console.log(obj4);
	    /*
	     * Output
	     */
	    {
	    	one: 1,
	    	two: 3,
	    	three: 4,
	    	four: 4
	    }

- ####`hDOM`.`format`(`str /* String */`, `dict /* Object */`)
    Used for formatting a string with dynamic content. This is essentially a very simple templating engine. Using curly-braces (`{`,`}`), we can interpolate values from the `dict` object using their key.
    
    _Example_:
    
    	var str = 'Hello {name}! You currently have {num} message(s).';
    	var dict = {
    		name: 'Simon',
    		num: 12
    	}
    	var output = hDOM.format(str, dict);
    	console.log(output); // Hello Simon! You currently have 12 message(s).
    	
- ####`hDOM`.`range`(`min /* int */`, `max /* int */`)
	Returns a random integer between min (inclusive) and max (inclusive).
	
	_Example_:
	
		var rnd = hDOM.range(0, 10);
		
- ####`hDOM`.`ajax`(`settings /* Object */`)
	Make a `XMLHttpRequest` using the `settings` object. The `settings` object can have the following options:
	
	- `settings.url` - URL to request
	- `settings.dataType` - Expected response type, default: `text`
	- `settings.data` - Data to send as either query string or `FormData`
	- `settings.cache` - Cache response, default: `true`
	- `settings.success` - Callback for a successful request
	- `settings.error` - Callback for an error on the request
	- `settings.method` - `GET` or `POST`, default: `GET`
	- `settings.async` - Run the request asynchronously, default: `true`
	
	
   _Example_:
	
		hDOM.ajax({
			url: 'http://ajaxrequest.com/json',
			dataType: 'json',
			success: function(response) {
				console.log(response) // Response is JSON object
			},
			error: function(response) {
				console.log(response) // Error message
			}
		});
		
	**Note**: hDOM does not currently support `JSONP`.

- ####`hDOM`.`scrollTop`([`value /* Number */`])
    This function will return the current scroll position of the page, or if a `value` is given, will scroll to that value.
    
    _Example_:
    
        hDOM.scrollTop(100); // scroll to 100px


- ####`hDOM`.`emitter`
	`hDOM` includes a small and simple `Publish/Subscribe` implementation built-in. It allows for custom event handlers and emission between modules.
	
	- ####`hDOM`.`emitter`.`on`(`event /* String */`, `handler /* Function */`)
	On `event`, call the `handler`, any arguments passed via the `emit` method become the arguments passed to the `handler`.
	
	
	- ####`hDOM`.`emitter`.`emit`(`event /* String */`[, `...args`])
	Emit `event`, effectively calling all `handlers` of the `event` name with the supplied arguments.
	
	_Example_:
	
	    hDOM.emitter.on('an_event', function(arg1, arg2, arg3) {
	    	console.log(arg1, arg2, arg3); // { hello: 'world' }, 100, true
	    });
	    
	    hDOM.emitter.emit('an_event', { hello: 'world' }, 100, true);
       
###License
---

Copyright (C) 2014 Joe Harlow (Fourth of 5 Limited)

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.


