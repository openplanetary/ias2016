## Data used during IAS 2016 Short Course

all data can be downloaded from http://ode.rsl.wustl.edu/mars/index.aspx 

### CTX

EDR from PDS Geosciences Node

* 99M D21_035563_1987_XN_18N282W.IMG
* 49M F04_037396_1985_XN_18N282W.IMG
* 79M P19_008650_1987_XI_18N282W.IMG

### HiRISE

DTM and orthoimages from University of Arizona
http://hirise.lpl.arizona.edu/dtm/dtm.php?ID=PSP_002387_1985

* 305M DTEEC_002387_1985_003798_1985_A01.IMG
* 369M PSP_002387_1985_RED_A_01_ORTHO.JP2
* 5.3K PSP_002387_1985_RED_A_01_ORTHO.LBL


### CRISM

TRDR from PDS Geosciences Node

### HRSC

Level4 from PDS Geosciences Node (or PSA)

### MOLA (crop)

MEGDR from PDS Geosciences Node

## Map_template

see https://isis.astrogeology.usgs.gov/UserDocs/index.html

```
  Group = Mapping
    ProjectionName     = EQUIRECTANGULAR
    CenterLongitude    = 0.0
    CenterLatitude     = 0.0
    TargetName         = Mars
    EquatorialRadius   = 3396190.0 <meters>
    PolarRadius        = 3396190.0 <meters>
    LatitudeType       = Planetocentric
    LongitudeDirection = PositiveEast
    LongitudeDomain    = 360
  End_Group
```

## Processing steps 

some skipped for ease during limited time, reproducible though with info below.

### HiRISE

#### DTM
source DTM in PDS format, orthoimages in JP2, from http://hirise.lpl.arizona.edu/dtm/dtm.php?ID=PSP_002387_1985

```
pds2isis from=DTEEC_002387_1985_003798_1985_A01.IMG to=DTEEC_002387_1985_003798_1985_A01.cub

map2map from=DTEEC_002387_1985_003798_1985_A01.cub map=../map_template-CTX.map \
  to=DTEEC_002387_1985_003798_1985_A01.eqc.cub pixres=from
```

#### Orthoimage(s)

```
JP2_to_PDS -Input PSP_002387_1985_RED_A_01_ORTHO.JP2 \
  -LAbel PSP_002387_1985_RED_A_01_ORTHO.LBL \
  -Output PSP_002387_1985_RED_A_01_ORTHO.IMG
  
pds2isis from=PSP_002387_1985_RED_A_01_ORTHO.IMG to=PSP_002387_1985_RED_A_01_ORTHO.cub

map2map from=PSP_002387_1985_RED_A_01_ORTHO.cub map=../map_template-CTX.map \
  to=PSP_002387_1985_RED_A_01_ORTHO.eqc.cub pixres=from
```

### CTX

source EDR, going thorugh PDS --> level0 cub --> level1 cub --> level2 cub

for ease https://gist.github.com/aprossi/dac0f38f4991f8da9fcfe3722728ce16

steps:
* mroctx2isis
* spiceinit
* ctxcal
* ctxevenodd
* cam2map

Plus, quick & dirty mosaic:

```
ls *lev2.cub > moslist

automos fromlist=moslist mosaic=ctx_mosaci_jezero.cub
```
A bit less dirty:

```
ls F04_037396_1985_XN_18N282W.lev2.cub > holdlist

equalizer fromlist=moslist holdlist=holdlist

ls *equ.cub > equlist

automos fromlist=equlist mosaic=ctx_mosaci_jezero.equ.cub
```

(extraction of label to get the right kernels for VM)

```
catlab from=D21_035563_1987_XN_18N282W.lev1.cub to=D21_035563_1987_XN_18N282W.lev1-label.txt
catlab from=F04_037396_1985_XN_18N282W.lev1.cub to=F04_037396_1985_XN_18N282W.lev1-label.txt
catlab from=P19_008650_1987_XI_18N282W.lev1.cub to=P19_008650_1987_XI_18N282W.lev1-label.txt
```

### HRSC

source Level4

```
pds2isis from=h0988_0000_nd4.img to=h0988_0000_nd4.cub

map2map from=h0988_0000_nd4.cub map=../map_template-CTX.map \
  to=h0988_0000_nd4.eqc.cub pixres=from
  
```
### Needed software
(only ISIS3 provided during short course)
ISIS3 - https://isis.astrogeology.usgs.gov/documents/InstallGuide/index.html
PDS_JP2 - http://pirlwww.lpl.arizona.edu/software/PDS_JP2/download.php

