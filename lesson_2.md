
## Exercise 2:

Async thing that runs in node - files.  Come up with a quick file example:
- reads one file, then takes the output, uses that to read another file.

Don't use readFileSync (or maybe you show this, and ask them to make it async)

## Synchronous File Reading Example (blocking)
```
var fs = require('fs');

var contents = fs.readFileSync('DATA', 'utf8');
console.log(contents);
```

## Figure out what the asynchronous (non-blocking) version of this would look
like?

HINT: Google!!

Exercise 4:

Mongo query - setup a quick mongo script, get one record, then get associated
records based on that record's data

Find a post by title
Then find all comments by that post's id

Exercise 3:

quick ajax call (setup an html page, and two json files, start the server with
http-server, make one ajax call and then make the next call based on the result
of the first)


## Reflect

go back to objectives.  How'd you do?

New questions:

Add 4 new questions here:

1. _
1. _
1. _
1. _
