# SharpWatchdogs

SharpWatchdogs is a program designed to watch other processes. The idea behind this code is to provide persistence in compromised hosts for Red Blue competitions.

## DISCLAIMERS
The information contained in this blog post is for educational purposes ONLY! The author and RITSEC DO NOT hold any responsibility for any misuse or damage of the information provided in blog posts, discussions, activities, or exercises. 

## About
SharpWatchdogs is written in C#. It's meant to add another layer of persistence where the host is fully compromised and has already intense persistent. It's designed to work with other techniques like Rootkits, not only by itself. 

## How to use
SharpWatchdogs can be a DLL, service, process, or all of them running and injected at the same time. An individual incident loads its configurations from the file called lol.dll, then start scanning the running processes every some random time defined in the execution time. When it does not find one of the predefined processes running, it starts it again and continues watching. The trick is to run multiple forms of this code, for example, a form where it's a service, another form as a regular process, and another as an injected DLL into another regular process. Thus, every process will watch all the other processes. Sure enough, running them without pre-established persistence like rootkits will not be sufficient at all. An ideal usage will be launching all the instances (dogs) after hiding all the red team processes. That way, they will start their work by keeping all the shells connected and well working, even if a blue teamer deleted/killed one of the shells, one of the dogs will redownload and run the missing shell.

The screenshot below shows many instances running. Each individual instance - Dog - is numbered with a different number. Their objective is to watch each other, and at the same time, their main objective is to keep the Dummy processes always running. In a real word engagement, each "dog" will have a legitimate name and placed in different locations, and the dummy processes will be the red team shells/malware.

![example](http://mohad.red/images/sharpwatchdogs/w1.png)

## Blue side
Getting rid of all the dogs is actually easy. As I mentioned, they by themselves are not dangerous; the main focus should the services running them and the rootkits hiding them. The main goal must be removing the installed rootkits so you can find the running services and other persistence techniques and stop them then restart the server. If restarting the server is not an option, after stopping what might hide them, you can start process explorer, then find the main processes, the processes that are responsible for running the rest dogs like Dog1.exe, Dog9.exe, and Dog16.exe in the above screenshot, and kill all the trees. And that is, of course, after making sure that none of them will run again because if just one instance got executed again in any way, all the other would run. Another way is to find where they all get the data from (text.dll) then remove the documented paths.

