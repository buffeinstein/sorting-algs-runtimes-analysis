# lab: sorting runtimes

In this lab you will measure just how much faster the built-in `sorted` function is than the functions you implemented on the last homework assignment.
You will also learn about how to use git submodules to connect multiple git repos together.

You will need a partner for portions of this lab.
(And recall that you must do partner work either in class or the QCL.)

## Tasks

**Task 1: initialize the repo**

Do not clone this repository.
Instead:

1. Create a new empty repo.
    Ensure that the name of your repo is different than the name of your partner's repo (so that you can fork it later).

1. Copy the contents of this repo to your new repo with the following commands
    ```
    $ git clone https://github.com/mikeizbicki/lab-sorting-runtimes
    $ cd lab-sorting-runtimes
    $ git remote rm origin
    $ git remote add origin <URL>
    $ git push origin master
    ```
    where `<URL>` is the url to your newly created repo.

    > **NOTE:**
    > In the past I used `$URL` syntax to denote a "variable" that you need to substitute into a command.
    > Both the `$URL` and `<URL>` style syntaxes are commonly used.

    > **NOTE:**
    > At this point you should ensure you understand what the `git remote` commands are doing fully.
    > I will stop giving these commands to you explicitly in the future.

**Task 2: setup the submodule**

Run the `runtimes.py` file.
You should get a `ModuleNotFoundError` that looks something like
```
$ python3 runtimes.py
Traceback (most recent call last):
  File "runtimes.py", line 9, in <module>
    from sorting.sorting import merge_sorted, quick_sorted
ModuleNotFoundError: No module named 'sorting'
```
> **NOTE:**
> In the code block above, I put both the command and the entire output of the command.
> Before the code block, I put a short summary of my steps using English intermixed with inline-code.
> When reporting error messages, it is standard to have a short English language summary and a full output of the error message in a code block.
> (Even if the output is hundreds of lines long, people will still include the full output, since all of those lines provide important information.)
> If you report your errors in this same way (e.g. on github issues/stackoverflow), you will be more likely to get good help from people.

The problem is that the `runtimes.py` file is trying to load the functions that you wrote in the sorting homework,
but it can't find those files.
The most obvious way to get access to those files is to clone your sorting homework into the repository.
BUT DON'T DO THIS!
It is never a good idea to clone one git repository inside another repository directly.
Instead, we will use something called a [git submodule](https://git-scm.com/docs/gitsubmodules) which will allow the two git repositories to "talk" to each other when needed.

<!--
> **NOTE:**
> Git has a semi-official tutorial website at <https://git-scm.com>.
> It has a (very long) tutorial on [git submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules) that discusses all of their features, and much more than you'll need to know for this class.
-->

You can initialize a git submodule by running the command:
```
$ git submodule add <URL>
```
where `<URL>` is the url to your sorting homework.

If this command works successfully, then the command
```
$ ls -a
```
should have a new `sorting` directory in its output and a file `.gitmodules`.
If you run
```
$ ls sorting
```
then you should see all of the contents of your sorting repo.
Running the command 
```
$ python3 runtimes.py
```
should now generate no errors.
If you run
```
$ git status
```
you should see that the `sorting` repo and the `.gitmodules` file have has been added into the staging area.
You should commit theses changes and push them to github now.

If you visit the github website for your lab repo,
you should see that the `sorting` folder is now visible in the repo,
and it will have a symbol next to it that looks like `@ f43eacf` where the hash number after the `@` is the hash of the commit of the sorting repo and will be different.
Clicking on the folder should take you to the url for your sorting repo and away from this repo.
There will be no indication on your sorting repo that it is a git submodule in another repo.

**Task 3: runtimes of sorting algorithms on random lists**

Run the command
```
$ python3 runtimes.py --max_x=5
```
This will use the `timeit` module on 5 different lists of increasing size to measure the runtimes of the built-in `sort` function (which uses timsort) versus the sorting functions that you implemented in your last homework.
The output of this command, however, is currently not nicely formatted.
At the end of the file is a line labeled `FIXME 1`.
Follow the instructions so that the `runtimes.py` file generates output in markdown table format.

> **NOTE:**
> It is very common to write python code that generates code in another programming language, especially markdown or html.

After you've completed the FIXME, run the following command
```
$ python3 runtimes.py --max_x=22
```
and copy/paste the resulting table into this README file below this line.


|    | timsort | merge_sorted | quick_sorted |
len(xs)=2**0 runtimes={'timsort': 2.8908252716064453e-06, 'merge_sorted': 2.1569430828094482e-06, 'quick_sorted': 2.216547727584839e-06}
| 2**0 | 2.89e-06 | 2.16e-06 | 2.22e-06 |
len(xs)=2**1 runtimes={'timsort': 1.7657876014709473e-06, 'merge_sorted': 7.275491952896118e-06, 'quick_sorted': 8.691102266311646e-06}
| 2**1 | 1.77e-06 | 7.28e-06 | 8.69e-06 |
len(xs)=2**2 runtimes={'timsort': 1.6540288925170898e-06, 'merge_sorted': 1.1693686246871948e-05, 'quick_sorted': 1.1418014764785767e-05}
| 2**2 | 1.65e-06 | 1.17e-05 | 1.14e-05 |
len(xs)=2**3 runtimes={'timsort': 2.246350049972534e-06, 'merge_sorted': 2.42479145526886e-05, 'quick_sorted': 2.3331493139266968e-05}
| 2**3 | 2.25e-06 | 2.42e-05 | 2.33e-05 |
len(xs)=2**4 runtimes={'timsort': 3.200024366378784e-06, 'merge_sorted': 5.165114998817444e-05, 'quick_sorted': 4.9132853746414185e-05}
| 2**4 | 3.20e-06 | 5.17e-05 | 4.91e-05 |
len(xs)=2**5 runtimes={'timsort': 5.4389238357543945e-06, 'merge_sorted': 0.00011038780212402344, 'quick_sorted': 0.0001120157539844513}
| 2**5 | 5.44e-06 | 1.10e-04 | 1.12e-04 |
len(xs)=2**6 runtimes={'timsort': 1.1019408702850342e-05, 'merge_sorted': 0.00024988874793052673, 'quick_sorted': 0.0002718418836593628}
| 2**6 | 1.10e-05 | 2.50e-04 | 2.72e-04 |
len(xs)=2**7 runtimes={'timsort': 2.0969659090042114e-05, 'merge_sorted': 0.0005677156150341034, 'quick_sorted': 0.0005967617034912109}
| 2**7 | 2.10e-05 | 5.68e-04 | 5.97e-04 |
len(xs)=2**8 runtimes={'timsort': 4.521384835243225e-05, 'merge_sorted': 0.0011812523007392883, 'quick_sorted': 0.0014373064041137695}
| 2**8 | 4.52e-05 | 1.18e-03 | 1.44e-03 |
len(xs)=2**9 runtimes={'timsort': 0.0001009330153465271, 'merge_sorted': 0.0025784894824028015, 'quick_sorted': 0.0030718594789505005}
| 2**9 | 1.01e-04 | 2.58e-03 | 3.07e-03 |
len(xs)=2**10 runtimes={'timsort': 0.00021866336464881897, 'merge_sorted': 0.005689818412065506, 'quick_sorted': 0.007081698626279831}
| 2**10 | 2.19e-04 | 5.69e-03 | 7.08e-03 |
len(xs)=2**11 runtimes={'timsort': 0.000479135662317276, 'merge_sorted': 0.012330900877714157, 'quick_sorted': 0.01457204669713974}
| 2**11 | 4.79e-04 | 1.23e-02 | 1.46e-02 |
len(xs)=2**12 runtimes={'timsort': 0.0010517612099647522, 'merge_sorted': 0.026784319430589676, 'quick_sorted': 0.030973006039857864}
| 2**12 | 1.05e-03 | 2.68e-02 | 3.10e-02 |
len(xs)=2**13 runtimes={'timsort': 0.002276849001646042, 'merge_sorted': 0.057231269776821136, 'quick_sorted': 0.07228025794029236}
| 2**13 | 2.28e-03 | 5.72e-02 | 7.23e-02 |
len(xs)=2**14 runtimes={'timsort': 0.00494539737701416, 'merge_sorted': 0.09082823991775513, 'quick_sorted': 0.09473486244678497}
| 2**14 | 4.95e-03 | 9.08e-02 | 9.47e-02 |
len(xs)=2**15 runtimes={'timsort': 0.007100313901901245, 'merge_sorted': 0.1702684946358204, 'quick_sorted': 0.19592296332120895}
| 2**15 | 7.10e-03 | 1.70e-01 | 1.96e-01 |
len(xs)=2**16 runtimes={'timsort': 0.015462405979633331, 'merge_sorted': 0.35571739077568054, 'quick_sorted': 0.4366283528506756}
| 2**16 | 1.55e-02 | 3.56e-01 | 4.37e-01 |
len(xs)=2**17 runtimes={'timsort': 0.034007344394922256, 'merge_sorted': 0.7711575552821159, 'quick_sorted': 0.9004266820847988}
| 2**17 | 3.40e-02 | 7.71e-01 | 9.00e-01 |
len(xs)=2**18 runtimes={'timsort': 0.07599404081702232, 'merge_sorted': 1.642664574086666, 'quick_sorted': 2.0186996199190617}
| 2**18 | 7.60e-02 | 1.64e+00 | 2.02e+00 |
len(xs)=2**19 runtimes={'timsort': 0.1791650727391243, 'merge_sorted': 3.561136197298765, 'quick_sorted': 4.32806421816349}
| 2**19 | 1.79e-01 | 3.56e+00 | 4.33e+00 |
len(xs)=2**20 runtimes={'timsort': 0.4085576683282852, 'merge_sorted': 7.534945569932461, 'quick_sorted': 9.274117216467857}
| 2**20 | 4.09e-01 | 7.53e+00 | 9.27e+00 |
len(xs)=2**21 runtimes={'timsort': 0.9397964179515839, 'merge_sorted': 16.327220354229212, 'quick_sorted': 21.38327256217599}
| 2**21 | 9.40e-01 | 1.63e+01 | 2.14e+01 |
len(xs)=2**22 runtimes={'timsort': 2.174213510006666, 'merge_sorted': 34.74820298328996, 'quick_sorted': 50.07403355836868}
| 2**22 | 2.17e+00 | 3.47e+01 | 5.01e+01 |
<!-- add the table here -->

You should observe that python's built-in sort function is 10-100x faster than yours.
All functions have the same wort-case asymptotic complexity (i.e. $\Theta(n \log n)$),
but python's built-in sorting function uses lots of optimization tricks to achieve this extra speedup.
Native python code is not very good at performing these types of optimizations,
and so the built-in function is written in the C programming language.
(You can find the [source code of the built-in function on github](https://github.com/python/cpython/blob/c1b1f51cd1632f0b77dacd43092fb44ed5e053a9/Python/bltinmodule.c#L2356)).
Functions that must be very fast are generally written in C instead of Python.
One of the differences between a *computer sciencee* major and a *data science* major is that the CS major focuses on how to *write* these fast functions,
and the DS major focuses on how to *use* these fast functions to accomplish interesting tasks.

<!--
For fun, compare the runtimes of your sorting algorithms to your partner's to see who has the fastest implementation.
-->

Push your changes to github and verify that the table is being displayed correctly.

**Task 4: cloning**

Fork your partner's repo and clone your partner's repo to the lambda server.
In this forked repo, re-run the command
```
$ python3 runtimes.py --max_x=22
```
You should get a `ModuleNotFoundError` again.

The problem is that when you clone a repo with submodules,
the submodules are by default not cloned as well.

> **ASIDE:**
> [Facebook announced in 2014 that the size of the git repo for facebook.com was 54GB](https://news.ycombinator.com/item?id=7648237),
> and it is undoubtably much larger today.
> This 54GB only includes the code directly responsible for facebook.com, and not any of the libraries that facebook.com depends on.
> Facebook's releases many public libraries on github at <https://github.com/facebook>,
> and most of these are going to be included in the main facebook.com repo as submodules.
> Some popular examples include the [react](https://github.com/facebook/react) web framework and [pytorch](https://github.com/pytorch/pytorch) machine learning library.
> Since these are their own separate git repos (and therefore submodules of facebook.com),
> developers can work on these libraries without needing access to the main facebook.com repo.
> Or if they are working at Facebook and do have access,
> they don't need to waste their time downloading all of the 54GB of code for facebook.com just to work on react or pytorch.
>
> The main repo for facebook.com is not public, and not even hosted with github.
> Recall that one of the advantages of git is that it is a *distributed version control system* not tied to any hosting provider.
> Facebook only uses github for its public repos,
> and uses an internal git service for storing its proprietary repos like the one for facebook.com.

Now run the command
```
$ ls
```
and notice that the `sorting` folder does exist in your forked repo.
The problem is that it is empty,
and you can verify that with the command
```
$ ls sorting
```
We need to *populate* or *hydrate* this folder, which is currently *unpopulated* or *dehydrated*.
You can do that with the following sequence of commands:
```
$ git submodule init
$ git submodule update
```
Now, you should be able to run the original command successfully
```
$ python3 runtimes.py --max_x=22
```

**Task 5: runtime of sorting an already sorted list**

Remain in the forked repo for this task.

We will now measure the runtime of the sort functions on data that has already been sorted.
Do this by adding the `--input=sorted` argument to get the following
```
$ python3 runtimes.py --max_x=23 --input=sorted
Traceback (most recent call last):
  File "runtimes.py", line 38, in <module>
    xs = FIXME
NameError: name 'FIXME' is not defined
```
You'll notice that you get a `NameError` because the word `FIXME` is undefined.

> **NOTE:**
> It is common practice in python to put the word FIXME as a variable in a code path that you haven't implemented yet.
> If that code path accidentally gets run, then python will raise the `NameError` to let you know that you are running code that you shouldn't be running.
> From python's perspective, there's nothing special about the word `FIXME`:
> it's just a variable name that has not been defined.

Follow the instructions in the comments to provide a proper definition of `xs`,
then rerun the command above to generate a markdown table of runtimes.
Copy/paste the table into the README file below this line.

<!-- add the table here -->

You should notice that the built-in `sorted` function ran much faster on this input,
but your `merge_sorted` and `quick_sorted` functions have essentially the same runtimes.
This is because TimSort is designed to not have to resort already sorted data,
and its best-case runtime is therefore $\Theta(n)$ instead of $\Theta(n\log n)$.
It turns out that in practice, data is often partially sorted,
an so TimSort is much faster than even highly optimized mergesort or quicksort implementations.

Add/commit your changes and push them to the forked github repo.
Issue a pull request to your partner with your changes,
and accept your partner's pull request when you receive it.

## Submission

Submit the link to both your and your partner's github repos to sakai.
