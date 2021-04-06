# widen-tag

```javascript
<script type="text/javascript">
    window._paq = window._paq || [];
    _paq.push([PARAMETER_KEY, VALUE]);
    _paq.push(['e_n',"OPEN"]);

    (function () {
        var tScript = document.getElementsByTagName('script')[0]

        var gScript = document.createElement('script');
        gScript.src = CDN_URL;
        gScript.async = true;
        gScript.defer = true;

        tScript.parentNode.insertBefore(gScript, tScript);
    })();
</script>
```
