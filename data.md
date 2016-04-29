## Data used during IAS 2016 Short Course

all data can be downloaded from http://ode.rsl.wustl.edu/mars/index.aspx 

### CTX

* EDR

### HiRISE

* DTM and orthoimages

### CRISM

* TRDR

### HRSC

* Level4

### MOLA (crop)

* MEGDR

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

## Processing steps (for ease during limited time, reproducible though)

### HiRISE

source DTM in PDS format, orthoimages in JP2

```
pds2isis from=DTEEC_002387_1985_003798_1985_A01.IMG to=DTEEC_002387_1985_003798_1985_A01.cub

map2map from=DTEEC_002387_1985_003798_1985_A01.cub map=../map_template-CTX.map \
  to=DTEEC_002387_1985_003798_1985_A01.eqc.cub pixres=from
```

### CTX

source EDR, going thorugh PDS --> level0 cub --> level1 cub --> level2 cub
