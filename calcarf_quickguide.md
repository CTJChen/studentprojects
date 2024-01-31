## IXPECALCARF Quick Guide

### Software Requirements
* HEASoft (version 6.xx+)

### Data Requirements
##### Necessary:
* IXPE level 2 event list

##### Optional:
* IXPE level 1 attitude file
* Existing spectral files


### General Overview
A detailed documentation is available at the following url:
https://heasarc.gsfc.nasa.gov/docs/software/ftools/caldb/help/ixpecalcarf.py.html 

Here we show a few examples:

#### Case 1
**Make an MRF (for Q and U spectra) with a constant off-axis angle and spectral extraction aperture**

```bash
ixpecalcarf evtfile=ixpe02001001_det1_evt2_v02.fits offarcmin=1.5 radius=1.0 specfile=none \
resptype=mrf clobber=yes
```
Here ixpecalcarf recognized *rseptype=mrf* and saved the output MRF file as ixpe02001001_det1_evt2_v02.mrf. 



#### Case 2:
**Make an ARF and save the out to a designated filename with a .arf extension.**

```bash
ixpecalcarf evtfile=ixpe02001001_det1_evt2_v02.fits offarcmin=2.5 radius=2.0 specfile=none \
arfout=ixpe02001001_det1.arf clobber=yes
```
Between Case 1 and Case 2, ixpecalarf would attempt to make a file of the type specified by the *arfout* argument 
if it's parsed. Otherwise, it requires a *resptype* argument to tell it to either make an ARF (for I spectra),
or an MRF (for Q and U spectra)

#### Case 3 - making ARF for an existing spectra -- *warning, header keyword will be updated!*
```bash
ixpecalcarf evtfile=event_l2/ixpe01004601_det1_evt2_v05.fits  attfile=hk/ixpe01004601_det1_att_v02.fits.gz \
specfile=ixpe01004601_src_DU1_pha1.fits clobber=yes radius=1.0
```
In this case, a new file ixpe01004601_src_DU1_pha1.arf was made and placed in the same folder as the spectrum file,
and is associated to the original spectrum file by updating the ANCRFILE keyword in it.
