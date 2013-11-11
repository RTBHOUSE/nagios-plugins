nagios-plugins
==============

RTBHOUSE Nagios plugins



check_atop_log.pl
-----------------

This is Atop Log Check plugin. It parse output of atopsar command from last 
interval and compare values to given thresholds.   

Plugin returns stats variables as perfomance data for further nagios 2.0
post-processing, it was testet witch graphios. ( number of data change dinamicly
as sytem change, so write in to rrd file is inefficient  )

This program is based on check_redis.pl by William Leibzon - william(at)leibzon.org



####  SETUP NOTES ####

You need atop, that writes atop.log in to its default location. This plugin try to 
detect atop log inteval, but it was only tested, when atop writes once in 60 seconds.
and nagios checks also every 60 seconds.

Old debian/ubuntu atop uses logrotate to manage atop.log. This breakes this
plugin ( beetwen midnight, and logrotate time ).  Workaround is to trigger 
logrotate for atop just before midnight:

        CRON: 1 0 * * * logrotate  -f  /etc/logrotate.d/atop

Next for help and to see what parameters this plugin accepts do:
        ./check_atop_log.pl --help

 All variables can be returned as performance data for graphing. Graphios plugin
 was testeted ( https://github.com/shawn-sterling/graphios ) .

#### Example of Usage command-line usage ####

Generate performace data to graphite via garphios plugin ( also show all possible variables) 

        ./check_atop_log.pl -A   

Check if there are error or drop packets counter incrase on ANY inteface:

        ./check_atop_log.pl -o 'PATTERN:if_.*_ierr/s,WARN:>1,CRIT:>5' -o 'PATTERN:if_.*_idrop/s,WARN:>1,CRIT:>5' 

Check if any eth* inteface reached thtoughpput limit 700Mb/s Warning 900Mb/s Critical

        ./check_atop_log.pl -o 'PATTERN:if_eth.*_imbps,WARN:>700,CRIT:>900' -o 'PATTERN:if_eth.*_ombps,WARN:>700,CRIT:>900' 


