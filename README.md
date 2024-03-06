# cf9-rocky9-selinux
Bash Script to Install Coldfusion 9 on RockyLinux 9 (or Redhat 9 / CentOS 9) with SELinux enabled.<br>
Use on a fresh installed OS is recommended.

<ol>
<li>first, get the cf9rocky9selinux.sh<BR>
<BR></li>

<li>then, switch to a sudoer account and enter the sudoer password
<pre>su <i>a_sudoer_account</i></pre> </li>

<li>next, set the script as executable
<pre>chmod u+x cf9rocky9selinux.sh</pre></li>

<li>and now, run the bash script 
<pre>./cf9rocky9selinux.sh</pre> </li>

<li>wait until the script execution is done, then test your installation:
<pre>systemctl status coldfusion_9</pre></li>

<li>Open your browser to continue to the configuration:
<pre>http://127.0.0.1/CFIDE/administrator/index.cfm</pre></li>

</ol>
