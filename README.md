Download Link: https://assignmentchef.com/product/solved-tdt4186-exercises-5-unix-ipc
<br>



<h2>5.1        Pipeline performance</h2>

In this exercise, you are going to measure the <em>bandwidth </em>provided by Unix IPC methods.

In order to do this, write a benchmarking program. After creating a pipe, your program should split into two process:

<ul>

 <li>a child process that endlessly writes data blocks of the given size (the content of the data blocks is irrelevant) using write(2) over the given IPC method <em>as fast as possible </em>and</li>

 <li>a parent process that reads the data blocks of the same size <em>as fast as possible </em>using read(2) over the same IPC method.</li>

</ul>

<table>

 <tbody>

  <tr>

   <td width="55"></td>

  </tr>

  <tr>

   <td></td>

   <td></td>

  </tr>

 </tbody>

</table>

Your program should accept a block size (number of bytes) of the data blocks to be sent from child to parent as its only command line parameter.

<h3>a. Pipeline functionality</h3>

Write the program using <em>unnamed pipes </em>(system call pipe(2)) and output the cumulative number of received bytes after each read call of the parent process.

<h3>b. Performance</h3>

Use the alarm(3) libc function to trigger a Unix <em>signal handler </em>for the SIGALRM signal once a second.

Implement the signal handler so that it prints the current pipeline <em>bandwidth</em>, i.e. the number of bytes received in the previous second. See the signal(3) and alarm(3) man pages for details.

Test your program for different block sizes (powers of ten), so 1 byte, 10 bytes, 100 bytes, . . . up to the maximum block size possible on your system.

Investigate the following questions:

<ul>

 <li>What is the largest block size supported on your system?</li>

 <li>What is the highest bandwidth you can achieve and at which block size is this bandwidth achieved?</li>

 <li>Does the bandwidth change when you start several instances of your program at the same time?</li>

</ul>

<em>Hint 1: </em>Setting up a signal handler requires you to pass a <em>function pointer</em>. If you are unsure how to use function pointers, you can refer to the tutorial at <a href="https://www.cprogramming.com/tutorial/function-pointers.html">https://www.cprogramming.com/tutorial/function-pointers. </a><a href="https://www.cprogramming.com/tutorial/function-pointers.html">html</a><a href="https://www.cprogramming.com/tutorial/function-pointers.html">.</a>

<em>Hint 2: </em>Comment out the print function that outputs the cumulative number of bytes from part a, otherwise the throughput measured will be off, since the parent would not receive as fast as possible.

<h3>c. Trigger statistics printing</h3>

Register and write an additional signal handler to handle the SIGUSR1 signal. You can trigger the signal by executing kill -s USR1 pid in a shell, where pid is the process ID of the parent process of your benchmark program. In response to receiving the signal, in the corresponding signal handler, your program should print the <em>cumulative </em>number of bytes received over the pipe so far.

Your program should continue to run after printing the information.

<table>

 <tbody>

  <tr>

   <td width="55"></td>

  </tr>

  <tr>

   <td></td>

   <td></td>

  </tr>

 </tbody>

</table>

<h3>d. Named pipes Create a variant of your program that uses <em>named pipes </em>(see the mkfifo man page) instead of unnamed pipes and repeat the measurements from part b.</h3>

<em>Hint: </em>Open the named pipe (FIFO) file (using open(2) and specifying either O_RDONLY or O_WRONLY as appropriate) only after executing fork(2). If your program seems to hang when running it a second time, delete the created FIFO file from the file system using the rm command in the shell or by calling the unlink(2) system call in your program before calling mkfifo.