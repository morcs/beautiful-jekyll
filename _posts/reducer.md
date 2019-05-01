So can whole programs really be implemented as pure functions?

In some cases they obviously can. The `bash` command `wc` for example takes some text (i.e. a string) as input and returns the number of words in the string as output. You could represent `wc` as a table with a row for every possible example of a string. The table would map the string in the first column to a word count in the second column. This neatly matches our definition of a pure function.

But what about something more interesting like a web application or a video game? On first thought you'd expect to start the program with no input (other than maybe some config parameters), and after a time you'd expect it to end with no output other than maybe an exit/error code. All of the interesting behaviour manifests itself as side-effects, such as files being written, data appearing in a database, or images being rendered to the screen. These side-effects would emerge as the result of external things, such as user inputs, which from the machine's point-of-view sort of appear randomly over time.

In fact it's _time_ that seems to be the cause of impurity. In the `wc` example, we know all the input at the start.

## The benefit of hindsight

Is there another way of describing these types of program that makes them look more like pure functions? Well if we happen to be looking at things AFTER the program has run, there is! Everything looks simple with the benefit of hindsight after all.

Looking back on the execution of a program, you could have a timestamped log of all of the user actions that became input.

Video game
Timestamped log of all of the controller movements and button presses
Sequence of screen renders (a video)

Spreadsheet
User keyboard and mouse inputs
File

Web application
Browser events triggered by the user typing/clicking
HTML representing the current view
