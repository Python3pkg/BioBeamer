# Collecting instrument data using BioBeamer

![BioBeamer UML](/images/classes_No_Name.png)

## Install 
```bash
git clone git@github.com:cpanse/BioBeamer.git
```

## Configure 

### BioBeamer xml configuration file

```xml
<?xml-stylesheet type="text/xsl" href="BioBeamer.xsl"?>
<BioBeamerHosts>
<!-- 
# $HeadURL: http://fgcz-svn.uzh.ch/repos/fgcz/stable/proteomics/config/BioBeamer.xml $
# $Id: BioBeamer.xml 7914 2015-12-15 07:19:02Z cpanse $
# $Date: 2015-12-15 08:19:02 +0100 (Tue, 15 Dec 2015) $
# $Author: cpanse $
-->
<host name="fgcz-i-180" 
    instrument="TRIPLETOF_1"
    min_size="1024" 
    min_time_diff="10800" 
    max_time_diff="2419200" 
    simulate='false' 
    func_target_mapping="map_data_analyst_tripletof1" 
    robocopy_args="/E /Z /NP /R:0 /LOG+:C:\\Progra~1\\BioBeamer\\robocopy.log"
    pattern=".+" 
    source_path="D:/Analyst Data/Projects/" 
    target_path="\\130.60.81.21\\Data2San">
    <b-fabric>
        <applicationID>93</applicationID>
    </b-fabric>
</host>
```

### SOP deploy @ new location
* change syslog host
* change configuration url

### Configure Syslog '/etc/rsyslog.conf' 

```syslog
$template tplremote,"%timegenerated% %HOSTNAME% %fromhost-ip% %syslogtag%%msg:::drop-last-lf%\n"
$template RemoteHost,"/var/log/remote/%HOSTNAME%_%fromhost-ip%.log"

if ($fromhost-ip != '127.0.0.1') then ?RemoteHost;tplremote  
& ~
```

### Configure logrotate '/etc/logrotate.d/remote'
```conf
/var/log/remote/*
{
        rotate 13
        monthly
        missingok
        notifempty
        compress
}
```

## Run

### @ FGCZ
just 'run as administrator' justBeamFiles.exe.

the justBeamFiles.exe maps the storage and runs the fgcz_biobeamer.py script which uses robocopy.exe on Micorsoft installed PCs to sync the files.

### otherwise

```cmd
python BioBeamer.py
```



## Author
[Christian Panse](http://www.fgcz.ch/the-center/people/panse.html)

## See also
[fgcz-intranet wiki page](http://fgcz-intranet.uzh.ch/tiki-index.php?page=BioBeamer)

## TODO
* munin plugin

