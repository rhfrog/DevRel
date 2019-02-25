# Build Times Matter - Speed is Everything

After GoCenter was first released the team at JFrog said it was faster to get modules from there than it was getting modules “_the old way_”. To be honest, when they said that, I did not believe them. So, I set out to test the speed and see whether those claims were accurate.

**TL;DR**: It turns out the team was right! GoCenter doesn’t only save time, it also saves CPU cycles. As your project gets bigger, and your dependency tree grows, the savings will increase too. You don’t have to take my word for it, check out the “_Now it’s your turn!_” section below to see my test script and start your test in two minutes flat!

In order to make the test fair, I decided to take two projects and test them at three different networks. I chose one of my [personal projects](https://github.com/retgits/webhook-bridge) and my favorite static site generator, [Hugo](https://github.com/gohugoio/hugo). The GoCenter team had no influence on the projects or the modules and versions used. The three locations were to make sure that testing at the JFrog office couldn’t favor GoCenter unfairly.

## The test

Building a Go project usually consists of two parts. First of all, you’d download the dependencies and second you’d build the executable. With all of the tests done on the same machine, the results only focused on the first part, downloading the dependencies. To make the test as fair and uninterrupted as possible, I wrote a little script that removes all modules before running “go get”. For both using GitHub and GoCenter, the commands were ran ten times and the result logged in a file.

## The numbers

The complete list of tests and results can be found [here](https://docs.google.com/spreadsheets/d/1Vb6pb0UI3QGw1RG-X6-hZap1bBxZVz-9pNHQ0PddVTA/edit?usp=sharing). To make comparing the results a little easier the below tables mark the difference between the fastest and slowest runs for each of the projects.

**Comparison table for my personal project using the JFrog network**

|                            | CPU seconds spent in user mode (less is better) | CPU seconds spent in kernel mode (less is better) | The CPU percentage (less is better)  | Total elapsed time in seconds  (less is better) |
|----------------------------|-------------------------------------------------|---------------------------------------------------|--------------------------------------|-------------------------------------------------|
| Comparing the slowest runs | GoCenter spends 6.31 times fewer CPU seconds    | GoCenter spends 4.43 times fewer CPU seconds      | GoCenter uses 4.58 times less CPU    | Using GoCenter is 1.21 times faster             |
| Comparing the fastest runs | GoCenter spends 5.98 times fewer CPU seconds    | GoCenter spends 4.42 times fewer CPU seconds      | GoCenter uses 3.37 times less CPU    | Using GoCenter is 1.59 times faster             |

**Comparison table for my personal project using my home network**

|                            | CPU seconds spent in user mode (less is better) | CPU seconds spent in kernel mode (less is better) | The CPU percentage (less is better)  | Total elapsed time in seconds  (less is better) |
|----------------------------|-------------------------------------------------|---------------------------------------------------|--------------------------------------|-------------------------------------------------|
| Comparing the slowest runs | GoCenter spends 5.97 times fewer CPU seconds    | GoCenter spends 4.23 times fewer CPU seconds      | GoCenter uses 3.62 times less CPU    | Using GoCenter is 1.44 times faster             |
| Comparing the fastest runs | GoCenter spends 6.19 times fewer CPU seconds    | GoCenter spends 4.48 times fewer CPU seconds      | GoCenter uses 2.46 times less CPU    | Using GoCenter is 2.24 times faster             |

**Comparison table for my personal project using a hotel network**

|                            | CPU seconds spent in user mode (less is better) | CPU seconds spent in kernel mode (less is better) | The CPU percentage (less is better)  | Total elapsed time in seconds  (less is better) |
|----------------------------|-------------------------------------------------|---------------------------------------------------|--------------------------------------|-------------------------------------------------|
| Comparing the slowest runs | GoCenter spends 7.85 times fewer CPU seconds    | GoCenter spends 4.84 times fewer CPU seconds      | GoCenter uses 2.26 times less CPU    | Using GoCenter is 3.05 times faster             |
| Comparing the fastest runs | GoCenter spends 7.38 times fewer CPU seconds    | GoCenter spends 4.28 times fewer CPU seconds      | GoCenter uses 1.88 times less CPU    | Using GoCenter is 3.33 times faster             |

Looking at the total elapsed time, which is the time the machine spends getting the modules and putting them into the right location, using GoCenter is a lot faster than getting them from GitHub.

I’ll accept that this is a personal project with only 6 directly used modules and 12 indirectly used modules. What would be the impact of using GoCenter with a much larger project, like [Hugo](http://gohugo.io) with 43 directly used modules and 11 indirectly used modules?

**Comparison table for Hugo project using the JFrog network**

|                            | CPU seconds spent in user mode (less is better) | CPU seconds spent in kernel mode (less is better) | The CPU percentage (less is better)  | Total elapsed time in seconds  (less is better) |
|----------------------------|-------------------------------------------------|---------------------------------------------------|--------------------------------------|-------------------------------------------------|
| Comparing the slowest runs | GoCenter spends 8.60 times fewer CPU seconds    | GoCenter spends 5.82 times fewer CPU seconds      | GoCenter uses 3.00 times less CPU    | Using GoCenter is 2.53 times faster             |
| Comparing the fastest runs | GoCenter spends 9.23 times fewer CPU seconds    | GoCenter spends 6.17 times fewer CPU seconds      | GoCenter uses 2.85 times less CPU    | Using GoCenter is 2.78 times faster             |

**Comparison table for Hugo project using my home network**

|                            | CPU seconds spent in user mode (less is better) | CPU seconds spent in kernel mode (less is better) | The CPU percentage (less is better)  | Total elapsed time in seconds  (less is better) |
|----------------------------|-------------------------------------------------|---------------------------------------------------|--------------------------------------|-------------------------------------------------|
| Comparing the slowest runs | GoCenter spends 7.77 times fewer CPU seconds    | GoCenter spends 5.33 times fewer CPU seconds      | GoCenter uses 2.42 times less CPU    | Using GoCenter is 2.83 times faster             |
| Comparing the fastest runs | GoCenter spends 8.64 times fewer CPU seconds    | GoCenter spends 6.04 times fewer CPU seconds      | GoCenter uses 2.98 times less CPU    | Using GoCenter is 2.55 times faster             |

**Comparison table for Hugo project using a hotel network**

|                            | CPU seconds spent in user mode (less is better) | CPU seconds spent in kernel mode (less is better) | The CPU percentage (less is better)  | Total elapsed time in seconds  (less is better) |
|----------------------------|-------------------------------------------------|---------------------------------------------------|--------------------------------------|-------------------------------------------------|
| Comparing the slowest runs | GoCenter spends 17.79 times fewer CPU seconds   | GoCenter spends 7.03 times fewer CPU seconds      | GoCenter uses 6.12 times less CPU    | Using GoCenter is 2.14 times faster             |
| Comparing the fastest runs | GoCenter spends 9.37 times fewer CPU seconds    | GoCenter spends 5.64 times fewer CPU seconds      | GoCenter uses 2.64 times less CPU    | Using GoCenter is 2.94 times faster             |

Even for larger projects, it makes sense to use GoCenter to get Go modules.

## The results

As it turns out the team was right! GoCenter isn’t only useful to eliminate [vendoring](https://medium.com/jfrogplatform/golang-dependency-management-doing-it-right-3f214878a188) and making your builds repeatable using [immutable modules](https://github.com/jfrog/gocenter/wiki/Frequently-Asked-Questions#how-does-gocenter-guarantee-persistence-and-immutability). Using GoCenter to get your Go modules is also faster than getting them from a Version Control System. There are two interesting results from the above numbers. First of all, on slower networks, you’ll see a bigger performance increase as downloading modules takes a lot less bandwidth than downloading source code. Secondly, as the complexity of the project grows (larger dependency trees), you’ll also see a bigger performance increase. Apart from the performance increase, using GoCenter also saves CPU cycles so you can spend those cycles on other things.

## Now it’s your turn

You don’t have to take my word for it, in fact, I challenge you to try it yourself! This is the script I used to test the project:

<script src="https://gist.github.com/retgits/a67ce9b01fbe0e7c6f8dbe9dbd56a446.js"></script>

I challenge you to try it on your project and tweet the results to us using the hashtags #golang and #GoCenter. After you do that, drop us an email on [gocenter@jfrog.com](mailto:gocenter@jfrog.com) with an address we can ship this awesome shirt to!

![go-shirt](https://jfrog.com/wp-content/uploads/2018/08/Gopher-Shirt.png)