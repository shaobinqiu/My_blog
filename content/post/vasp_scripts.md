---
title: "Vasp_scripts"
date: 2018-08-08T17:06:55+08:00
lastmod: 2018-11-12T17:54:13+08:00
draft: false
tags: ["vasp", "script"]
categories: ["vasp"]
author: "qiusb"
---
# 一些小脚本

### 1. OSZICAR最后一行信息
$i $ionstep $F $E $mag(如果有)

```
for i in {1..80}
do
E=`tail -1 OSZICAR-$i`
inf=`echo $E|cut -d " " -f 1,3,5,10`
echo $i $inf >>energy
done
```

### 2.截断能测试
```
rm summary
for i in 250 300 350 400 450 500 550 600 650
do
cat>INCAR<<!
SYSTEM=Al13
NSW=0
ENCUT=$i
ISMEAR=0
EDIFFG=-0.01
LREAL=Auto
EDIFF=1e-06
NPAR=2
ISIF=2
ISPIN=2
IBRION=-1

!

mpirun -n 12 vasp_std
E=`tail -1 OSZICAR`
inf=`echo $E|cut -d " " -f 1,3,5,10`
echo $i $inf >>summary
mv OSZICAR OSZICAR-$i
mv CONTCAR CONTCAR-$i
done

```


### 3.团簇取胞大小测试（三维真空层测试）
```
for i in {12..22}
do
cat>POSCAR<<!
Al13
1
$i 0 0
0 $i 0 
0 0 $i
Al
13
Cartesian
1.695820756	0.952639371	1.856287251
-1.695821014	-0.952639637	-1.856287265
0	0	0
-2.058362865	-1.47868117	0.897650999
2.433507217	-1.13868169	0.102854328
0.493702277	-1.593661486	2.108467032
0.113294345	-2.641320534	-0.489615383
-0.493702239	1.593661679	-2.108467135
-2.433507292	1.13868191	-0.102854299
2.058362974	1.478681067	-0.89765081
-0.113294051	2.641320466	0.489615657
-1.080307483	0.742508125	2.347498631
1.080307559	-0.742508211	-2.347498757
!

mpirun -n 12 vasp_std
E=`tail -1 OSZICAR`
inf=`echo $E|cut -d " " -f 1,3,5,10`
echo $i $inf >>summary
mv OSZICAR OSZICAR-$i
mv CONTCAR CONTCAR-$i
done
```
!!修改POSCARdier行的a会将原子位置也改变（等比缩放），即便是笛卡尔坐标下。
