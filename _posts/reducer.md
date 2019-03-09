So can a program that does anything useful be represented as a pure function?

In some cases they obviously can. The `bash` command `wc` for example takes a file as input and returns the number of words in the file as output. Given the same file it'll always return the same number. You can easily imagine an infinitely long table with a row for every possible sequence of text characters, mapped to the number of words in that text.
