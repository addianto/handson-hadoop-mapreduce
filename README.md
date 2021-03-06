# Hands-on: Hadoop & MapReduce

> Practical exercises on Hadoop, HDFS, and MapReduce as part of Data Mining for
> Big Data short course program at Pusilkom UI.

This document describes the hands-on exercises for the participants that
Enrolled in **Data Mining for Big Data** training at
[Pusilkom UI](https://training.pusilkom.com/portal/). Each participant will
follow through the exercises to gain experience in using Hadoop and to
know MapReduce programming model.

## Table of Contents

1. [Prerequisites](#prerequisites)
2. [Getting Started](#getting-started)
3. [Managing Files in HDFS](#managing-files-in-hdfs)
4. [Trying a MapReduce Program](#trying-a-mapreduce-program)

## Prerequisites

In order to have a smooth experience in following the hands-on, please
ensure that you have installed the required software as follows:

- Modern Web browser, e.g. [Mozilla Firefox](https://www.mozilla.org/firefox/),
  [Google Chrome](https://www.google.com/chrome)
- Text editor other than Notepad, Wordpad, and Microsoft Word, e.g.
  [Visual Studio Code](https://code.visualstudio.com),
  [Notepad++](https://notepad-plus-plus.org)
- (Optional) FTP client, e.g. [FileZilla Client](https://filezilla-project.org),
  [WinSCP](https://winscp.net)
- (Optional) SSH client, e.g. [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)

Some basic proficiency with command-line interface (CLI) is required. If you
have ever used `cmd` command prompt in Windows or `bash` shell in Mac OS/
Linux-based OS, then you can follow the exercises just fine. Otherwise, you
might want to follow the mini exercises at the end of [Getting Started](#getting-started)
section.

> [Back to ToC](#table-of-contents)

## Getting Started

During this session, most of the activities will be about interacting with the
Hadoop Cluster via the master node. The master node provides tools for managing
files in HDFS and submitting MapReduce-based program that will use files in
HDFS as input.

There are two methods to access the master node:

- Use native SSH client such as PuTTY (Windows) or `ssh` (Mac OS/Linux-based
  OS) to connect to the master node available at the following host/address:
  `10.119.234.100`
- Use browser-based shell, e.g. instance of shell-in-a-box that provided when
  you access https://bigdata.pusilkom.com using Web browser

Ask the instructor(s) to obtain your credential to access the master node.
Usually the credentials are provided at the beginning of the training session.
Once you have obtained the credential, try to connect to the master node using
either approach. You should see the CLI/shell similar to the following screenshot:

![Shell's display once connected to the master node](images/example_shell_01.png)

Congratulations! You are now connected to the master node of a Hadoop Cluster.
If you are not familiar with CLI/shell or new to the shell in Linux-based OS, try
to follow the mini exercises below before proceeding to the next exercise.
Otherwise, feel free to skip directly to [Managing Files in HDFS](#managing-files-in-hdfs)
section.

> Note: Most commands in the shell has the following invocation pattern:
> `<COMMAND> [<OPTIONS>] <ARGS>` where:
> - `<COMMAND>` is the command, e.g. `pwd`, `ls`, `cd`
> - `[<OPTIONS>]` is optional list of options that will change how the command
>   behaves, e.g. `-l` option when calling `ls`
> - `<ARGS>` is list of arguments/parameters that will be read by the command to
>   perform its functionality, e.g. `mkdir foo` where `foo` is a single value
>   passed as an argument to `mkdir` command

1. `pwd`

    ```bash
    $ pwd
    ```
    > - Question #1: What does `pwd` stand for?

2. `ls`

    ```bash
    $ ls
    $ ls -a
    $ ls -a -l
    $ ls -a -l -h
    ```
    > - Question #1: What does `ls` perform?

3. `mkdir` and `rmdir`

    ```bash
    $ mkdir foo bar
    $ ls -l
    $ mkdir baz
    $ ls -l
    $ rmdir bar
    $ ls -l
    $ mkdir -p nasi/goreng/teri
    $ ls -l -R
    ```
    > - Question #1: What does `mkdir` and `rmdir` stand for?
    > - Hint: Many commands in the shell are abbreviation of name of actions that
    >   user can do when operating their computer.

4. `cd`

    ```bash
    $ pwd
    $ cd foo
    $ cd ..
    $ pwd
    $ cd nasi/goreng
    $ pwd
    $ cd ../..
    $ cd nasi/goreng
    $ pwd
    $ cd ~
    $ pwd
    ```

    > - Question #1: What does `cd` perform?
    > - Question #2: What happened when you use `~` as an argument when calling
    >  `cd`?

5. `nano`

    ```bash
    $ nano a_file.txt
    # Write some text in the CLI program
    # You can save your work by pressing CTRL-O buttons.
    # You can exit from nano by pressing CTRL-X buttons.
    ```

    > Note: `nano` is a text editor program that runs in CLI. Pretty often we
    > do not have access to graphical user interface (GUI) when operating a
    > computer/server that runs somewhere on the Internet. Therefore, it is
    > really important to get familiar with commands and CLI-based programs
    > in the shell.

6. `cat`

    ```bash
    $ cat a_file.txt
    ```

    > - Question #1: What is the output of `cat` invocation above?

7. `cp`

    ```bash
    $ cp a_file.txt b_file.txt
    $ ls -l
    $ cp a_file.txt foo/a_file.txt
    $ ls -l -R
    ```

    > - Question #1: What does `cp` perform?

8. `rm`

    ```bash
    $ ls -l
    $ rm a_file.txt
    $ ls -l
    ```

    > Question #1: What happened to `a_file.txt` file?

Good job for reaching the end of the mini exercises! Now you have the basic
knowledge on using basic commands that you can call when interacting with a
Linux-based computer. In this case, the master node is actually a Linux-based
server and all exercises in the subsequent sections will require you to call
commands in the shell. You can proceed to the next section.

> [Back to ToC](#table-of-contents)

## Managing Files in HDFS

Contrary to the traditional programming model where we try to **bring the data into the program**,
MapReduce programming model does the opposite, which is to **bring the program into the data**.
The data itself are also distributed across multiple nodes in the cluster.
Therefore, you need to know how to get the data from the cluster and to put the
data into the cluster using commands that interact with the Hadoop Distributed File
System (HDFS). Once you are able to manage your data, you can run a MapReduce
program to perform computation on the distributed data.

You need to know several basic commands provided by basic Hadoop
installation in the master node to manage data files in the Hadoop Cluster.
The commands are similar to basic shell commands on Linux-based OS. The
complete list of commands can be read in the following reference:
https://hadoop.apache.org/docs/r2.7.3/hadoop-project-dist/hadoop-common/FileSystemShell.html

The following is several examples of basic commands that you will
frequently use when interacting with the HDFS.

- Get the list of files in HDFS:

    ```bash
    $ hadoop fs -ls
    $ hadoop fs -ls /foo/bar
    ```

    > Explanation: First example will get list of files in the root (topmost)
    > directory in the HDFS. Second example will get list of files in `/bar`
    > directory that is present under `/foo` directory in the root of HDFS.

- Copy a file from local machine (server/master node) into HDFS:

    ```bash
    $ hadoop fs -copyFromLocal a_file.txt
    # Alternative
    $ hadoop fs -put a_file.txt
    ```

    > Explanation: The example above will copy `a_file.txt` file from current
    > active directory in local machine into the root directory in the HDFS.

- Copy a file within HDFS:

    ```bash
    $ hadoop fs -cp a_file.txt b_file.txt
    ```

    > - Question #1: What does `hadoop fs -cp <ARG_1> <ARG_2>` do?
    > - Hint: Try calling `hadoop fs -ls` after executed the command above

- Copy file from HDFS into local machine:

    ```bash
    $ hadoop fs -copyToLocal b_file.txt
    # Alternative
    $ hadoop fs -get b_file.txt
    ```

    > - Question #1: Is it possible to copy a file from HDFS and give it
    >   a different name in the local machine in single `hadoop fs` execution?
    >   How?

- Display content of a file in the HDFS into the shell:

    ```bash
    $ hadoop fs -cat a_file.txt
    ```

- Remove a file in the HDFS:

    ```bash
    $ hadoop fs -rm a_file.txt
    ```

> [Back to ToC](#table-of-contents)

## Trying a MapReduce Program

Basic Hadoop installation provides several MapReduce program examples. You can
see the list of program examples by executing the following commands:

```bash
# Change current active directory to your home directory
$ cd ~
# Copy the program collection into current directory
$ cp /opt/hadoop-2.7.3/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.3.jar .
# Run the program to see list of available MapReduce program examples
$ hadoop jar hadoop-mapreduce-examples-2.7.3.jar
```

In this session, we will try to run a MapReduce program called `wordcount`. You can
see the options and arguments required by the program by executing the following command:

```bash
$ hadoop jar hadoop-mapreduce-examples-2.7.3.jar wordcount
```

The `wordcount` program requires a list of arguments where the first arguments
are the input files and the last argument is a path to the output directory.
The input file(s) must be put into the HDFS before can be processed by the program.
Therefore, you need to copy all the input file(s) into the HDFS before running the
MapReduce program.

Once you have copied all the input files, you can run the `wordcount` MapReduce
program by executing the following command:

```bash
$ hadoop jar hadoop-mapreduce-examples-2.7.3.jar wordcount input/input1.txt input/input2.txt output
```

> Explanation: The `wordcount` program will use `input1.txt` and `input2.txt` in
> `input` directory at HDFS as the input data files. The computation results will
> be written into a new folder called `output` in the HDFS.

Congratulations! You just run your first MapReduce program on Hadoop! Now try to
copy the results from HDFS to your own directory in local machine and see the
content of each result files.

> [Back to ToC](#table-of-contents)

***

## Contributors

- Samuel Louvan
- Remmy Augusta Menzata Zen
- Ardhi Putra Pratama Hartono
- Daya Adianto

## License

This document is licensed under Creative Commons Attribution-ShareAlike 4.0
International ([CC-BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)).
