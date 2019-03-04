2019.1.28 - 2019.1.29

---
帮石磊提取时长，带宽，realbrs

SQL命令：  
select 
rtime,
countryCode,
netType,
sessionId,
uid,
item.idx,
item.blockCnt,
item.blockTime,
item.bitrate,
item.framerate,
item.height,
item.width,
item.bandWidth0,
item.bandWidth1,
item.ip,
item.port,
item.transMode,
item.bufferTime,
(CASE WHEN item.extra['brs'] IS NULL THEN 'unknow' ELSE item.extra['brs'] END) as brs,
(CASE WHEN item.extra['qoe'] IS NULL THEN 'unknow' ELSE item.extra['qoe'] END) as qoe,
(CASE WHEN item.extra['buffs'] IS NULL THEN 'unknow' ELSE item.extra['buffs'] END) as buffs,
(CASE WHEN item.extra['realbrs'] IS NULL THEN 'unknow' ELSE item.extra['realbrs'] END) as realbrs,
(CASE WHEN item.extra['bwes'] IS NULL THEN 'unknow' ELSE item.extra['bwes'] END) as bwes,
(CASE WHEN item.extra['resolution'] IS NULL THEN 'unknow' ELSE item.extra['resolution'] END) as resolution,
(CASE WHEN item.extra['bws'] IS NULL THEN 'unknow' ELSE item.extra['bws'] END) as bws,
from indigo.indigo_player_stat
LATERAL VIEW explode(items) t1 AS item
where day = '2019-01-26'
order by uid,sessionId,rtime
limit 100