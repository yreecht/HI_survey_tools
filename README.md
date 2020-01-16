# Cruisenotebook
Create a quick cruise summary report detailing the survey track, trawl catches, acoustic registrations, and CTD data during or after a cruise. Can be set up to read data direcly from the servers on board.

## Create a project and folder structure

1. Create a new R Studio project.

2. Download the cruise report script from https://github.com/jofall/Cruisenotebook and place in project directory.

3. The script requires a specific folder structure for storing the cruise data. Create these folders in the project directory:
   + Data
      + Acoustic
         + Integrated
         + Channels
      + Biotic
      + Bird
      + CTD
      + Track
      + Whale
      + Definitions
      + GeoData
   
For acoustic, biotic and track data, it is also possible to replace the folder structure with paths directly to the location of data on the server on board. This way, the files in the script with be updated automatically when you knit the file. However, CTD data cannot be read directly from the server location with this version of the script (as there are several different types of .cnv-files).

4. Download the HI logo from https://hinnsiden.no/tema/profiltorg/LOGO/HI%20logo%20farger%20engelsk.jpg and place in project directory (optional, but see information below on how to avoid an error if you do not include the logo).

## Gather data

Collect the following cruise data from the server on board:

1. Acoustic data: *ListUserFile03.txt* and *ListUserFile16.txt*. If these reports have not been created, export them from LSSS.
   + Example path to acoustic reports on Johan Hjort: //nas1-jhjort/CRUISE_DATA_*YEAR*/  
   *cruise_name*/ACOUSTIC/LSSS/REPORTS/
   + Place in "Acoustic" folder.

2. Trawl data: The *.xml* file exported from Sea2Data. 
   + Example path from Johan Hjort: //nas1-jhjort/CRUISE_DATA_*YEAR*/*cruise_name*/  
   BIOLOGY/CATCH_MEASUREMENTS/BIOTIC/
   + Place in "Biotic" folder.

3. Seabird data: Data sheet from bird observer (ods-format, if other format change code for reading in this file at line 1732 in the script) and the file *"Field sheet, Methodology, species codes NINA.xls"*. Place in folder "Birds".

4. CTD data: *ctdsort.cnv*-files (one for each CTD cast).
   + Example path from Johan Hjort: //nas1-jhjort/CRUISE_DATA_*YEAR*/*cruise_name*/  
   PHYSICS/CTD/CTD_DATA/
   + Place in "CTD" folder.

5. Definitions: These are files used for reading the biotic xlm-data. Download them from https://github.com/Sea2Data/cruisetools and place in folder "Definitions".

6. Map data: depth contours for plotting. Download the file *ETOPO1_nm4.csv* from the github repository and place in folder "GeoData".

7. Cruise track data: .csv-files (one for each day).
   + Example path from Johan Hjort:
   //nas1-jhjort/CRUISE_DATA_*YEAR*/*cruise_name*/  
   CRUISE_LOG/TRACK/
   + Place in "Track" folder.

8. Whale data: Excel-file with sightings and the *Miljø.log*-file (from either the starboard or port side) from the whale observers. Place in "Whale" folder.

NB! It is important not to place other files in these folders, as the code is based on loading files with specific file extensions.

## Change or check the following in the script to adapt the report to specific survey areas and species in the catch

1. Remove line 3 if you do not wish to have the HI logo.

2. Change title and author.

3. Check the r packages specified in the second r code chunk (lines 23-24), and install any packages you do not have on your computer.

4. Lines 40-42: specify the extent of your survey area and the depth contours you would like to show in map plots.

### Trawl data

5. Load packages by running lines 23-25, then run lines 37-72. Run unique(dataset$`Station type`) and check that the gear definitions on lines 75-83 correspond to those used in your cruise. If not, change the definitions.

6. Check if you are using any other trawls than those specified at lines 495-497, if so add them/modify.

7. Lines 622-955 contain code for plotting catches of different species on a map, by gear type. For each species, two sets of plots are produced - one showing catch rates in biomass/nmi and one showing numbers/nmi. Adjust this code to match the species in your area by changing the "species <-" argument in each code chunk, and deleting/copying code chunks as necessary. The figure size can be adjusted using the "fig.width" and "fig.height" arguments at the beginning of each code chunk. In this example, this has been done for herring and shrimp, which were caught in fewer gears than the other species.

8. Lines 971-972: specify the species that you wish to plot length distributions, length-weight relationships, and length-age relationships for. Here they are divided into larger (demersal) and smaller (pelagic) species. Note that age data will not be available early in the survey, and will later only be available for species whose otholits are read on board.

9. Line 1027: This inline code summarises the total distance of scrutinized acoustic transects, accounting for the possibility of a reset in the log values, i.e., that the log reaches 10000 and then starts over from 1. No change necessary.

10. Line 1029: In this section it is possible to add screenshots from LSSS (or other images) in case you find particularly interesting or unusual echograms. The screenshot is placed in the project directory, and the filename inserted within the brackets on line 1031. The figure width can be adjusted within the curly brackets. Remove line 1031 and the figure text at line 1033 in case you have no screenshots to add.

### Acoustics

11. Line 1041: For acoustic plots, the maximum bubble size can be adjusted here.

12. Line 1044: Change year manually in case you are using this script to plot data from a past year.

13. Line 1045: This script selects data from the 38 kHz frequency, which is what is normally exported from LSSS. Change here if you are looking at other frequencies.

14. Lines 1058-1060: If you want other names for the acoustic categories than the Norwegian output from LSSS, specify labels here. "Comment out" or delete lines 1062-1064 & 1200-1202 if you don't want to change the names.

15. Line 1069: Check name of survey and vessel.

16. Line 1091/1111: Specify names of pelagic/demersal acoustic categories that you want to plot on a map.

### CTD

17. Line 1403: Choose the maximum depth for plots of density and irradiance.

18. Lines 1457-1458: For figures of diel variation in acoustic backscatter - choose layerdepth, i.e., the lower depth of the layer over which acoustic backscatter will be integrated and plotted against light level (upper limit is the surface). Also choose a suitable bottom depth, i.e., it needs to be deep enough for the chosen depth layer to move deeper. Example: if we want to look at diel variation in backscatter in the upper 100 m, we set layerdepth = 100 m and bottomdepth to, e.g., 200 m so that we only look at samples taken in areas where the scatterers in the upper 100 m can move deeper.

19. Line 1524: Choose maximum distance in km between acoustic registrations and CTD measurements. Acoustic registrations farther away from a CTD will not be included in the figures.

### Whales

20. Lines 1624-1628: If other species have been observed, specify names here. 

21. Line 1651: This line was required to clean some old entries from the file that belonged to a different cruise. "Comment out" or delete if this does not apply to your cruise.

22. Lines 1689-1720: These plots assume that you are in an area where white-beaked dolphins are much more abundant than the other species, and they therefore get their own plot. If this does not apply, the two plots can be combined into one by deleting the second block of code and removing "%>% dplyr::filter(!species == "White-beaked dolphin")" from the first plot.

### Birds

23. Lines 1818-1837: These two figures show histograms of the total number observed, by species. As some species occur in higher numbers than others, one figure shows species that are more abundant while the other shows those occuring in lower numbers. The split between the figures can be specified at line 1819.

24. The last set of figures show the spatial distribution of bird observations. These are divided into three plots based on observed numbers - low, medium, and high. The split between these three plots can be adjusted at lines 1862, 1880, and 1897 (e.g., "filter(maxN>25)").


---

- info about cache=true, when you don't want to read in new data, just update other things
in the file when you knit

- info on how to adjust size of figures - width, height, out width, resolution, dpi