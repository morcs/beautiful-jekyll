So can whole programs really be implemented as pure functions?

In some cases they obviously can. The `bash` command `wc` for example takes some text as input and returns the number of words in the text as output. You could represent `wc` as a table with a row for every possible sequence of characters, mapping the text to the count of words contained within it. Every piece of text contains a number of words, and that number never changes.

But what about something more interesting like a web application or a video game? On first thoughts you'd expect to start the program with no input (other than maybe some configuration parameters), with it eventually ending with no output other than maybe an exit code. All of the interesting behaviour manifests itself as data appearing in a database, or images being rendered to the screen, as the result of user inputs which from the machine's point-of-view sort of appear randomly over time.
