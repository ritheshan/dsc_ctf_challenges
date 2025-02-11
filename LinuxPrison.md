Here’s a unique CTF write-up for the Linux Prison 3 challenge with alternative trial-and-error conditions while maintaining clarity and structure.

CTF Write-up: Linux Prison 3 (hearsay3.ctf.dscjssstuniv.in) 🚀🔐🐧

Challenge Overview 🎯

The third installment in the Linux Prison series provided SSH access to a remote machine. The goal was to extract the flag despite system restrictions.

Connection Details 🖥️

ssh ctf@hearsay3.ctf.dscjssstuniv.in -p 9103  
Password: ctf  

Once logged in, the next step was enumeration to explore the available files and potential access limitations.

Enumeration & Initial Exploration 🔍📂

Listing the files in the current directory:

ls  

Revealed files:

README.txt  
flag.txt  
flags  
more_flags  

Since flag.txt seemed to be the primary target, the first instinct was to display its contents using common commands.

Flag Extraction: Trial & Error Attempts 🛠️

Attempt 1: Using cat 📜

cat flag.txt  

Result: ❌ Failed
Error Message: -rbash: cat: command not found
Reason: The system was running restricted bash (rbash), blocking certain commands.

Attempt 2: Using less & more 📖

less flag.txt  
more flag.txt  

Result: ❌ Both commands were restricted.

Attempt 3: Using echo for a Workaround 📢

echo "$(flag.txt)"  

Result: ❌ No output; the method failed since flag.txt is not an environment variable.

Attempt 4: Checking Available Commands 🧐

Since some commands were blocked, it was useful to see what commands were permitted:

compgen -c  

This provided a list of allowed commands, revealing that bash was available, which led to the next approach.

Attempt 5: Executing the File with bash 🏆

bash flag.txt  

Result: ✅ Success!
Flag Revealed: dscctf{cat_not_working_huh}

Key Findings & Lessons Learned 🧠
	1.	Restricted Shell Bypass 🚧
	•	rbash blocks common commands like cat, ls, and more, requiring alternative methods.
	•	Executing the file with bash worked because it allowed script execution even when direct reading was restricted.
	2.	Checking Available Commands 🔎
	•	Using compgen -c helped identify allowed commands, guiding the workaround approach.
	3.	Alternative Approaches 🤔
	•	In a real-world scenario, additional techniques could include:
	•	Using awk or sed (if available).
	•	Copying the file to a different location with looser restrictions.
	•	Exploiting environment variables or symbolic links.

Final Flag 🏁

dscctf{cat_not_working_huh}

This write-up demonstrated a different trial-and-error approach to obtaining the flag by experimenting with command restrictions and bypassing them effectively. 🎯🔥