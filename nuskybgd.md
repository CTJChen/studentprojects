#### 1. Assuming you already have heasoft and CALDB with $CALDB and $CALDBCONFIG environment variables defined
#### 2. go to terminal, run 
```bash
cd path_you_want_to_have_nuskybgd
git clone https://github.com/achronal/nuskybgd-py


export NUSKYBGD=path_you_want_to_have_nuskybgd/nuskybgd-py
export NUSKYBGD_AUXIL=$NUSKYBGD/auxil


export PATH=$NUSKYBGD/bin:$PATH

export PYTHONPATH=$NUSKYBGD:$PYTHONPATH
```

#### 3. Go to your nustar data directory, preferably the re-processed folder
```bash
cd /home/ctchen/work_scripts/j1440_nustar/nustar/60601028002/out
python $NUSKYBGD/bin/mkimgs nu60601028002A01_cl.evt im3to20kev 3 20
```

#### 4. These are just modified version of what's available on the git repo site:https://github.com/achronal/nuskybgd-py 
```bash
mkdir bgd
```
##### Manually create bgd1.reg bgd2.reg bgd3.reg files and save it under the bgd folder just created

```bash
getspecnoarf.py nu60601028002A01_cl.evt reg=bgd/bgd1.reg  indir=. outdir=bgd outprefix=bgd1A attfile=../auxil/nu60601028002_att.fits.gz >& bgd/bgd1A.log  

getspecnoarf.py nu60601028002A01_cl.evt reg=bgd/bgd2.reg indir=. outdir=bgd outprefix=bgd2A attfile=../auxil/nu60601028002_att.fits.gz >& bgd/bgd2A.log  

getspecnoarf.py nu60601028002A01_cl.evt reg=bgd/bgd3.reg indir=. outdir=bgd outprefix=bgd3A attfile=../auxil/nu60601028002_att.fits.gz >& bgd/bgd3A.log  

getspecnoarf.py nu60601028002B01_cl.evt reg=bgd/bgd1.reg indir=. outdir=bgd outprefix=bgd1B attfile=../auxil/nu60601028002_att.fits.gz >& bgd/bgd1B.log  

getspecnoarf.py nu60601028002B01_cl.evt reg=bgd/bgd2.reg indir=. outdir=bgd outprefix=bgd2B attfile=../auxil/nu60601028002_att.fits.gz >& bgd/bgd2B.log  

getspecnoarf.py nu60601028002B01_cl.evt reg=bgd/bgd3.reg indir=. outdir=bgd outprefix=bgd3B attfile=../auxil/nu60601028002_att.fits.gz >& bgd/bgd3B.log
```


#### Changing executing the binary directly to calling it like a python script
```
python $NUSKYBGD/bin/nuskybgd mkinstrmap nu60601028002A01_cl.evt

python $NUSKYBGD/bin/nuskybgd mkinstrmap nu60601028002B01_cl.evt


python $NUSKYBGD/bin/nuskybgd aspecthist nu60601028002A_det1.fits gtifile=nu60601028002A01_gti.fits out=aspecthistA.fits

python $NUSKYBGD/bin/nuskybgd aspecthist nu60601028002B_det1.fits gtifile=nu60601028002B01_gti.fits out=aspecthistB.fits
```

#### Now go into the bgd folder...
```
cd bgd
python $NUSKYBGD/bin/nuskybgd projbgd refimg=../im3to20kev.fits out=bgdapA.fits mod=A det=1234 chipmap=../newinstrmapA.fits aspect=../aspecthistA.fits

python $NUSKYBGD/bin/nuskybgd projbgd refimg=../im3to20kev.fits out=bgdapB.fits mod=B det=1234 chipmap=../newinstrmapB.fits aspect=../aspecthistB.fits
```

#### ctrl+c when the prompt stopped updating
```
python $NUSKYBGD/bin/nuskybgd fit bgdinfo.json savefile=j1440bgd >& fit.log &
tail -f fit.log
```

#### follow the git repo page's instruction and start xspec, call the j1440bgd.xcm script.
We don't have extended sources in the region so we can just "fit"
Once you're done fitting, see if the background fit looks fine. If so, run "save all mymodel" in xspec and quit
