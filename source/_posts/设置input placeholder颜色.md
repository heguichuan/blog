---
title: 设置input placeholder颜色
date: 2018-5-12 14:36:35
---

参考链接：[使用CSS修改HTML5 input placeholder颜色](https://segmentfault.com/q/1010000000397925)

*_css方式_*

WebKit和Blink（Safari,Google Chrome, Opera15+）使用伪元素：

`::-webkit-input-placeholder`

Mozilla Firefox 4-18使用伪类

`:-moz-placeholder`

Mozilla Firefox 19+ 使用伪元素

`::-moz-placeholder`

IE10使用伪类

`:-ms-input-placeholder`

例子：

```css
input::-webkit-input-placeholder, textarea::-webkit-input-placeholder {
    color:    #666;
}
input:-moz-placeholder, textarea:-moz-placeholder {
    color:    #666;
}
input::-moz-placeholder, textarea::-moz-placeholder {
    color:    #666;
}
input:-ms-input-placeholder, textarea:-ms-input-placeholder {
    color:    #666;
}
```

*_js方式_*

```javascript
    setPlaceholderColor: function(){
        /* jqury版
        $('[placeholder]').focus(function() {
            var input = $(this);
            if (input.val() == input.attr('placeholder')) {
                input.val('');
                input.removeClass('placeholder');
            }
        }).blur(function() {
            var input = $(this);
            if (input.val() == '' || input.val() == input.attr('placeholder')) {
                input.addClass('placeholder');
                input.val(input.attr('placeholder'));
            }
        }).blur();
        $('[placeholder]').parents('form').submit(function() {
            $(this).find('[placeholder]').each(function() {
                var input = $(this);
                if (input.val() == input.attr('placeholder')) {
                    input.val('');
                }
            })
        });
        */
        // 原生js
        var inputList = document.querySelectorAll('[placeholder]');
        var formList = new Set();
        inputList.forEach(function(v,i){
            v.addEventListener('focus', function(evt){
                var input = evt.target;
                if (input.value == input.placeholder) {
                    input.value = '';
                    input.classList.remove('placeholder');
                }
            });
            v.addEventListener('blur', function(evt){
                var input = evt.target;
                if (input.value == '' || input.value == input.placeholder) {
                    input.classList.add('placeholder');
                    input.value = input.placeholder;
                }
            })
            v.blur();
            var parent = v.parentNode;
            while (parent.tagName !== 'FORM' && parent.tagName !== 'BODY') {
                parent = parent.parentNode;
            }
            if (parent.tagName === 'FORM') {
                formList.add(parent);
            }
        })
        formList.forEach(function(form){
            form.addEventListener('submit', function(evt){
                evt.target.querySelectorAll('[placeholder]').forEach(function(input){
                    if (input.value == input.placeholder) {
                        input.value = '';
                    }
                })
            })
        })

    }
```