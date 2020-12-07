---
layout: article
title: The Art of Logging
tags: logging python
mathjax: true
key: 2020-10-24-python-logging
---

### WWH(What, Why, How) for Log?

I do not think log is a concept that computer professionals have hardly heard, but I want to explain again with something that I have seen online said, that is, "Logs are like car insurance. Nobody wants to pay for it, but when something goes wrong everyone wants to get it."

When everything works well, nobody cares about log. But if the system suddenly crashes, files can be lost, programs can be shut down, work you have done for a long time can be empty. Almost people think the restart can solve every problem, although it has indeed handled 99%. But what if the remaining problems will cost several hours, or even longer? What if the problems happen again and again?

Searching in Google, looking at the documentation, asking someone for help... but maybe it is still not powerful enough to answer all your questions. Under this situation, I think, you should pin your hope on attempting to browse logs and find the location of problems so that solve the problems completely and clearly.

Now, let's talk about what logs are systematically. Logs are some files automatically created by the system to record of all the events from programs. More exactly, it keeps track of every event that happens in your program from the minute you start running it to the second you stop it. The whole information that you don't normally track are recorded in the log, and that's the reason why you can still have the ability to find out what have happened in the programs after a sudden crash. For developers, once you start looking in the log files when you have weird errors, it becomes easier to find ways to fix them.

There are several choices about how to use log to get more accurate and profer information. The logging function that programming languages in the early time provide is somewhat rudimentary, and the ouput is generally direct to the terminals and files. These languages contain C++, Java, and so on. As for them, there is an individual log frame, such as glog, log4cpp, log4j, elf4j, etc. However, several new languages like Python have built-in logging module.

In next section, I will introduce the logging module in Python properly, with text description and code examples. There are also offical resource link for you to learn:

* Tutorial: https://docs.python.org/3/howto/logging.html#logging-howto

* Documentation: https://docs.python.org/3/library/logging.html

* Cookbook: https://docs.python.org/3/howto/logging-cookbook.html#logging-cookbook

* Source Code: https://github.com/python/cpython/blob/3.9/Lib/logging/\_\_init\_\_.py


### Logging Module of Python

The basic classes of Logging module defined by the module, together with their functions, are listed below.
* Loggers expose the interface that application code directly uses.
* Handlers send the log records (created by loggers) to the appropriate destination.
* Filters provide a finer grained facility for determining which log records to output.
* Formatters specify the layout of log records in the final output.





