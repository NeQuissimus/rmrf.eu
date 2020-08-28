# Ad blocking on Mikrotik router
##### (Apr 25, 2017)
###### RouterOS

```bash
/system script add name="Squid_Blacklist" source="/tool fetch url=\"http://www.squidblacklist.org/downloads/tik-dns-ads.rsc\" dst-path=ads.rsc; /import file-name=ads.rsc;"

/system scheduler add comment="Squid_Blacklist" interval=1d name="DownloadSquidBlacklist" on-event=Squid_Blacklist start-date=jan/01/1970 start-time=20:00:00
```

---

# Wildcards in SSH known_hosts
##### (Oct 4, 2016)
###### Linux, OSX

```bash
github.com,192.30.252.*,192.30.253.*,192.30.254.*,192.30.255.* ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAq2A7hRGmdnm9tUDbO9IDSwBK6TbQa+PXYPCPy6rbTrTtw7PHkccKrpp0yVhp5HdEIcKr6pLlVDBfOLX9QUsyCOV0wzfjIJNlGEYsdlLJizHhbn2mUjvSAHQqZETYP81eFzLQNnPHt4EVVUh7VfDESU84KezmD5QlWpXLmvU31/yMf+Se8xhHTvKSCZIFImWwoG6mbUoWf9nzpIoaSjB+weqqUUmpaaasXVal72J+UX2B+2RPW3RcT0eOzQgqlJL3RKrTJvdsjE3JEAvGq3lGHSZXy28G3skua2SmVi/w4yCE6gbODqnTWlg7+wC604ydGXA8VJiS5ap43JXiUFFAaQ==
```

---

# List options current kernel was compiled with
##### (Sep 14, 2016)
###### Linux

```bash
zcat /proc/config.gz
```

--- 

# Largest MySQL tables
##### (Aug 12, 2016)
###### MySQL

```sql
SELECT CONCAT(table_schema, ".", table_name),
CONCAT(ROUND(table_rows / 1000000, 2), "M") rows, 
CONCAT(ROUND(data_length / ( 1024 * 1024 * 1024 ), 2), "G") DATA, 
CONCAT(ROUND(index_length / ( 1024 * 1024 * 1024 ), 2), "G") idx, 
CONCAT(ROUND(( data_length  + index_length ) / ( 1024 * 1024 * 1024 ), 2), "G") total_size, 
ROUND(index_length / data_length, 2) idxfrac 
FROM information_schema.TABLES 
ORDER BY data_length + index_length DESC 
LIMIT 10;
```

---

# Screen to open serial connection
##### (Apr 13, 2016)
###### Linux

```bash
screen /dev/ttyUSB0 9600
```

---

# Keep processes running beyond Jenkins job
##### (Apr 11, 2016)
###### Jenkins

In Jenkins job:
```bash
export BUILD_ID="dontKillMe /usr/bin/vagrant"
```

See <https://wiki.jenkins-ci.org/display/JENKINS/ProcessTreeKiller>

---

# Set JAVA_HOME from PATH
##### (Apr 05, 2016)
###### Java, Linux

```bash
export JAVA_HOME="${$(readlink -e $(type -p java))%*/bin/java}"
```

---

# Disable CAPS LOCK
##### (Apr 04, 2016)
###### Linux

```bash
setxkbmap -option ctrl:nocaps
```

---

# Get all patch files for Git revisions
##### (Nov 19, 2015)
###### Git

```bash
$ export N=100; for i in $(git log --abbrev-commit --format=format:'%h' --all  ${FILE}); do N=$((N+1)); git diff "${i}^..${i}" > ${N}-${i}.patch; done
```

---

# Run specific tests in Maven
##### (Nov 19, 2015)
###### Maven

```bash
$ mvn test -Dtest=com.nequissimus.*Test
```

---

# Environment variables for a PID
##### (Aug 26, 2015)
###### Linux

```bash
$ cat /proc/${PID}/environ
```

---

# Revert ESXi snapshot from the CLI
##### (Aug 25, 2015)
###### ESXi, VMware

```bash
$ vim-cmd vmsvc/getallvms
```

VMID is the first column

```bash
$ vim-cmd vmsvc/snapshot.get ${VMID}
```

Find snapshot ID

```bash
$ vim-cmd vmsvc/snapshot.revert ${VMID} ${SNAPID} false
```

---

# Grab images from Instagram app
##### (Mar 31, 2015)
###### Android

```bash
$ adb root
$ mkdir ~/Downloads/instagram
$ adb pull /data/data/com.instagram.android/cache ~/Downloads/instagram
$ find ~/Downloads/instagram -name '*.0' -exec mv {} {}.jpg \;
$ open ~/Downloads/instagram
```

---

# Show Mac CPU model
##### (Feb 7, 2015)
###### OSX

```bash
$ sysctl -n machdep.cpu.brand_string
Intel(R) Core(TM) i7-3770S CPU @ 3.10GHz
```

---

# Run IntelliJ IDEA without Java 1.6
##### (Dec 30, 2014)
###### Java, OSX

- Open `/Applications/IntelliJ\ IDEA\ 14\ CE.app/Contents/Info.plist`
- Find `JVMVersion`
- Change the value from `1.6*` to `1.8*`

---

# Find class file version
##### (Dec 15, 2014)
###### Java

```bash
$ od -x ~/dev/Test.class | head -1
0000000      feca    beba    0000    3200    4200    0007    073d    3e00
```

The 3200 signals JDK 1.6, 3100 would be 1.5 etc.

---

# Follow log files
##### (Dec 4, 2014)
###### Linux

```bash
find /var/log/ -name "*log" -type f -follow | xargs tail -n 0 -f | grep -v '^$'
```

---

# Change JVM from command line
##### (Nov 12, 2014)
###### Java, OSX

```bash
function setjdk() {
  if [ $# -ne 0 ]; then
    removeFromPath '/System/Library/Frameworks/JavaVM.framework/Home/bin'
    if [ -n "${JAVA_HOME+x}" ]; then
      removeFromPath $JAVA_HOME
    fi
    export JAVA_HOME=`/usr/libexec/java_home -v $@`
    export PATH=$JAVA_HOME/bin:$PATH
  fi
}

function removeFromPath() {
  export PATH="$(echo "$PATH" | sed -E -e "s;:$1;;" -e "s;$1:?;;")"
}

setjdk 1.9
```

_[Source](http://www.jayway.com/2014/01/15/how-to-switch-jdk-version-on-mac-os-x-maverick/)_

---

# MySQL prompt
##### (Nov 12, 2014)
###### MySQL

```bash
export MYSQL_PS1="\u@\d> "
```

---

# Single-line git commit history
##### (Jul 28, 2014)
###### git

```bash
git config --global alias.lg "log --graph --abbrev-commit --decorate --date=relative --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)' --all"
```

```bash
git lg
```

---

# Send plain-text messages via UDP
##### (Jul 24, 2014)
###### shell

```bash
echo "Test" | socat -u STDIO UDP-DATAGRAM:127.0.0.1:514
```

---

# Compare files with comm
##### (Jul 24, 2014)
###### shell

```bash
# First column: Lines unique to file1
# Second column: Lines unique to file2
# Third column: Lines common to both files
$ comm file1 file2
```

```bash
# Hide all columns (1, 2 & 3; which makes no sense!)
$ comm -123 file1 file2
 
# Only show common lines
$ comm -12 file1 file2
```

---

# Revert Subversion merge
##### (Apr 16, 2014)
###### Subversion

```bash
svn merge -c -REVISION .
```

---

# Send/Receive SNMP traps
##### (Sep 17, 2013)
###### OSX, Linux

_~/.snmp/snmptrapd.conf_

```bash
disableAuthorization yes
com2sec local     127.0.0.1/32    public
com2sec local     10.30.68.0/24   public
view all     included  .1
```

_Start receiver_

```bash
sudo snmptrapd -Lf ~/.snmp/output.txt
```

_Send sample trap_

```bash
sudo snmptrap -v 2c -c public -m ALL localhost "" SNMPv2-MIB::sysLocation.0 s s "Hello World"
```

---

# Change .NET framework in Powershell
##### (Sep 17, 2013)
###### Windows

```xml
<!-- The following should be saved in C:\Windows\System32\WindowsPowershell\v1.0\ (or the correct path to Windows Powershell)
 as a file named "powershell.exe.config" -->
<?xml version="1.0"?>
<configuration>
    <startup useLegacyV2RuntimeActivationPolicy="true">
        <supportedRuntime version="v4.0.30319"/>
        <supportedRuntime version="v2.0.50727"/>
    </startup>
</configuration>
```

---

# Path to .NET Framework
##### (Sep 17, 2013)
###### Windows

```powershell
function Get-FrameworkDirectory {
  $([System.Runtime.InteropServices.RuntimeEnvironment]::GetRuntimeDirectory())
}
```

---

# Copy files with progress
##### (Sep 16, 2013)
###### OSX, Linux

```bash
alias cp='rsync --progress'
```

---

# Remove key from known_hosts
##### (Sep 16, 2013)
###### OSX, Linux

```bash
ssh-keygen -R HOST
```

---

# MySQL log
##### (Sep 16, 2013)
###### MySQL

_my.cnf.51_

```
log=/var/log/mysql/mysql.log
```

_my.cnf.56_

```
general_log=true
general_log_file=/var/log/mysql/mysql.log
```

---

# Prevent Spotlight indexing 
##### (Mar 29, 2013)
###### OSX

```bash
touch /Volumes/MyDrive/.metadata_never_index
```

---

# List/Clear downloaded files list in OSX
##### (Jan 19, 2013)
###### OSX

_List_

```bash
sqlite3 ~/Library/Preferences/com.apple.LaunchServices.QuarantineEventsV2 'select LSQuarantineDataURLString from LSQuarantineEvent'
```

_Empty_

```bash
sqlite3 ~/Library/Preferences/com.apple.LaunchServices.QuarantineEvents* 'delete from LSQuarantineEvent'
```

---

# Rebuild context menu entries
##### (Aug 22, 2012)
###### OSX

```bash
/System/Library/Frameworks/CoreServices.framework/Versions/A/Frameworks/LaunchServices.framework/Versions/A/Support/lsregister -kill -r -domain local -domain system -domain user
```

---

# Limit bandwidth
##### (Apr 19, 2012)
###### OSX

_Add_

```bash
ipfw pipe 1 config bw 100KB
ipfw add 1 pipe 1 src-port any
```

_Remove_

```bash
ipfw delete 00001
```

---

# Headless VMware Fusion
##### (Feb 17, 2012)
###### OSX, VMware Fusion

```bash
/Library/Application\ Support/VMware\ Fusion/vmrun -T fusion start /Volumes/VMs/_Hadoop/Hadoop\ Master.vmwarevm/Hadoop\ Master.vmx nogui
```

---

# Hide/Show ~/Library
##### (Jul 21, 2011)
###### OSX

```bash
chflags [nohidden|hidden] ~/Library
```

---

# Force login via RDP
##### (Mar 11, 2011)
###### Windows

```
mstsc /v:123.123.123.123 /admin
```

---

# Blank Dock.app icon
##### (Feb 12, 2011)
###### OSX

```bash
defaults write com.apple.dock persistent-apps -array-add '{tile-data={}; tile-type="spacer-tile";}'
killall Dock
```

---

# Rebuild Spotlight index
##### (Oct 6, 2010)
###### OSX

```bash
sudo mdutil -E /
```

---

# Trigger OSX maintenance scripts
##### (Sep 21, 2010)
###### OSX

```bash
sudo periodic daily weekly monthly
```

---