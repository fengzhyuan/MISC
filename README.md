# MISC

bugs
----------
* Some components in Kendo UI, such as `DropDownList`, `ComboBox` won't trigger filtering event with CN/JAPAN or other IME;  
Based on [this](http://www.telerik.com/forums/chinese-input-method-bug-in-firefox) post, Kendo UI listens to particular HTML *input* events such as `keydown`, `keypress`, but somehow FF failed to capture those events;  
One temp solution is to manually trigger `keydown` on `input` event (usin `JQuery`);first we have to find those *input* elements based on *id* or other selectors, then add some code like this:  
```javascript
my_input.bind('input', function(e) {
    $(this).focus();
    var value = $(this).val(),
        ch = value.substring(value.length-1, length).charCodeAt(0); // 
    if (ch > 200) { // or other values, see ASCII Table code (http://www.asciitable.com/)
        var e = $.Event('keydown');
        e.which = 32; // space; or other key; see https://css-tricks.com/snippets/javascript/javascript-keycodes/
        $(this).trigger(e);
    }
});
```
