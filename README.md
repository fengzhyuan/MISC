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
* After modify `selectedIndex` of a `dropdown` type element, jquery's `clone` method won't copy the modified value to the new element  
https://github.com/spencertipping/jquery.fix.clone  
```javascript
(function (original) {
  jQuery.fn.clone = function () {
    var result           = original.apply(this, arguments),
        my_textareas     = this.find('textarea').add(this.filter('textarea')),
        result_textareas = result.find('textarea').add(result.filter('textarea')),
        my_selects       = this.find('select').add(this.filter('select')),
        result_selects   = result.find('select').add(result.filter('select'));

    for (var i = 0, l = my_textareas.length; i < l; ++i) $(result_textareas[i]).val($(my_textareas[i]).val());
    for (var i = 0, l = my_selects.length;   i < l; ++i) {
      for (var j = 0, m = my_selects[i].options.length; j < m; ++j) {
        if (my_selects[i].options[j].selected === true) {
          result_selects[i].options[j].selected = true;
        }
      }
    }
    return result;
  };
}) (jQuery.fn.clone);
```
