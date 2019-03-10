So can whole programs really be implemented as pure functions?

In some cases they obviously can. The `bash` command `wc` for example takes some text as input and returns the number of words in the text as output. You can easily imagine `wc` represented as a table with a row for every possible sequence of characters. The second column would contain the count of words contained in the first column. Given the same piece of text, you'll always get the same number out.

But what about something more interesting like a web application or a video game? On first thoughts you'd expect to start the program with no input (other than maybe some configuration parameters), with it eventually ending with no output other than maybe an exit code. All of the interesting inputs and outputs are in the form of user inputs and data appearing in a database or images rendered to the screen.
