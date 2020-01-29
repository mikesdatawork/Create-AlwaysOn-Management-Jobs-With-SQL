![MIKES DATA WORK GIT REPO](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_01.png "Mikes Data Work")        

# Create AlwaysOn Management Jobs With SQL
**Post Date: September 28, 2016**        



## Contents    
- [About Process](##About-Process)  
- [SQL Logic](#SQL-Logic)  
- [Build Info](#Build-Info)  
- [Author](#Author)  
- [License](#License)       

## About-Process

<p>It's always a good idea to create some Standard AlwaysOn jobs for management purposes whenever you're first learning, or experimenting with the AlwaysOn configurations. You'll find times when you need to remove databases, or all databases from the HADR configurations, or to completely remove them from Availability Groups.
Below you'll find some quick logic to create these jobs. You can see some typical AlwaysOn Jobs for this purpose.</p>

![SQL AlwaysOn Jobs](https://mikesdatawork.files.wordpress.com/2016/09/image0014.png "AlwaysOn SQL Jobs")
 
In this case we'll be focusing the 2 of them.
ALWAYSON – REMOVE DATABASES FROM AG
ALWAYSON – SET HADR OFF FOR DATABASES

![AG Jobs]( https://mikesdatawork.files.wordpress.com/2016/09/image0021.png "Manage Availability Groups With SQL")
 
You can take the logic below and throw them into a quick Job if needed.
ALWAYSON – REMOVE DATABASES FROM AG

## SQL-Logic
```SQL
use master;
set nocount on
declare @remove_from_ag         varchar(max)
declare @availability_group     varchar(255)
set     @remove_from_ag         = ''
set     @availability_group     = (select name from sys.availability_groups)
select distinct @remove_from_ag = @remove_from_ag +
'alter availability group [' + @availability_group + '] remove database [' + database_name + '];' + char(10)
from   sys.dm_hadr_database_replica_cluster_states
exec   (@remove_from_ag)
```
ALWAYSON – SET HADR OFF FOR DATABASES



## SQL-Logic
```SQL
use master;
set nocount on
declare @set_hadr_off          varchar(max)
declare @availability_group    varchar(255)
set     @set_hadr_off          = ''
set     @availability_group    = (select name from sys.availability_groups)
select  distinct @set_hadr_off = @set_hadr_off +
'alter  database [' + database_name + '] set hadr off;' + char(10)
from    sys.dm_hadr_database_replica_cluster_states
exec    (@set_hadr_off)
```


[![WorksEveryTime](https://forthebadge.com/images/badges/60-percent-of-the-time-works-every-time.svg)](https://shitday.de/)

## Build-Info

| Build Quality | Build History |
|--|--|
|<table><tr><td>[![Build-Status](https://ci.appveyor.com/api/projects/status/pjxh5g91jpbh7t84?svg?style=flat-square)](#)</td></tr><tr><td>[![Coverage](https://coveralls.io/repos/github/tygerbytes/ResourceFitness/badge.svg?style=flat-square)](#)</td></tr><tr><td>[![Nuget](https://img.shields.io/nuget/v/TW.Resfit.Core.svg?style=flat-square)](#)</td></tr></table>|<table><tr><td>[![Build history](https://buildstats.info/appveyor/chart/tygerbytes/resourcefitness)](#)</td></tr></table>|

## Author

[![Gist](https://img.shields.io/badge/Gist-MikesDataWork-<COLOR>.svg)](https://gist.github.com/mikesdatawork)
[![Twitter](https://img.shields.io/badge/Twitter-MikesDataWork-<COLOR>.svg)](https://twitter.com/mikesdatawork)
[![Wordpress](https://img.shields.io/badge/Wordpress-MikesDataWork-<COLOR>.svg)](https://mikesdatawork.wordpress.com/)

     
## License
[![LicenseCCSA](https://img.shields.io/badge/License-CreativeCommonsSA-<COLOR>.svg)](https://creativecommons.org/share-your-work/licensing-types-examples/)

![Mikes Data Work](https://raw.githubusercontent.com/mikesdatawork/images/master/git_mikes_data_work_banner_02.png "Mikes Data Work")

