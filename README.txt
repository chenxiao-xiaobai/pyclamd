pyClamad 是一个 Python 模块，可让 Python 应用直接使用 ClamAV 病毒扫描守护进程 clamd
pyClamd is a portable Python module to use the ClamAV antivirus engine on 
Windows, Linux, MacOSX and other platforms. It requires a running instance of 
the clamd daemon.

This is a slightly improved version of pyClamd v0.1.1 created by Alexandre 
Norman and published on his website: http://xael.org/norman/python/pyclamd/

website for v0.2.0: http://www.decalage.info/en/python/pyclamd  

Improvements from version 0.1.1 to 0.2.0:

    * added option to set a timeout on scans
    * fixed warning due to string exceptions with Python 2.5

License:
pyClamd is released as open-source software under the GPLv2 license.

Download:
see http://www.decalage.info/en/python/pyclamd


How to install clamd:

    * For Windows: you need an unofficial version from http://hideout.ath.cx/clamav/ 
      or http://oss.netfarm.it/clamav/ (http://w32.clamav.net does not provide 
      clamd anymore, and neither does ClamWin)
          o Before running clamd, edit clamd.conf and make sure it is configured 
            to use a TCP port instead of a Unix socket: LocalSocket should be 
            disabled, TCPSocket and TCPAddr should be enabled.
    * For MacOSX: you may install ClamXav, and then run clamd from /usr/local/clamXav/sbin.
    * For other operating systems such as Linux and *BSD: http://www.clamav.org/download

 

How to run clamd as a service on Windows:

See http://www.andornot.com/blog/post/How-to-set-up-ClamAV-as-a-Windows-Service-to-scan-file-streams-on-demand.aspx 
or http://www.google.com/search?q=clamd+windows+service

There used to be instructions on http://www.asspsmtp.org/wiki/ClamAV_Win32 to 
use either runclamd or the NJH Power Tools, but the website is not available 
anymore.


How to use pyClamd:

See source code or Alexandre Norman's website: 
http://xael.org/norman/python/pyclamd/

Here is an example on Unix:

$ python
>>> import pyclamd
>>> cd = pyclamd.ClamdUnixSocket()
>>> try:
...     cd.ping()
... except pyclamd.ConnectionError:
...     cd = pyclamd.ClamdNetworkSocket()
...     cd.ping()
... 
True
>>> cd.version().split()[0]
'ClamAV'
>>> cd.reload()
'RELOADING'
>>> cd.stats().split()[0]
'POOLS:'
>>> open('/tmp/EICAR','w').write(cd.EICAR())
>>> open('/tmp/NO_EICAR','w').write('no virus in this file')
>>> cd.scan_file('/tmp/EICAR')
{'/tmp/EICAR': ('FOUND', 'Eicar-Test-Signature')}
>>> cd.scan_file('/tmp/NO_EICAR') is None
True
>>> cd.scan_stream(cd.EICAR())
{'stream': ('FOUND', 'Eicar-Test-Signature')}
>>> 
