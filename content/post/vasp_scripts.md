---
title: "Vasp_scripts"
date: 2018-08-08T17:06:55+08:00
lastmod: 2018-08-08T17:54:13+08:00
draft: false
tags: ["vasp", "script"]
categories: ["vasp"]
author: "qiusb"
---
# 一些小脚本

## 1. OSZICAR最后一行信息
$i $ionstep $F $E $mag(如果有)

```
for i in {1..80}
do
E=`tail -1 OSZICAR-$i`
inf=`echo $E|cut -d " " -f 1,3,5,10`
echo $i $inf >>energy
done
```


