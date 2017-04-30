<body>

<h1 align="center">haveged</h1>

<a href="#NAME">NAME</a><br>
<a href="#SYNOPSIS">SYNOPSIS</a><br>
<a href="#DESCRIPTION">DESCRIPTION</a><br>
<a href="#OPTIONS">OPTIONS</a><br>
<a href="#NOTES">NOTES</a><br>
<a href="#FILES">FILES</a><br>
<a href="#DIAGNOSTICS">DIAGNOSTICS</a><br>
<a href="#EXAMPLES">EXAMPLES</a><br>
<a href="#SEE ALSO">SEE ALSO</a><br>
<a href="#REFERENCES">REFERENCES</a><br>
<a href="#AUTHORS">AUTHORS</a><br>

<hr>


<h2>NAME
<a name="NAME"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">haveged &minus;
Generate random numbers and feed linux random device.</p>

<h2>SYNOPSIS
<a name="SYNOPSIS"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em"><b>haveged
[options]</b></p>

<h2>DESCRIPTION
<a name="DESCRIPTION"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em"><b>haveged</b>
generates an unpredictable stream of random numbers
harvested from the indirect effects of hardware events on
hidden processor state (caches, branch predictors, memory
translation tables, etc) using the HAVEGE (HArdware Volatile
Entropy Gathering and Expansion) algorithm. The algorithm
operates in user space, no special privilege is required for
file system access to the output stream.</p>

<p style="margin-left:11%; margin-top: 1em">Linux pools
randomness for distribution by the /dev/random and
/dev/urandom device interfaces. The standard mechanisms of
filling the /dev/random pool may not be sufficient to meet
demand on systems with high needs or limited user
interaction. In those circumstances, <b>haveged</b> may be
run as a privileged daemon to fill the /dev/random pool
whenever the supply of random bits in /dev/random falls
below the low water mark of the device.</p>

<p style="margin-left:11%; margin-top: 1em"><b>haveged</b>
tunes itself to its environment and provides the same
built-in test suite for the output stream as used on
certified hardware security devices. See <b>NOTES</b> below
for further information.</p>

<h2>OPTIONS
<a name="OPTIONS"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">-b nnn,
--buffer=nnn</p>

<p style="margin-left:22%;">Set collection buffer size to
nnn KW. Default is 128KW (or 512KB).</p>

<p style="margin-left:11%;">-d nnn, --data=nnn</p>

<p style="margin-left:22%;">Set data cache size to nnn KB.
Default is 16 or as determined dynamically.</p>

<p style="margin-left:11%;">-f file, --file=file</p>

<p style="margin-left:22%;">Set output file path for
non-daemon use. Default is &quot;sample&quot;, use
&quot;-&quot; for stdout.</p>

<p style="margin-left:11%;">-F , --Foreground</p>

<p style="margin-left:22%;">Run daemon in foreground. Do
not fork and detach.</p>

<p style="margin-left:11%;">-i nnn, --inst=nnn</p>

<p style="margin-left:22%;">Set instruction cache size to
nnn KB. Default is 16 or as determined dynamically.</p>

<p style="margin-left:11%;">-n nnn, --number=nnn</p>

<p style="margin-left:22%;">Set number of bytes written to
the output file. The value may be specified using one of the
suffixes k, m, g, or t. The upper bound of this value is
&quot;16t&quot; (2^44 Bytes = 16TB). A value of 0 indicates
unbounded output and forces output to stdout. This argument
is required if the daemon interface is not present. If the
daemon interface is present, this setting takes precedence
over any --run value.</p>

<p style="margin-left:11%;">-o &lt;spec&gt;,
--onlinetest=&lt;spec&gt;</p>

<p style="margin-left:22%;">Specify online tests to run.
The &lt;spec&gt; consists of optional &quot;t&quot;ot and
&quot;c&quot;ontinuous groups, each group indicates the
procedures to be run, using &quot;a&lt;n&gt;&quot; to
indicate a AIS-31 procedure A variant, and &quot;b&quot; to
indicate AIS procedure B. The specifications are order
independent (procedure B always runs first in each group)
and case insensitive. The a&lt;n&gt; variations exist to
mitigate the a slow autocorrelation test (test5). Normally
all procedure A tests, except the first are iterated 257
times. An a&lt;n&gt; option indicates test5 should only be
executed every modulo &lt;n&gt; times during the
procedure&rsquo;s 257 repetitions. The effect is so
noticable that A8 is the usual choice.</p>

<p style="margin-left:22%; margin-top: 1em">The
&quot;tot&quot; tests run only at initialization - there are
no negative performance consequences except for a slight
increase in the time required to initialize. The
&quot;tot&quot; tests guarantee haveged has initialized
properly. The use of both test procedures in the
&quot;tot&quot; test is highly recommended because the two
test emphasize different aspects of RNG quality.</p>

<p style="margin-left:22%; margin-top: 1em">In continuous
testing, the test sequence is cycled repeatedly. For
example, the string &quot;tbca8b&quot; (suitable for an AIS
NTG.1 device) would run procedure B for the &quot;tot&quot;
test, then cycle between procedure A8 and procedure B
continuously for all further output. Continuous testing does
not come for free, impacting both throughput and resource
consumption. Continual testing also opens up the possibility
of a test failure. A strict retry procedure recovers from
spurious failure in all but the most extreme circumstances.
When the retry fails, operation will terminate unless a
&quot;w&quot; has been appended to the test token to make
the test advisory only. In our example above, the string
&quot;tbca8wbw&quot; would make all continuous tests
advisory. For more detailed information on AIS retries see
<b>NOTES</b> below.</p>

<p style="margin-left:22%; margin-top: 1em">Complete
control over the test configuration is provided for
flexibility. The defaults (ta8bcb&quot; if run as a daemon
and &quot;ta8b&quot; otherwise) are suitable for most
circumstances.</p>

<p style="margin-left:11%;">-p file, --pidfile=file</p>

<p style="margin-left:22%;">Set file path for the daemon
pid file. Default is &quot;/var/run/haveged.pid&quot;,</p>

<p style="margin-left:11%;">-r n, --run=n</p>

<p style="margin-left:22%;">Set run level for daemon
interface:</p>

<p style="margin-left:22%; margin-top: 1em">n = 0 Run as
daemon - must be root. Fills /dev/random when the supply of
random bits <br>
falls below the low water mark of the device.</p>

<p style="margin-left:22%; margin-top: 1em">n = 1 Display
configuration info and terminate.</p>

<p style="margin-left:22%; margin-top: 1em">n &gt; 1 Write
&lt;n&gt; kb of output. Deprecated (use --number instead),
only provided for backward compatibility.</p>

<p style="margin-left:22%; margin-top: 1em">If --number is
specified, values other than 0,1 are ignored. Default is
0.</p>

<p style="margin-left:11%;">-v n, --verbose=n</p>

<p style="margin-left:22%;">Set diagnostic bitmap as sum of
following options:</p>

<p style="margin-left:22%; margin-top: 1em">1=Show
build/tuning summary on termination, summary for online test
retries.</p>

<p style="margin-left:22%; margin-top: 1em">2=Show online
test retry details</p>

<p style="margin-left:22%; margin-top: 1em">4=Show timing
for collections</p>

<p style="margin-left:22%; margin-top: 1em">8=Show
collection loop layout</p>

<p style="margin-left:22%; margin-top: 1em">16=Show
collection loop code offsets</p>

<p style="margin-left:22%; margin-top: 1em">32=Show all
online test completion detail</p>

<p style="margin-left:22%; margin-top: 1em">Default is 0.
Use -1 for all diagnostics.</p>

<p style="margin-left:11%;">-w nnn, --write=nnn</p>

<p style="margin-left:22%;">Set write_wakeup_threshold of
daemon interface to nnn bits. Applies only to run level
0.</p>

<p style="margin-left:11%;">-?, --help</p>

<p style="margin-left:22%;">This summary of program
options.</p>

<h2>NOTES
<a name="NOTES"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">haveged tunes
the HAVEGE algorithm for maximum effectiveness using a
hierarchy of defaults, command line options, virtual file
system information, and cpuid information where available.
Under most circumstances, user input is not required for
excellent results.</p>

<p style="margin-left:11%; margin-top: 1em">Run-time
testing provides assurance of correct haveged operation. The
run-time test suite is modeled upon the AIS-31 specification
of the German Common Criteria body, BIS. This specification
is typically applied to hardware devices, requiring formal
certification and mandated start-up and continuous
operational testing. Because haveged runs on many different
hardware platforms, certification cannot be a goal, but the
AIS-31 test suite provides the means to assess haveged
output with the same operational tests applied to certified
hardware devices.</p>

<p style="margin-left:11%; margin-top: 1em">AIS test
procedure A performs 6 tests to check for statistically
inconspicuous behavior. AIS test procedure B performs more
theoretical tests such as checking multi-step transition
probabilities and making an empirical entropy estimate.
Procedure A is the much more resource and compute intensive
of the two but is still recommended for the haveged start-up
tests. Procedure B is well suited to use of haveged as a
daemon because the test entropy estimate confirms the
entropy estimate haveged uses when adding entropy to the
/dev/random device.</p>

<p style="margin-left:11%; margin-top: 1em">No test is
perfect. There is a 10e-4 probability that a perfect
generator will fail either of the test procedures. AIS-31
mandates a strict retry policy to filter out false alarms
and haveged always logs test procedure failures. Retries are
expected but rarely observed except when large data sets are
generated with continuous testing. See the
<b>libhavege(3)</b> notes for more detailed information.</p>

<h2>FILES
<a name="FILES"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">If running as a
daemon, access to the following files is required</p>


<p style="margin-left:22%; margin-top: 1em"><i>/dev/random</i></p>


<p style="margin-left:22%; margin-top: 1em"><i>/proc/sys/kernel/osrelease</i></p>


<p style="margin-left:22%; margin-top: 1em"><i>/proc/sys/kernel/random/poolsize</i></p>


<p style="margin-left:22%; margin-top: 1em"><i>/proc/sys/kernel/random/write_wakeup_threshold</i></p>

<h2>DIAGNOSTICS
<a name="DIAGNOSTICS"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">Haveged returns
0 for success and non-zero for failure. The failure return
code is 1 &quot;general failure&quot; unless execution is
terminated by signal &lt;n&gt;, in which case the return
code will be 128 + &lt;n&gt;. The following diagnostics are
issued to stderr upon non-zero termination:</p>

<p style="margin-left:11%; margin-top: 1em">Cannot fork
into the background</p>

<p style="margin-left:22%;">Call to daemon(3) failed.</p>

<p style="margin-left:11%; margin-top: 1em">Cannot open
file &lt;s&gt; for writing.</p>

<p style="margin-left:22%;">Could not open sample file
&lt;s&gt; for writing.</p>

<p style="margin-left:11%; margin-top: 1em">Cannot write
data in file:</p>

<p style="margin-left:22%;">Could not write data to the
sample file.</p>

<p style="margin-left:11%; margin-top: 1em">Couldn&rsquo;t
get pool size.</p>

<p style="margin-left:22%;">Unable to read
/proc/sys/kernel/random/poolsize</p>

<p style="margin-left:11%; margin-top: 1em">Couldn&rsquo;t
initialize HAVEGE rng</p>

<p style="margin-left:22%;">Invalid data or instruction
cache size.</p>

<p style="margin-left:11%; margin-top: 1em">Couldn&rsquo;t
open PID file &lt;s&gt; for writing</p>

<p style="margin-left:22%;">Unable to write daemon PID</p>

<p style="margin-left:11%; margin-top: 1em">Couldn&rsquo;t
open random device</p>

<p style="margin-left:22%;">Could not open /dev/random for
read-write.</p>

<p style="margin-left:11%; margin-top: 1em">Couldn&rsquo;t
query entropy-level from kernel: error</p>

<p style="margin-left:22%;">Call to ioctl(2) failed.</p>

<p style="margin-left:11%; margin-top: 1em">Couldn&rsquo;t
open PID file &lt;path&gt; for writing</p>

<p style="margin-left:22%;">Error writing
/var/run/haveged.pid</p>


<p style="margin-left:11%; margin-top: 1em">Fail:set_watermark()</p>

<p style="margin-left:22%;">Unable to write to
/proc/sys/kernel/random/write_wakeup_threshold</p>

<p style="margin-left:11%; margin-top: 1em">RNDADDENTROPY
failed!</p>

<p style="margin-left:22%;">Call to ioctl(2) to add entropy
failed</p>

<p style="margin-left:11%; margin-top: 1em">RNG failed</p>

<p style="margin-left:22%;">The random number generator
failed self-test or encountered a fatal error.</p>

<p style="margin-left:11%; margin-top: 1em">Select
error</p>

<p style="margin-left:22%;">Call to select(2) failed.</p>

<p style="margin-left:11%; margin-top: 1em">Stopping due to
signal &lt;n&gt;</p>

<p style="margin-left:22%;">Signal &lt;n&gt; caught.</p>

<p style="margin-left:11%; margin-top: 1em">Unable to setup
online tests</p>

<p style="margin-left:22%;">Memory unavailable for online
test resources.</p>

<h2>EXAMPLES
<a name="EXAMPLES"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">Write 1.5MB of
random data to the file /tmp/random</p>

<p style="margin-left:22%;">haveged -n 1.5M -f
/tmp/random</p>

<p style="margin-left:11%;">Generate a /tmp/keyfile for
disk encryption with LUKS</p>

<p style="margin-left:22%;">haveged -n 2048 -f
/tmp/keyfile</p>

<p style="margin-left:11%;">Overwrite partition /dev/sda1
with random data. Be careful, all data on <br>
the partition will be lost!</p>

<p style="margin-left:22%;">haveged -n 0 | dd
of=/dev/sda1</p>

<p style="margin-left:11%;">Generate random ASCII passwords
of the length 16 characters</p>

<p style="margin-left:22%;">(haveged -n 1000 -f -
2&gt;/dev/null | tr -cd &rsquo;[:graph:]&rsquo; | fold -w 16
&amp;&amp; echo ) | head</p>

<p style="margin-left:11%;">Write endless stream of random
bytes to the pipe. Utility pv measures <br>
the speed by which data are written to the pipe.</p>

<p style="margin-left:22%;">haveged -n 0 | pv &gt;
/dev/null</p>

<p style="margin-left:11%;">Evaluate speed of haveged to
generate 1GB of random data</p>

<p style="margin-left:22%;">haveged -n 1g -f - | dd
of=/dev/null</p>

<p style="margin-left:11%;">Create a random key file
containing 65 random keys for the encryption <br>
program aespipe.</p>

<p style="margin-left:22%;">haveged -n 3705 -f -
2&gt;/dev/null | uuencode -m - | head -n 66 | tail -n 65</p>

<p style="margin-left:11%;">Test the randomness of the
generated data with dieharder test suite</p>

<p style="margin-left:22%;">haveged -n 0 | dieharder -g 200
-a</p>

<p style="margin-left:11%;">Generate 16k of data, testing
with procedure A and B with detailed test <br>
results. No c result seen because a single buffer fill did
not contain <br>
enough data to complete the test.</p>

<p style="margin-left:22%;">haveged -n 16k -o tba8ca8 -v
33</p>

<p style="margin-left:11%;">Generate 16k of data as above
with larger buffer. The c test now <br>
completes - enough data now generated to complete the
test.</p>

<p style="margin-left:22%;">haveged -n 16k -o tba8ca8 -v 33
-b 512</p>

<p style="margin-left:11%;">Generate 16m of data as above,
observe many c test completions with <br>
default buffer size.</p>

<p style="margin-left:22%;">haveged -n 16m -o tba8ca8 -v
33</p>

<p style="margin-left:11%;">Generate large amounts of data
- in this case 16TB. Enable <br>
initialization test but made continuous tests advisory only
to avoid a <br>
possible situation that program will terminate because of
procedureB <br>
failing two times in a row. The probability of procedureB to
fail two <br>
times in a row can be estimated as &lt;TB to
generate&gt;/3000 which yields <br>
0.5% for 16TB.</p>

<p style="margin-left:22%;">haveged -n 16T -o tba8cbw -f -
| pv &gt; /dev/null</p>

<p style="margin-left:11%;">Generate large amounts of data
(16TB). Disable continuous tests for the <br>
maximum throughput but run the online tests at the startup
to make sure <br>
that generator for properly initialized:</p>

<p style="margin-left:22%;">haveged -n 16T -o tba8c -f - |
pv &gt; /dev/null</p>

<h2>SEE ALSO
<a name="SEE ALSO"></a>
</h2>



<p style="margin-left:11%; margin-top: 1em"><b>libhavege(3),</b></p>

<p style="margin-left:22%;"><b>cryptsetup(8), aespipe(1),
pv(1), openssl(1), uuencode(1)</b></p>

<h2>REFERENCES
<a name="REFERENCES"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em"><b><i>HArdware
Volatile Entropy Gathering and Expansion: generating
unpredictable random numbers at user level</i></b> by A.
Seznec, N. Sendrier, INRIA Research Report, RR-4592, October
2002</p>

<p style="margin-left:11%; margin-top: 1em"><i>A proposal
for: Functionality classes for random number generators</i>
by W. Killmann and W. Schindler, version 2.0, Bundesamt fur
Sicherheit in der Informationstechnik (BSI), September,
2011</p>

<p style="margin-left:11%; margin-top: 1em"><i>A
Statistical Test Suite for the Validation of Random NUmber
Generators and Pseudorandom Number Generators for
Cryptographic Applications,</i> special publication
SP800-22, National Institute of Standards and Technology,
revised April, 2010</p>

<p style="margin-left:11%; margin-top: 1em">Additional
information can also be found at
<b>http://www.issihosts.com/haveged/</b></p>

<h2>AUTHORS
<a name="AUTHORS"></a>
</h2>


<p style="margin-left:11%; margin-top: 1em">Gary Wuertz
&lt;gary@issiweb.com&gt; and Jirka Hladky &lt;hladky jiri AT
gmail DOT com&gt;</p>
<hr>
</body>
</html>
