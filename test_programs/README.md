# Test Programs for Graceful Shutdown
The files in this directory are used to exercise and demonstrate the capabilities of the Graceful Shutdown implementation. These files will be built into executables by the initial ```make all``` command, and executed by running the ```run_program_with_gs.sh``` script.

If any files need to be edited and rebuilt, the commands

```make clean```

will clean all generated files (executables and text files ouput from running tests) in the ```test_programs``` directory. To rebuild the files in the directory, run

```make all```

## Using the script to execute test programs
The script ```run_program_with_gs.sh``` is used to execute each of the test programs. This script automates all of the steps a user would need to take to assign a graceful shutdown process to a running process.

Usage of the script is as follows:

```./run_program_with_gs.sh <oom watch file> <graceful shutdown file>```

where both the ```<oom watch file>``` and the ```<graceful shutdown file``` are full file paths. The script will then start the file which will be tagged as the process to watch for the oom condition, grab it's PID, and echo the PID and the path of the graceful shutdown program into the /proc/graceful_shutdown file, just as a user would need to do in actual use.

```<PID> <graceful shutdown file>```

Finally, the script will execute the ```<oom watch file>``` in the limited memory cgroup, which contains large memory leaks so that there is still memory for the graceful shutdown file to run in.

## OOM Condition Creator Program
This is the file that is used for the test programs to create out of memory conditions. This file begins and then waits for the user to press the ```Enter``` key so that during testing there is enough time for the user to inspect the PID of the created process. Once the user presses the ```Enter``` key, the file will allocate very large blocks of memory infinitely. This will eventually cause the OOM condition to occur.

## Testing File Create/Write
This test uses the program ```write_to_file_gs``` as the graceful shutdown program. When called, it creates a file named "graceful_shutdown_output.txt" in the ```test_programs``` directory with a simple sentence within. The path and name of the file are hardcoded inside of the test file, so in order to change this path to match your own user directory you must open the source file, edit, close, and re-make before executing. The command to execute this file will need to look something like the following:

```./run_program_with_gs.sh /<full>/<file>/<path>/Project/csci5573-project/test_programs/oom_condition_creator /<full>/<file>/<path>/Project/csci5573-project/test_programs/write_to_file_gs_test```

The program will start the ```oom_condition_creator``` program and pause and wait for the user to press ```Enter```. After that, the program will start allocating large blocks of memory until an OOM condition is created and the program is marked for shutdown. It will then call and execute the ```write_to_file_gs``` program which will create a file, "graceful_shutdown_output.txt" in the ```test_programs``` directory, which contains the following text:

```The oom killer was invoked, and this is a file generated as a result of the graceful shutdown```


## Testing File Read and Write
This test uses the program ```read_and_write_to_file_gs``` as the graceful shutdown program. When called, it line-by-line reads from a file named "read_from_test.txt" containing an except from Huckleberry Finn, and creates a file named "write_to_test.txt" in the ```test_programs``` directory which then contains a copy of the contents from "read_from_test.txt" file. The path and name of the file are hardcoded inside of the test file, so in order to change this path to match your own user directory you must open the source file, edit, close, and re-make before executing. The command to execute this file will need to look something like the following:

```./run_program_with_gs.sh /<full>/<file>/<path>/Project/csci5573-project/test_programs/oom_condition_creator /<full>/<file>/<path>/Project/csci5573-project/test_programs/read_and_write_to_file_gs_test```

The program will start the ```oom_condition_creator``` program and pause and wait for the user to press ```Enter```. After that, the program will start allocating large blocks of memory until an OOM condition is created and the program is marked for shutdown. It will then call and execute the ```read_and_write_to_file_gs``` program which will line-by-line read the "read_from_test.txt" file, and then create a file, "write_to_test.txt" in the ```test_programs``` directory, which then contains the same text as the file which was read in, both contain the following text:

```CHAPTER 1```

```DISCOVER MOSES AND THE BULRUSHERS```

```You don't know about me without you have read a book by the name of The Adventures of Tom Sawyer; but that ain't no matter. That book was made by Mr. Mark Twain, and he told the truth, mainly. There was things which he stretched, but mainly he told the truth. That is nothing. I never seen anybody but lied one time or another, without it was Aunt Polly, or the widow, or maybe Mary. Aunt Polly--Tom's Aunt Polly, she is--and Mary, and the Widow Douglas is all told about in that book, which is mostly a true book, with some stretchers, as I said before.```

```Now the way that the book winds up is this: Tom and me found the money that the robbers hid in the cave, and it made us rich. We got six thousand dollars apiece--all gold. It was an awful sight of money when it was piled up. Well, Judge Thatcher he took it and put it out at interest, and it fetched us a dollar a day apiece all the year round--more than a body could tell what to do with. The Widow Douglas she took me for her son, and allowed she would sivilize me; but it was rough living in the house all the time, considering how dismal regular and decent the widow was in all her ways; and so when I couldn't stand it no longer I lit out. I got into my old rags and my sugar-hogshead again, and was free and satisfied. But Tom Sawyer he hunted me up and said he was going to start a band of robbers, and I might join if I would go back to the widow and be respectable. So I went back.```


## Testing Server Connectivity
*This test must be run as root - not using sudo*. This test must be run as root because the functionality for pinging the server in the test file is not accessible to users other than root. The server test uses the program ```ping_server_gs_test``` as the graceful shutdown program. When called, it creates a file named "ping_output_gs.txt" in the ```test_programs``` directory. The program then pings the "google.com" server 10 times with a 0.01ms sleep between pings. The number of pings and the ping sleep values can be configured inside the source file. The path and name of the file are hardcoded inside of the test file as well.TO change any of these values you must open the source file, edit, close, and re-make before executing. The command to execute this file will need to look something like the following:

```./run_program_with_gs.sh /<full>/<file>/<path>/Project/csci5573-project/test_programs/oom_condition_creator /<full>/<file>/<path>/Project/csci5573-project/test_programs/ping_server_gs_test```

The program will start the ```oom_condition_creator``` program and pause and wait for the user to press ```Enter```. After that, the program will start allocating large blocks of memory until an OOM condition is created and the program is marked for shutdown. It will then call and execute the ```write_to_file_gs``` program which will create a file, "ping_output_gs.txt" in the ```test_programs``` directory, which contains somethig similar to the the following text:

```PING OUTPUT FROM GRACEFUL SHUTDOWN TEST```

```64 bytes from den02s01-in-f14.1e100.net (h: google.com) (172.217.11.238) msg_seq=1 ttl=64 rtt = 26.602555 ms.```

```64 bytes from den02s01-in-f14.1e100.net (h: google.com) (172.217.11.238) msg_seq=2 ttl=64 rtt = 28.284108 ms.```

```64 bytes from den02s01-in-f14.1e100.net (h: google.com) (172.217.11.238) msg_seq=3 ttl=64 rtt = 25.787927 ms.```

```64 bytes from den02s01-in-f14.1e100.net (h: google.com) (172.217.11.238) msg_seq=4 ttl=64 rtt = 23.470388 ms.```

```64 bytes from den02s01-in-f14.1e100.net (h: google.com) (172.217.11.238) msg_seq=5 ttl=64 rtt = 36.908389 ms.```

```64 bytes from den02s01-in-f14.1e100.net (h: google.com) (172.217.11.238) msg_seq=6 ttl=64 rtt = 23.713488 ms.```

```64 bytes from den02s01-in-f14.1e100.net (h: google.com) (172.217.11.238) msg_seq=7 ttl=64 rtt = 24.359937 ms.```

```64 bytes from den02s01-in-f14.1e100.net (h: google.com) (172.217.11.238) msg_seq=8 ttl=64 rtt = 24.107772 ms.```

```64 bytes from den02s01-in-f14.1e100.net (h: google.com) (172.217.11.238) msg_seq=9 ttl=64 rtt = 24.311721 ms.```

```64 bytes from den02s01-in-f14.1e100.net (h: google.com) (172.217.11.238) msg_seq=10 ttl=64 rtt = 23.276582 ms.```


## Testing Access to Shared Data
