So can a program that does anything useful be represented as a pure function?

In some cases they obviously can. A `bash` command for example. The `wc` command takes a file as input and outputs the number of words in the file. Given the same file it'll always return the same number. You can easily imagine an infinitely long table that lists every possible sequence and combination of text characters in the left hand column, and a count of the number of words in that text in the right-hand column.
