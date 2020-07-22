0. It's generally a good idea to have https://heasarc.gsfc.nasa.gov/docs/nustar/analysis/nustar_quickstart_guide.pdf and https://heasarc.gsfc.nasa.gov/docs/nustar/analysis/nustar_swguide.pdf for your reference.

0.1 In additional to heasoft, you should also have a calibration data base (CALDB) for NuSTAR. You can download it from here:
https://heasarc.gsfc.nasa.gov/docs/heasarc/caldb/caldb_supported_missions.html
Alternatively, you can try enabling the remote access option following the instructions here (calibration data base can be quite large):
https://heasarc.gsfc.nasa.gov/docs/heasarc/caldb/caldb_remote_access.html

1. Go to https://heasarc.gsfc.nasa.gov/db-perl/W3Browse/w3browse.pl
2. Select NuSTAR under "Most Requested Missions"
3. Type "IC 750" in the search box and search
4. Click "numaster:NuSTAR Master Catalog"
5. In the first row of the table (with obsid=60061217004), in the second column, select "D" to preview the data of this row.
6. check " Select all products in this row", and go to the end of the webpage, click TAR selected products. Wait a few seconds, once you see "TAR compete", you can download the data from the link in the webpage.
7. Once you've downloaded the data, go to terminal and enter the directory in which you placed the downloaded file, extract the TAR file by typing
```bash
tar -xvf your_file_name.tar
```
This will extract the tar file.
8. Assume you have HEASOFT succesfully installed on your laptop, there shuold be two scripts called "nupipeline" and "nuproducts" that you can run under terminal. You can type this to see where they are:
```bash
which nupipeline
which nuproducts
```

9. nupipeline, essenstially, is the same as chandra_repro that you ran using CIAO, so what you need to do is specify the path to which you put the data, and how the files are named (note that outdir can be anywhere you specified):
```bash
nupipeline indir=/path_to_nustar_data/60061217004/ \
 steminputs=nu60061217004 outdir=/path_to_nustar_data/60061217004/out
```
If you get any error messages here, try running nupipeline without any arguments, and set the parameters interactively. 

10. Once you've ran nupipeline (it takes a while), you can visually check the event lists by going into your outdir
```
ds9 
```

11. The source we are interested in is called IC 750, note that it's not the two luminous sourcces in the center of the FOV, IC 751, but you should see some weak signal at the coordinate of IC 751 (which you can find out using services such as NED). Once you've located the weak source, make a source region and a background region. 
You should also create regions for one of the luminous IC 751 source, which can be used as a comparison. 

12. With the regions specified, you can extract the spectra using this command:
```bash
nuproducts srcregionfile=your_source_region.reg bkgregionfile=your_background_region.reg indir=your_outdir_from_nupipeline  outdir=dir_to_put_your_products instrument=FPMA steminputs=nu60061217004 bkgextract=yes
```
Note that NuSTAR has two almost-identical instruments, FPMA and FPMB. You will need to repeat nuproducts with different instrument settings to get products for both FPMs. 
Again, if you encountered errors doing this, try running nuproducts directly and set the parameters interactively with the shell. 


13. Once you succesfully run nuproducts, you shuold have some spectral files for analysis. For noisy, low-count data, it is ususally beneficial to bin the data from different channels. It can be achieved by using the FTOOLS' grppha:
```

```

14. We can now use xspec or sherpa (there's also something called ISIS, https://space.mit.edu/home/mnowak/isis_vs_xspec/ ) to analyze the data. Since you are new to this you should spend sometime exploring these options, and pick the one you feel the most comfortable using. I do most of my X-ray spectral analysis using XSPEC.

