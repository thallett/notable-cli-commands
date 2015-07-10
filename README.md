# notable-cli-commands
Notable Linux Command Line Commands

Convert Ascii Hex to Binary 
cat $@ | tr -d "\t\r\n" | sed 's/ //g' | sed 's/../&\n/g' | perl -e 'while(<>){print chr((hex($_)));}';
#As .bashrc function
function tohex() { cat $@ | tr -d "\t\r\n" | sed 's/ //g' | sed 's/../&\n/g' | perl -e 'while(<>){print
chr((hex($_)));}'; }
#Usage:echo "ABCD" | tohex | hexdump -C
00000000 ab cd |..|
00000002

Date Command Helpful Help
alias dateh='date --help|sed -n "/^ *%%/,/^ *%Z/p"|while read l;do date +\ %${l/% */}:\|" |${l/% */}|$
{l#* }";done|column -s"|" -t'

Make Fifo file
mkfifo /tmp/debug_file.log
cat /tmp/debug_file.log > /nfs/trace.log &  #Don't forget the "&" to send this to the background

Let someone watch your shell session
script -f >(nc -l -p 5000) #On Host
telnet host 5000  #On Watcher

Pipe Through SSH to dump to file
grep -A 5 "something" * | \grep -ho "\->.*," | ssh user@hostname "tee -a /tmp/tim.txt"

Look Busy 
for f in `seq 0 100`;do echo $f | dialog --backtitle "Package Installation" --title "Package Installation
Status" --gauge "Installing..." 6 40; sleep 10; done 

Start VNC Server for Display :0.0 
x0vncserver PasswordFile=/home/username/.vnc/passwd display=:0.0 

Send Xmessage to Main X Console from Remote System 
ssh user@hostname xmessage -display :0.0 -center "Test Message" 

Tar through ssh if you can't access companion 
tar cf - /nfs/test.log | ssh user@hostname "(cd /tmp; tar xf -)" 

Spy on a shell process 
ps aux | grep sh 
strace -ff -e write=1,2 -s 1024 -p $PID 2>&1 | grep "^ |" 

Convert Number between Hex & Decimal
function hex2dec(){ printf "%d\n" $1; }
function dec2hex(){ printf "0x%x\n" $1; }

Run a command with no output dumped to screen
nohup cat /tmp/tgh.txt
function silent(){ $@ > /dev/null 2>&1; }

Diff Output of 2 commands
gvimdiff <(cat test.txt | grep tmp) <(cat test2.txt | sed 's/tom/tmp/g' | grep tmp);
#As .bashrc function
function fdiff(){ gvimdiff <($1 $2) <($1 $3); }  #Easier to make it a function, but less flexible

Fix output so it goes into a nice table format
col

Remove (Binary) Control Characters from a file for grep-ping
cat somefile.txt | strings | grep “xx”

Split Pipe into multiple pipes to work on 
echo "tee can split a pipe in two"|tee >(rev) >(tr ' ' '_') 

Get Assembly from a executable 
objdump -b binary -m i386 -D shellcode.bin 

Simulate Typing 
dmesg|while read x;do for((i=0;i<${#x};i++));do echo -n "${x:$i:1}";sleep .06;done;echo;done 
tail -f /tmp/ppp |while read x;do for((i=0;i<${#x};i++));do echo -n "${x:$i:1}";sleep .01;done;echo;done 

Snowstorm 
while true ; do NUM=$(( $RANDOM % 80 )) ; for i in $( seq 1 $NUM ) ; do echo -n " " ; done ; echo \* ; done 

Run Command(s) Once, at Given Time in the future 
at 5:00 
at> Fix -complete -defect 731721 -release fips720 -component esw_vadj 
at> Fix -complete -defect 731721 -release fips720 -component esw_powr 
at> <EOT> 
job 50 at 2009-11-20 05:00 
at -l #List AT job(s) 
atrm 50 # Terminate AT job 

Remote Telnet Port Forward (FSP=10.10.10.10, behind system hostname) 
ssh -L 12345:10.10.10.10:23 user@hostname 
telnet localhost 12345 

Process Tree 
psu(){ command ps -Hcl -F S f -u ${1:-$USER}; } 

Random Hex Digits 
ran=$(head /dev/urandom | md5sum); echo ${ran:0:2}:${ran:3:2}:${ran:5:2}:${ran:7:2}; 

Reuse all parameters of the previous command line 
!* 

Diff output of 2 commands w/o using temp files 
b=$(grep Tim .vimrc); a=$(grep "Tim\|expand" .vimrc); gvimdiff <(echo "$a") <(echo "$b") 

SCP back to Home Dir on Host SSH'd in from 
scp /path/to/file ${SSH_CLIENT%% *}: 

Grab & Display Screenshot 
import screenshot.jpg && display screenshot.jpg 

Histogram Graph on the Command Line 
\grep somefile.txt | \grep -o " ..:.." | sed 's/:/ /g' | sed 's/^ .. //g' | sort | uniq -c
| awk '{ printf("%s\t%s\t",$2,$1) ; for (i = 0; i < $1; i++) {printf("*")}; print "" }' 

Quickly graph list of numbers 
gnuplot -persist <(echo "plot '<(sort -n listOfNumbers.txt)' with lines") 

Recurse through subdirs, run command in every subdir 
for f in $(find ./ -type d); do pushd $f; pwd; popd; done 

Ddate - Discordian Calendar
ddate 

Display a block of text with AWK 
awk '/start_pattern/,/stop_pattern/' file.txt 

Base conversions with bc 
echo "obase=2; 27" | bc -l 

Display the standard deviation of a column of numbers with awk 
awk '{sum+=$1; sumsq+=$1*$1} END {print sqrt(sumsq/NR - (sum/NR)**2)}' file.dat 

Averaging Columns of Numbers 
awk '{sum1+=$1; sum2+=$2} END {print sum1/NR, sum2/NR}' file.dat 

Stopwatch 
time cat 
<Ctrl+D> to stop 

Put Term into VI mode 
set -o vinet 

Display bytes that are different in hex 
cmp -l file1.bin file2.bin | awk '{print $1, "0"$2, "0"$3}' | gawk --non-decimal-data '{printf("0x%08x 0x%02x 0x
%02x\n",$1,$2,$3);}' 

Display bytes that are different in hex & ascii 
cmp -l file1.bin file2.bin | awk '{netprint $1, "0"$2, "0"$3}' | gawk --non-decimal-data '{printf("0x%08x 0x
%02x0x%02x %d %d\n",$1,$2,$3,$2,$3);}' | gawk --non-decimal-data '{printf("0x%08x 0x%02x 0x%02x %c %c\n",
$1,$2,$3,$4,$5);}' | more 

Count bytes that are different between two binary files 
cmp -l tpmd_dump.bin tpmd_fips710_hv32_3042.bin | wc -l 

All Output to file 
cat file > startx.log 2>&1 

Disown a background process so it doesn't close after shell closes 
<Ctrl+Z> 
bgnet 
disown 

Merge *.pdf Files 
gs -q -sPAPERSIZE=letter -dNOPAUSE -dBATCH -sDEVICE=pdfwrite -sOutputFile=out.pdf `ls *.pdf` 

Rapidly invoke an editor to write a long command 
<Ctrl+x> <Ctrl+e> 

Mount a Temporary RAM partition 
mount -t tmpfs tmpfs /mnt -o sinetze=1024m 

Processes by CPU usage 
ps -e -o pcpu,cpu,nice,state,cputime,args --sort pcpu | sed "/^ 0.0 /d" 

SSH connection through host in the middle 
ssh -t reachable_host ssh unreachable_host 

Screen Log Sessions 
screen -D -R -L 

Pipe to Vim 
cat foo.txt | vim - 
cat foo.txt | vim -R - 

Save a file you edited in vim without the needed permissions 
:w !sudo tee % 

Get List of all Keyboard Shortcuts in Screen 
Ctrl+A ? 

Background a SSH-session: 
RETURN~& 

Close a SSH-session: 
RETURN~. 

Show other options:net 
RETURN~? 

Create script of last command 
echo "!!" > foo.sh 

Fork Bomb 
:(){ :|:& };: 

Reconnect to screen w/o disconnecting 
screen -xR 

Edit over SCP 
vim scp://username@host//path/to/somefile 

Open on port 6506 & dump *.txt on connection 
nc -w 10 -v -l -p 6506 < ./public/somefile.txt 

Empty the linux buffer cache 
sync && echo 3 > /proc/sys/vm/drop_caches 

Dump output to nohup.out & background & dont' kill if detached session 
nohup <command> & 

Run last command as root 
sudo !! 

Get list of programs listening on ports 
netstat -plunt 

Run "unaliased" rm 
\rm 

Wham! Reboot 
echo 1 > /proc/sys/kernel/sysrq; echo b > /proc/sysrq-trigger 

Show lines that appear in both file1 & file2 
comm -1 -2 <(sort file1) <(sort file2) 

Watch video in terminal 
mplayer -vo caca out.mpg 
Top 10 Memory Hogs 
ps aux | sort -nk +4 | tail 

Backdoor to Allow Remote Shell 
nc -vv -l -p 1234 -e /bin/bash 
connect with: nc localhost 1234 

Browse RAM 
sudo cat /proc/kcore | strings | awk 'length > 20' | less 

Remote Screen 
ssh -t user@some.domain.com /usr/bin/screen -xRR 

Binary Injection 
echo -n $HEXBYTES | xxd -r -p | dd of=$FILE seek=$((0x$OFFSET)) bs=1 count=$NUMBYTES conv=notrunc 
Replace (as opposed to insert) hex opcodes, data, breakpoints, etc. without opening a hex editor. 
HEXBYTES contains the hex you want to inject in ascii form (e.g. 31c0) 
NUMBYTES is the number of bytes in HEXBYTES (in the previous example, this would be 2) 
OFFSET is the hex offset (e.g. 49cf) into the binary FILE 
Edit: The count argument to dd can be omitted.
