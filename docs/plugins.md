# jQuery

If you use jQuery 1.7 and up, you can include a plugin script that will instrument jQuery to wrap any functions passed into jQuery's ready(), on() and off() to catch errors and report them to Rollbar. To install this plugin, copy the following snippet into your pages, making sure it is BELOW the `<script>` tag where jQuery is loaded:

<!-- EditableTextAreaStart -->
<!-- RemoveNext -->
```html
<script>
!function(a,b,c){var d=b.Rollbar;if(d){var e="0.0.6";d.configure({notifier:{plugins:{jquery:{version:e}}}});var f=function(a){d.error(a),b.console&&b.console.log(a.message+" [reported to Rollbar]")};a(c).ajaxError(function(a,b,c,e){var f,g=b.status,h=c.url,i=c.type;e&&e.hasOwnProperty("stack")&&(f=e);var j={status:g,url:h,type:i};d.warning("jQuery ajax error for "+i,j,f)});var g=a.fn.ready;a.fn.ready=function(a){return g.call(this,function(){try{a()}catch(b){f(b)}})};var h=a.event.add;a.event.add=function(b,c,d,e,g){var i,j=function(a){return function(){try{return a.apply(this,arguments)}catch(b){f(b)}}};return d.handler?(i=d.handler,d.handler=j(d.handler)):(i=d,d=j(d)),d.guid=i.guid?i.guid:i.guid=a.guid++,h.call(this,b,c,d,e,g)}}}(jQuery,window,document);
</script>
```
<!-- RemovePrev -->
<!-- EditableTextAreaEnd -->

The plugin will also automatically report any AJAX errors using jQuery's `ajaxError()` handler. You can disable this functionality by configuring the Rollbar notifier with the following:
```javascript
window.Rollbar.configure({
  payload: {
    notifier: {
      plugins: {
        jquery: {
          ignoreAjaxErrors: true
        }
      }
    }
  }
});
```
