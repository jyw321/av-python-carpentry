---
title: "Finding Files"
teaching: 20
exercises: 10
questions:
- "How can I use Python to find different types of files?"

objectives:
- "Import modules to add functions to a Python script"
- "Use `os.walk` and `glob.glob` to generate lists of files"

keypoints:
- "Collecting filepaths is often the first step of any AV script"
- "The import statement allows you to expand Python's functionality by loading modules with specialized functions"
- "Standard distribution modules are installed with Python. Other modules must be installed"
- "Two common modules for listing files with Python are `os` and `glob`"
- "The filepath is the sequence of parent folders that define the location of a particular file or directory"
---

## The First Step: Finding Files

Before Alice can do anything with any file in Python, she first needs to be able to refer Python to the location that file.

## Navigating filenames, file paths, and file systems

Gaining a basic understanding of file systems is the first step in learning how to find and work with files. File systems provide a structured method for arranging data on a storage device. If you've ever formatted a hard drive using Disk Utility (macOS) or Disk Management (Windows), you've applied some form of file system to your device. That file system is what makes it possible to create files and folders on your storage device.

File systems have different rules, regulations, and ways of behaving; in your work as AV archivists, you'll most likely encounter a few common acronyms:

* FAT32 (old Windows)
* NTFS (new Windows)
* exFAT (made by Windows but cross-platform)
* HFS+ (old macOS, aka Mac OS Extended)
* APFS (new macOS as of 2019)

If you've used a program like Windows Explorer or macOS Finder, you probably have an intuitive sense of how to navigate within a file system by using a mouse to click or double-click on folders or file icons. When we use scripts, we use a similar logic to express the location of a particular object within the larger hierarchical system structure -- the file path.

File paths are a series of text-based breadcrumbs that describe the location of a particular file or directory. There are two types of file paths: 
* absolute, or full file paths, which include what's called the root directory, or the top-most level of your file system
* relative file paths, which provide a file's location in relation to your current working directory.

Alice uses macOS. For her, the root directory is the slash character `/`. Every absolute file path will start with `/`.

The absolute file path to a cool file on her desktop would be: `/Users/alice/Desktop/CoolStuff/qckitty.gif` That path will always lead to that file, no matter where she's working from (her current working directory).

> ## Root Directories on Windows vs Mac/Linux
>
> On Windows, there isn't a root directory for the entire machine.
> File systems are rooted on the storage volume that they use.
> For example, `A:\` has traditionally referred to the root directory of a floppy drive.
> And `C:\` generally refers to the root directory that contains user files, programs, and windows itself.
>
> On Mac/Linux, the operating system defines a single file system for all devices.
> The root folder is '/'.
> If you plug in an external drive, it is probably mounted as a folder in '/Volumes/'.
{: .callout}

Some portions of that path, for example, `Desktop`, might be as familiar to you as the folders you see in a GUI.
A file path encodes the same hierarchy of folders that we click through, expressing it as series of folder names divided by slashes.
We know that `CoolStuff` is a subdirectory stored inside of `Desktop`.
We can also see that `Desktop` is a subdirectory of `alice`, which is a subdirectory of `Users` which is a subdirectory of root.

You may not see folders like `Users` on a regular basis.
Often, we spend time working from a folder like `Desktop` and thinking about the locations of files relative to that folder.
The relative file path to `qckitty.gif` from Alice's `Desktop` is `CoolStuff/qckitty.gif`.
That path only works in relation to the `Desktop` folder.
If we try to access `CoolStuff/qckitty.gif` from the root folder, our computer will report an error, explaining that it cannot find anything at that path.

This distinction will become particularly important as we begin using Python to find and do things to different kinds of files.
Our first step will always be gathering up file paths based upon a chosen quality such as filename extension.

> ## Slashes
>
> On Windows, the backward slash `\` is used to separate parts of a path.
> This can be frustrating when moving back and forth between operating systems.
> We will be using Python to remove some of these frustrations and write code that is more portable.
> 
> If you find code in this workshop that doesn't work because it uses the wrong kind of slashes for your operating system, please let us know.
{: .callout}

> ## Running More Python Cells
>
> For each of the following paths, choose if they are
> 1. absolute or relative
> 2. Windows or Unix/Mac
>
> ~~~
> 1. C:\Users\admin\Desktop
> 2. /Desktop/Downloads
> 3. Downloads
> 4. usr/local/bin/python
> ~~~
> {: .language-bash}
> 
> > ## Solution
> >
> > 1. Absolute, Windows
> > 2. Absolute, Unix/Mac
> > 3. Relative, Windows/Unix/Mac
> > 4. Relative, Unix/Mac
> {: .solution}
{: .challenge}


## Listing Files in a Folder

Now let's learn how to see the contents of our filesystem with Python.

~~~
import os
os.listdir()
~~~
{: .language-python}

~~~
['Library',
 'Applications',
 'Downloads',
 'Desktop',
 'Documents',
...
~~~
{: .output}

(Your results may be slightly different depending on your operating system and how you have customized your filesystem.)

A lot of things just happened here, so we'll step through each one-by-one.
1. importing module
2. calling functions
3. lists and strings

## Importing modules (`import os`)
The very first line is `import os`.
This is instructs python to load the `os` module.

Modules, or code libraries designed to expand Python's functionality, can be, at times, a little tricky. There are different kinds of modules in Python: 
* standard distribution modules, which are built into Python itself;
* third-party modules, which you load, often at the start of your script;
* home-made modules, which you create to meet your own specific needs.

There are two ways to figure out which modules you already have installed.

When you're in a Jupyter notebook or Python console:

~~~
help('modules')
~~~
{: .language-python}

When you're using a terminal:

~~~
pip list
~~~
{: .language-bash}

The base python environment has very few functions built-in.
In order to power yourself up, you need to import modules with the functions you need.
You only need to load a module once per script.
To make sure we load every module that we need, we typically put all of our import statements at the very top of a script.

> ## Loading another module
>
> We will be using the `glob` module to find files.
> Do you have glob installed?
> How would you load it into your script?
> 
> > ## Solution
> > Yes, glob is one of the standard distribution modules.
> > It can be loaded like this:
> > ~~~
> > import glob
> > ~~~
> > {: .language-python}
> {: .solution}
{: .challenge}

## Calling functions `os.listdir()`

In this code
* `os` is a module that connects Python to your underlying operating system (Mac, Windows, Linux)
* `listdir()` is a function that is part of that module

`os.listdir()` prints the names of files and folders in the current directory as a list.
By default, `os.listdir()` prints contents of the current working directory.
In our case that's the home folder.

> ## Python Syntax: Modules and `.`
>
> Like directory structures, modules have a heirarchal structure.
> Each module can contain functions and submodules, which themselves can contain functions and submodules.
> The `.` syntax works like the slash in a filepath.
> It clarifies that we want to use a function defined in a specific module.
> If we don't include the parent modules, the Python interpreter will not know where to find the function we want to run.
{: .callout}

> ## Python Syntax: Functions and ()
>
> Whenever you see text immediately followed by `( ... )`, you're looking at a function call.
> If no additional information is needed, the parentheses are left empty, like `os.listdir()`.
> Additional information, known as arguments, is written between the parentheses, like this `os.listdir('Desktop')`.
{: .callout}

It can also print the contents of another folder.
We will use the relative path to the Desktop.

~~~
os.listdir('Desktop')
~~~
{: .language-python}

If the path points to folder that doesn't exist, `os.listdir()` will give an error.
~~~
os.listdir('Desk')
~~~
{: .language-python}

We're curious about the contents of the `amia19` folder, so let's examine that.
Because we will use this path repeatedly, first we will store it with a variable.

On macOS:
~~~
video_dir = '/Users/username/Desktop/amia19'
~~~
{: .language-python}

On Windows:
~~~
video_dir = 'C:\\Users\\username\\Desktop\\amia19'
~~~
{: .language-python}

Let's check to make sure we have the right path.

~~~
os.listdir(video_dir)
~~~
{: .language-python}

~~~
['120th anniversary',
 'federal_grant',
 'on_demand'
]
~~~
{: .output}

## Lists and Strings

Python classifies data in different ways to make it useable for functions.
The output of `os.listdir()` introduces us to two data types, strings and lists.

> ## Python Syntax: Strings
> 
> ### Strings
> Characters that you want Python to interpret as single piece of text need to be quoted.
> This is called a string.
> You can uses either single-quotes `'` or double-quotes `"` to wrap a string, although single-quotes are more common (and you don't have to hit the shift-key)
{: .callout}

Strings will be one of our workhorse data types.
Already we've used strings to read folder paths and to write new paths.
We will also them in other contexts as well.

Strings include a number of methods to manipulate them.
For example, 
~~~
'filename.mov'.replace('mov', 'mp4')
~~~
{: .language-python}

~~~
'filename.mp4'
~~~
{: .output}

Notice that we're using the dot-notation we used to call a function.
In this case, the function uses the string itself as an argument.
These types of functions that are attached to objects are called methods.

> ## Python Syntax: Lists
> In Python, lists start and end with square brackets `[` `]`.
> Lists can contain any kind of data, strings, numbers, other lists, etc.
> Each list item is separated by a comma.
>
> This is a list with three items.
> ~~~
> ['120th anniversary',
> 'federal_grant',
> 'on_demand'
> ]
> ~~~
> {: .language-python}
>
> This is a list with one item.
>
> ~~~
> ['120th anniversary, federal_grant, on_demand']
> ~~~
> {: .language-python}
> 
{: .callout}

To learn more about lists, let's work with the folder list we generated above.
First we will store the results of that function call to a variable so we can use the list repeatedly without calling the function every time.

~~~
project_folders = os.listdir(video_dir)
~~~
{: .language-python}

Now we can experiment with the list called `project_folders`.
We can calculate the length of the list:

~~~
len(project_folders)
~~~
{: .language-python}

~~~
3
~~~
{: .output}

We can print specific items within a list by "subscripting" (note that Python uses "zero-based indexing," a.k.a. the first element of a list will start with 0):

~~~
project_folders[0]
~~~
{: .language-python}

~~~
'120th anniversary'
~~~
{: .output}

We can use a negative index to begin at the end of a list:

~~~
project_folders[-1]
~~~
{: .language-python}

~~~
'on_demand'
~~~
{: .output}

We can sort the list (when strings, this means alphabetizing; when integers/floats, this means imposing numerical order):

~~~
sorted(project_folders)
~~~
{: .language-python}

~~~
['120th anniversary',
 'federal_grant',
 'on_demand'
]
~~~
{: .output}

We can add items to a list (by using the append method):
~~~
project_folders.append('frog')
project_folders
~~~
{: .language-python}

~~~
['120th anniversary',
 'federal_grant',
 'on_demand',
 'frog'
]
~~~
{: .output}

## Listing all the files in a directory

If you're asking yourself: are we ever gonna get to finding some damn AV files?
We're here!
Our time is now!

> ## Finding files with Bash
> For reference, in a Unix environment, you might run something like this.
> 
> ~~~
> $ find Desktop/amia19 -type f 
> ~~~
> {: .language-bash} 
>
> Again, it can be extremely useful to know how to accomplish the same goal in multiple ways.
{: .callout}

With `os.listdir()` we could list all the files in a directory of our choosing, but if we have a really nested set of folders like Alice does, we would have to run `os.listdir` over and over again.
Instead, we will use a function from the `glob` module and a function from the `os.path` submodule.

`glob` is a module that lets you use wildcards like `*` in filepaths.
For example, the glob pattern `Desktop\*19` matches against all file and folder names that end with `19` in the `Desktop` folder.
However, you need to use a function from the `glob` module to perform that match.

If you haven't loaded the `glob` module, you can load it now.

~~~
glob.glob('*19')
~~~
{: .language-python}

~~~
['amia19', <maybe additional files depending on your Desktop>]
~~~
{: .output}

We would like to search for `mov` files in our video directory, so we'll need to construct a glob-pattern for those paths.
For this, we will two wildcard characters.
* `*` - a wildcard for part or all of a file name or folder
* `**` - a wildcard for nested folders

> ## globbing for mov files
>
> What would be a glob-pattern for filenames ending in `mov` in the nested folders of `video_dir`
> 
> > ## Solution
> > `C:\Users\username\Desktop\amia19\**\*mov`
> > or
> > `/Users/username/Desktop/amia19/**/*mov`
> {: .solution}
{: .challenge}

The above challenge points to common frustrations with file paths.
* How do you remember which kind of slash to use?
* How do you write code that works on both Windows and Mac paths?

The answer is to tell Python to figure out.

### `os.path`

The `os` module has an entire submodule devoted to working with paths, `os.path`. We can use `video_dir` to experiment.

We can find out if the file path is for a file or folder.
~~~
os.path.isfile(video_dir)
~~~
{: .language-python}

~~~
False
~~~
{: .output}

~~~
os.path.isdir(video_dir)
~~~
{: .language-python}

~~~
True
~~~
{: .output}

We can extract the relative path based on our current directory.
~~~
os.path.relpath(video_dir)
~~~
{: .language-python}

~~~
'amia19'
~~~
{: .output}

We can generate the absolute path, if we only have a relative path.
~~~
relpath = os.path.relpath(video_dir)
os.path.abspath(relpath)
~~~
{: .language-python}

~~~
'/Users/username/Desktop/amia19'
~~~
{: .output}


We can find the folder that holds the last piece of the filepath.
~~~
os.path.dirname(video_dir)
~~~
{: .language-python}

~~~
'/Users/username/Desktop/'
~~~
{: .output}

We can create a path based on components, and let `os.path` handle the syntax.
~~~
os.path.join(video_dir, 'nonexistent_file.gif')
~~~
{: .language-python}

~~~
('/Users/username/Desktop/amia19/nonexistent_file.gif')
~~~
{: .output}

~~~
badpath = os.path.join(video_dir, 'nonexistent_file.gif')
os.path.isfile(badpath)
~~~
{: .language-python}

~~~
False
~~~
{: .output}

> ## `os.path.join` and globbing
>
> How would you use `os.path.join` to create the glob pattern for mov files?
> 
> > ## Solution
> > ~~~
> > os.path.join(video_dir, '\*\*', '\*mov')
> > ~~~
> > {: .language-python}
> > Depending on your system, you should see
> > `C:\Users\username\Desktop\amia19\**\*mov`
> > or
> > `/Users/username/Desktop/amia19/**/*mov`
> {: .solution}
{: .challenge}

> ## Python Syntax: Functions with multiple arguments 
>
> If a function uses more than argument, each argument is separated by a comma.
> Arguments can be anything, including strings, lists, variables that hold strings, and other functions.
> However, functions can have rules for what they will accept.
> For example, using a list as arguments in `os.path.join` returns an error.
> ~~~
> os.path.join(['Desktop', 'amia19'])
> ~~~
> {: .language-python}
>
> ~~~
> ...
> TypeError: expected str, bytes or os.PathLike object, not list
> ~~~
> {: .output}
>
> But using string, returns the result we want.
>
> ~~~
> os.path.join(['Desktop', 'amia19'])
> ~~~
> {: .language-python}
{: .callout}

Finally, we can combine `glob.glob()` and `os.path.join()` to generate our list of files.
In order to search recursively through nested folders, we need to give an extra argument to `glob.glob()`, `recursive=True`.

~~~
mov_list = glob.glob(os.path.join(video_dir, "**", "*mov"), recursive=True)
~~~
{: .language-python}

> ## How many files?
>
> How many files were found using `glob.glob`?
> Hint, calculate the length of the list.
> 
> > ## Solution
> > ~~~
> > len(mov_list)
> > ~~~
> > {: .language-python}
> {: .solution}
{: .challenge}

## Another strategy for generating a file list

`glob.glob` does one job, find filepaths, really well.
It can't do other jobs, like count the number of subdirectories or find all the files that don't match a particular pattern.
If you need to do additional filtering, counting, or other operations you need to use other strategies.

The following code generates the same list as our `glob.glob` but uses 6 lines instead of 1.
Under-the-hood, `glob.glob` is doing a very similar set of steps as this code.
However, it can be useful to do each steps yourself in case you want to perform additional actions while generating lists of files.

~~~
mov_list = []
for root, dirs, files in os.walk(video_dir):
    for file in files:
        if file.endswith('.mov'):
            item_path = os.path.join(root, file)
            mov_list.append(item_path)
~~~
{: .language-python}

We don't need to fully understand this code right now.
It uses concepts like `for` loops and `if` conditionals that will be covered in upcoming lessons.
For now, it's a useful exercise to see if you can interpret the gist of what is happening.

> ## What does the above code do?
>
> Go through each line of the code above.
> What do you think happens on each line?
{: .challenge}




