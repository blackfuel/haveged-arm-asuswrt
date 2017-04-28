# dnscrypt-arm-asuswrt

### HOWTO: Compile Haveged for AsusWRT firmware
```
cd
git clone https://github.com/blackfuel/haveged-arm-asuswrt.git
cd haveged-arm-asuswrt
./haveged.sh
```

http://www.issihosts.com/haveged/index.html

<body>
<div id="page">
<h1>haveged - A simple entropy daemon</h1>

<h2>Introduction</h2>
<p>
The haveged project is an attempt to provide an easy-to-use, unpredictable random
number generator based upon an adaptation of the
<a href="http://www.irisa.fr/caps/projects/hipsor/">HAVEGE</a> algorithm.
Haveged was created to remedy low-entropy conditions in the Linux random device
that can occur under some workloads, especially on headless servers. Current
development of haveged is directed towards improving overall reliability and
adaptability while minimizing the barriers to using haveged for other tasks.
<p>
Downloads and release notes for haveged are found  <a href="downloads.html">here</a>.
<p>
Background on the details of the <a href="history.html#intro">linux random device</a>,
a survey of <a href="history.html#other">other solutions</a> to entropy shortages,
and notes on the <a href="history.html#havege">design</a> and
<a href="history.html#haveged">evolution</a> of haveged are extracted
from the home page that graced this site for several years.


<h2>Recent Documentation</h2>
<p>
The original HAVEGE research dates back to 2003 and much of the original haveged
documentation is now quite dated. Recent work on haveged has included an effort
to provide more recent information on the project and its applications.
<p>
The original research behind HAVEGE use was based upon studies of the behavior
of processor caches from a hardware level. The 'Flutter' documents below attempt
to provide a modern view of HAVEGE at software level through the use of a
diagnostic build of haveged that captures the non deterministic inputs to
haveged for analysis by external tools.
<ul>
<li><a href="faq.html">FAQ</a>
<li><a href="ais31.html">Runtime Testing</a>
<li><a href="flutter.html">Processor Flutter - Part 1</a>
<li><a href="rtillery.html">Processor Flutter - Part 2</a>
<li><a href="rtillery2.html">Processor Flutter - Part 3</a>
</ul>
<h2>Current Version</h2>
<p>
<a href="downloads.html">The current version of haveged is 1.9.1 .</a>

<p style="text-align:center">
<img src="images/entropy-day.png" alt="">
<p>
gary at issiweb dot com
<p>
Like haveged? You may also be interested in  <a href="../topvhost/"><b>topvhost</b></a>.
</p>
<p>
Recent support for haveged provided by Ben Eleventh Consulting.
</p>
</div>
</body>

