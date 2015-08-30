```
window.heap = window.heap || [];
heap.load = function (a) {
    window._heapid = a;
    var b = document.createElement("script");
    b.type = "text/javascript", b.async = !0, b.src = ("https:" === document.location.protocol ? "https:" : "http:") + "//cdn.heapanalytics.com/js/heap.js";
    var c = document.getElementsByTagName("script")[0];
    c.parentNode.insertBefore(b, c);
    var d = function (a) {
        return function () {
            heap.push([a].concat(Array.prototype.slice.call(arguments, 0)))
        }
    }, e = ["identify", "track"];
    for (var f = 0; f < e.length; f++) heap[e[f]] = d(e[f])
};
```
Heap Analytics is printing an inline Javascript call to the page that references, loads, and initializes the off-site analytics tracking script from it with parameters specific to your website (like tracking options and ID).


heap.load("YOUR_APP_ID");

The first thing the script does is define a global heap object.

The second thing the script does is define a heap.load method. heap.load first sets your app id to a global variable to save it for later.

Next, it creates a new script element to load the heap.js tracking script. heap.js takes care of logging user events to Heap.

After loading heap.js, heap.load defines stubs for two other methods: heap.identify and heap.track. This enables you can call those methods even if heap.js hasn't loaded. (You can read more about what they do on Heap's docs page.)

The third thing the script does is call heap.load with your app id, which does everything I just talked about. :)

