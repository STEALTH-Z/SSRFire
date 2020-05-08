# SSRFX
An automated SSRF finder. Just give the domain name and your server and chill! ;)
It also has an option to find open redirects.

![SSRFX](https://github.com/michaelben6/SSRFX/blob/master/static/ssrfx.png)

### Syntax  
./ssrfx.sh domain.com yourserver.com 

**domain.com**     ---> The website you want to test

**yourserver.com** ---> Your server which logs any incoming traffic Eg. Burp colloborator

### Requirements
Since this uses GAU, FFUF and OpenRedirex, you need GO and python 3.7+. You need not have the tools installed, as the script **setup.sh** would install everything. 
You just need to install python and GO.
Even if you have the tools installed I would highly recommend you to install them again so that there no conflicts while setting the paths.

If you don't want to install the tools again, paste this code in your .profile in your home directory and source .profile them.
Also, you have to make a small change in the ssrfx.sh on line 10, where you have to replace source /home/hari/.profile without 
your .profile path. **(Only if you are not installing tools through setup.sh)**
```
#Replace /path/to/ with the specific directory where the tool is installed
#If you already have configured paths for any of the tools, replace that code with the below one.
ffuf(){
        echo "Usage: ffuf https://www.domain.com/FUZZ payloads.txt"
        /path/to/ffuf/./main -u $1 -w $2 -c -t 100
}

gau(){
        echo "Usage: gau domain.com"
        /path/to/gau/./main $1
}

openredirex(){
        echo "Usage: openredirex urls.txt payloads.txt"
        python3 /path/to/OpenRedireX/openredirex.py -l $1 -p $2 --keyword FUZZ
}
```
## Usage
```
chmod +x setup.sh
./setup.sh (preferably yes for all ---> **highly recommended**)
./ssrfx.sh domain.com yourserver.com 
```
### Finding SSRF
Now, gau gets into action by fetching all the URLs of the domain. This may take a lot of time. 
You can check the output generated till now at output/domain.com/raw_urls.txt

Let it run for at least 10-15 minutes, and then if you want to continue, you can.
But if you want to test the URLs fetched till now, quit the process. 
Copy the raw_urls.txt inside of output/domain.com and place it outside the domain.com folder
Now run
```
./ssrfx.sh domain.com yourserver.com /path/to/copied_raw_urls.txt
```
Select yes when asked whether to delete the existing folder.

This will skip the process of GAU fetching URLs.

Now the all the URLs with the parameters will be filtered and yourserver.com will be placed into their parameter values.(final_urls.txt)

The next step is to fire request to all the final URLs. This makes use of an asynchronous python script for sending faster requests.

### Finding open redirects

You can choose ffuf or openredirex for this process.
I personally prefer openredirex, as it is specifically designed to check for open redirects by loading the URLs from the list 
and it looks a lot cleaner, and doesn't flood your terminal.

## Tools used:

GAU - [https://github.com/lc/gau](https://github.com/lc/gau)

ffuf - [https://github.com/ffuf/ffuf](https://github.com/ffuf/ffuf)

OpenRedireX - [https://github.com/devanshbatham/OpenRedireX](https://github.com/devanshbatham/OpenRedireX)

Thanks to all the authors of the tools.

***
Finally, if you are stuck in any of the above processes, you can always reach me at [twitter](https://twitter.com/michael63385254)

