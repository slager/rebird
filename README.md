
<!-- README.md is generated from README.Rmd. Please edit that file -->

# rebird: wrapper to the eBird API

[![Project Status: Active – The project has reached a stable, usable
state and is being actively
developed.](http://www.repostatus.org/badges/latest/active.svg)](http://www.repostatus.org/#active)
[![Build
Status](https://api.travis-ci.org/ropensci/rebird.png)](https://travis-ci.org/ropensci/rebird)
[![Build
status](https://ci.appveyor.com/api/projects/status/s3dobn991c20t2kg?svg=true)](https://ci.appveyor.com/project/sckott/rebird)
[![cran
checks](https://cranchecks.info/badges/worst/rebird)](https://cranchecks.info/pkgs/rebird)
[![Coverage
Status](https://coveralls.io/repos/ropensci/rebird/badge.svg)](https://coveralls.io/r/ropensci/rebird)
[![rstudio mirror
downloads](http://cranlogs.r-pkg.org/badges/rebird)](https://github.com/metacran/cranlogs.app)
[![cran
version](http://www.r-pkg.org/badges/version/rebird)](https://cran.r-project.org/package=rebird/)

`rebird` is a package to interface with the eBird webservices.

eBird is a real-time, online bird checklist program. For more
information, visit their website: <http://www.ebird.org>

The API for the eBird webservices can be accessed here:
<https://documenter.getpostman.com/view/664302/S1ENwy59?version=latest>

## Install

You can install the stable version from CRAN

``` r
install.packages("rebird")
```

Or the development version from Github

``` r
install.packages("devtools")
devtools::install_github("ropensci/rebird")
```

# Direct use of `rebird`

Load the package:

``` r
library("rebird")
```

The [eBird API
server](https://documenter.getpostman.com/view/664302/S1ENwy59?version=latest)
has been updated and thus there are a couple major changes in the way
`rebird` works. API requests to eBird now require users to provide an
API key, which is linked to your eBird user account. You can pass it to
the ‘key’ argument in `rebird` functions, but we highly recommend
storing it as an environment variable called EBIRD\_KEY in your
.Renviron file. If you don’t have a key, you can obtain one from
<https://ebird.org/api/keygen>.

You can keep your .Renviron file in your global R home directory
(`R.home()`), your user’s home directory (`Sys.getenv("HOME")`), or your
current working directory (`getwd()`). Remember that .Renviron is loaded
once when you start R, so if you add your API key to the file you will
have to restart your R session. See
<https://csgillespie.github.io/efficientR/r-startup.html> for more
information on R’s startup files.

Furthermore, functions now use species codes, rather than scientific
names, for species-specific requests. We’ve made the switch easy by
providing the `species_code` function, which converts a scientific name
to its species code:

``` r
species_code('sula variegata')
#> Peruvian Booby (Sula variegata): perboo1
#> [1] "perboo1"
```

The `species_code` function can be called within other `rebird`
functions, or the species code can be specified directly.

## Sightings at location determined by latitude/longitude

Search for bird occurrences by latitude and longitude point

``` r
ebirdgeo(species = species_code('spinus tristis'), lat = 42, lng = -76)
#> American Goldfinch (Spinus tristis): amegfi
#> # A tibble: 26 x 12
#>    speciesCode comName sciName locId locName obsDt howMany   lat   lng
#>    <chr>       <chr>   <chr>   <chr> <chr>   <chr>   <int> <dbl> <dbl>
#>  1 amegfi      Americ… Spinus… L100… Bare V… 2019…       7  41.8 -75.9
#>  2 amegfi      Americ… Spinus… L447… Bingha… 2019…       6  42.1 -76.0
#>  3 amegfi      Americ… Spinus… L275… "Home " 2019…       1  42.1 -76.0
#>  4 amegfi      Americ… Spinus… L505… Boland… 2019…       1  42.2 -75.9
#>  5 amegfi      Americ… Spinus… L351… Anson … 2019…      25  42.1 -76.1
#>  6 amegfi      Americ… Spinus… L524… Victor… 2019…       3  42.1 -76.0
#>  7 amegfi      Americ… Spinus… L846… 31 Pul… 2019…       2  42.2 -76.2
#>  8 amegfi      Americ… Spinus… L217… Vestal  2019…      11  42.1 -76.0
#>  9 amegfi      Americ… Spinus… L211… Tri-Ci… 2019…       2  42.1 -76.1
#> 10 amegfi      Americ… Spinus… L166… Chugnu… 2019…      NA  42.1 -76.0
#> # … with 16 more rows, and 3 more variables: obsValid <lgl>,
#> #   obsReviewed <lgl>, locationPrivate <lgl>
```

## Recent observations at a region

Search for bird occurrences by region and species name

``` r
ebirdregion(loc = 'US', species = 'btbwar')
#> # A tibble: 1,493 x 12
#>    speciesCode comName sciName locId locName obsDt howMany   lat   lng
#>    <chr>       <chr>   <chr>   <chr> <chr>   <chr>   <int> <dbl> <dbl>
#>  1 btbwar      Black-… Setoph… L668… Naples… 2019…       1  26.1 -81.7
#>  2 btbwar      Black-… Setoph… L399… Florid… 2019…       1  26.4 -80.1
#>  3 btbwar      Black-… Setoph… L900… 1301 S… 2019…       2  26.3 -80.1
#>  4 btbwar      Black-… Setoph… L486… Union … 2019…       1  40.7 -74.0
#>  5 btbwar      Black-… Setoph… L100… "Parki… 2019…       1  26.1 -80.1
#>  6 btbwar      Black-… Setoph… L685… Hillsb… 2019…       2  26.3 -80.2
#>  7 btbwar      Black-… Setoph… L616… Lake L… 2019…       3  35.9 -78.7
#>  8 btbwar      Black-… Setoph… L871… Eno Ri… 2019…       1  36.1 -79.0
#>  9 btbwar      Black-… Setoph… L994… Jupite… 2019…       1  27.0 -80.1
#> 10 btbwar      Black-… Setoph… L127… Hugh T… 2019…       1  26.1 -80.1
#> # … with 1,483 more rows, and 3 more variables: obsValid <lgl>,
#> #   obsReviewed <lgl>, locationPrivate <lgl>
```

## Recent observations at hotspots

Search for bird occurrences by a given hotspot

``` r
ebirdregion(loc = 'L99381')
#> # A tibble: 68 x 12
#>    speciesCode comName sciName locId locName obsDt howMany   lat   lng
#>    <chr>       <chr>   <chr>   <chr> <chr>   <chr>   <int> <dbl> <dbl>
#>  1 cangoo      Canada… Branta… L993… Stewar… 2019…       8  42.5 -76.5
#>  2 mallar3     Mallard Anas p… L993… Stewar… 2019…      25  42.5 -76.5
#>  3 ribgul      Ring-b… Larus … L993… Stewar… 2019…      30  42.5 -76.5
#>  4 hergul      Herrin… Larus … L993… Stewar… 2019…      35  42.5 -76.5
#>  5 gbbgul      Great … Larus … L993… Stewar… 2019…       6  42.5 -76.5
#>  6 doccor      Double… Phalac… L993… Stewar… 2019…     250  42.5 -76.5
#>  7 rebwoo      Red-be… Melane… L993… Stewar… 2019…       1  42.5 -76.5
#>  8 dowwoo      Downy … Dryoba… L993… Stewar… 2019…       3  42.5 -76.5
#>  9 norfli      Northe… Colapt… L993… Stewar… 2019…       1  42.5 -76.5
#> 10 blujay      Blue J… Cyanoc… L993… Stewar… 2019…       8  42.5 -76.5
#> # … with 58 more rows, and 3 more variables: obsValid <lgl>,
#> #   obsReviewed <lgl>, locationPrivate <lgl>
```

## Nearest observations of a species

Search for a species’ occurrences near a given latitude and longitude

``` r
nearestobs(species_code('branta canadensis'), 42, -76)
#> Canada Goose (Branta canadensis): cangoo
#> # A tibble: 36 x 12
#>    speciesCode comName sciName locId locName obsDt howMany   lat   lng
#>    <chr>       <chr>   <chr>   <chr> <chr>   <chr>   <int> <dbl> <dbl>
#>  1 cangoo      Canada… Branta… L147… Quaker… 2019…       2  42.0 -75.9
#>  2 cangoo      Canada… Branta… L978… Murphy… 2019…      NA  42.1 -76.0
#>  3 cangoo      Canada… Branta… L207… Workwa… 2019…       4  42.1 -75.9
#>  4 cangoo      Canada… Branta… L245… Water … 2019…      20  42.1 -75.9
#>  5 cangoo      Canada… Branta… L274… River … 2019…      25  42.1 -76.0
#>  6 cangoo      Canada… Branta… L255… Wall S… 2019…       1  42.1 -75.9
#>  7 cangoo      Canada… Branta… L446… PA-SQ-… 2019…      25  41.8 -75.9
#>  8 cangoo      Canada… Branta… L179… Joyce … 2019…       6  41.8 -75.9
#>  9 cangoo      Canada… Branta… L246… Martin… 2019…       2  42.1 -75.9
#> 10 cangoo      Canada… Branta… L166… Chugnu… 2019…      40  42.1 -76.0
#> # … with 26 more rows, and 3 more variables: obsValid <lgl>,
#> #   obsReviewed <lgl>, locationPrivate <lgl>
```

## Frequency of observations at hotspots or regions

Obtain historical frequencies of bird occurrences by hotspot or region

``` r
ebirdfreq(loctype = 'hotspots', loc = 'L196159')
#> # A tibble: 9,408 x 4
#>    comName                     monthQt   frequency sampleSize
#>    <chr>                       <chr>         <dbl>      <dbl>
#>  1 Snow Goose                  January-1     0             33
#>  2 Greater White-fronted Goose January-1     0             33
#>  3 Cackling Goose              January-1     0             33
#>  4 Canada Goose                January-1     0             33
#>  5 Cackling/Canada Goose       January-1     0             33
#>  6 Trumpeter Swan              January-1     0             33
#>  7 Wood Duck                   January-1     0.152         33
#>  8 Blue-winged Teal            January-1     0             33
#>  9 Cinnamon Teal               January-1     0             33
#> 10 Blue-winged/Cinnamon Teal   January-1     0             33
#> # … with 9,398 more rows
```

## Recent notable sightings

Search for notable sightings at a given latitude and longitude

``` r
ebirdnotable(lat = 42, lng = -70)
#> # A tibble: 1,710 x 12
#>    speciesCode comName sciName locId locName obsDt howMany   lat   lng
#>    <chr>       <chr>   <chr>   <chr> <chr>   <chr>   <int> <dbl> <dbl>
#>  1 buhvir      Blue-h… Vireo … L100… 92–98 … 2019…       1  42.4 -72.6
#>  2 yebcuc      Yellow… Coccyz… L167… Rachel… 2019…       1  43.4 -70.4
#>  3 redhea      Redhead Aythya… L131… Sabatt… 2019…       1  44.1 -70.1
#>  4 pinwar      Pine W… Setoph… L345… Home    2019…       1  42.3 -72.4
#>  5 comgal1     Common… Gallin… L100… Trusto… 2019…       1  41.4 -71.6
#>  6 tufduc      Tufted… Aythya… L100… Trusto… 2019…       1  41.4 -71.6
#>  7 grycat      Gray C… Dumete… L271… Dahl W… 2019…       1  44.0 -71.1
#>  8 trokin      Tropic… Tyrann… L593… Rock M… 2019…       1  42.4 -71.2
#>  9 reevir1     Red-ey… Vireo … L813… P. J. … 2019…       1  41.7 -70.9
#> 10 wessan      Wester… Calidr… L830… Plum I… 2019…       1  42.8 -70.8
#> # … with 1,700 more rows, and 3 more variables: obsValid <lgl>,
#> #   obsReviewed <lgl>, locationPrivate <lgl>
```

or a region

``` r
ebirdnotable(locID = 'US-NY-109')
#> # A tibble: 49 x 12
#>    speciesCode comName sciName locId locName obsDt howMany   lat   lng
#>    <chr>       <chr>   <chr>   <chr> <chr>   <chr>   <int> <dbl> <dbl>
#>  1 blkvul      Black … Coragy… L212… Steven… 2019…       2  42.4 -76.4
#>  2 blkvul      Black … Coragy… L212… Steven… 2019…       2  42.4 -76.4
#>  3 whevir      White-… Vireo … L100… Monroe… 2019…       1  42.4 -76.5
#>  4 whevir      White-… Vireo … L542… Norths… 2019…       1  42.4 -76.5
#>  5 whevir      White-… Vireo … L100… 101–19… 2019…       1  42.4 -76.5
#>  6 andduc1     Andean… Oxyura… L100… Bogota  2019…       2  42.5 -76.5
#>  7 whtdov1     White-… Leptot… L100… Bogota  2019…      11  42.5 -76.5
#>  8 whtdov      White-… Leptot… L100… Bogota  2019…       3  42.5 -76.5
#>  9 wfwduc1     White-… Dendro… L100… Bogota  2019…       1  42.5 -76.5
#> 10 musduc      Muscov… Cairin… L100… Bogota  2019…       3  42.5 -76.5
#> # … with 39 more rows, and 3 more variables: obsValid <lgl>,
#> #   obsReviewed <lgl>, locationPrivate <lgl>
```

## Historic Observations

Search for historic observations on a date at a region

``` r
ebirdhistorical(loc = 'US-VA-003', date = '2019-02-14',max = 10)
#> # A tibble: 10 x 12
#>    speciesCode comName sciName locId locName obsDt howMany   lat   lng
#>    <chr>       <chr>   <chr>   <chr> <chr>   <chr>   <int> <dbl> <dbl>
#>  1 cangoo      Canada… Branta… L139… Lickin… 2019…      30  38.1 -78.7
#>  2 mallar3     Mallard Anas p… L139… Lickin… 2019…       5  38.1 -78.7
#>  3 gnwtea      Green-… Anas c… L139… Lickin… 2019…       8  38.1 -78.7
#>  4 killde      Killde… Charad… L139… Lickin… 2019…       1  38.1 -78.7
#>  5 baleag      Bald E… Haliae… L139… Lickin… 2019…       1  38.1 -78.7
#>  6 belkin1     Belted… Megace… L139… Lickin… 2019…       1  38.1 -78.7
#>  7 carwre      Caroli… Thryot… L139… Lickin… 2019…       1  38.1 -78.7
#>  8 whtspa      White-… Zonotr… L139… Lickin… 2019…       2  38.1 -78.7
#>  9 norcar      Northe… Cardin… L139… Lickin… 2019…       1  38.1 -78.7
#> 10 canvas      Canvas… Aythya… L331… Montic… 2019…      19  38.0 -78.5
#> # … with 3 more variables: obsValid <lgl>, obsReviewed <lgl>,
#> #   locationPrivate <lgl>
```

or a hotspot

``` r
ebirdhistorical(loc = 'L196159', date = '2019-02-14', fieldSet = 'full')
#> # A tibble: 14 x 27
#>    speciesCode comName sciName locId locName obsDt howMany   lat   lng
#>    <chr>       <chr>   <chr>   <chr> <chr>   <chr>   <int> <dbl> <dbl>
#>  1 annhum      Anna's… Calypt… L196… Vancou… 2019…       4  49.3 -123.
#>  2 ribgul      Ring-b… Larus … L196… Vancou… 2019…       4  49.3 -123.
#>  3 glwgul      Glauco… Larus … L196… Vancou… 2019…      29  49.3 -123.
#>  4 norcro      Northw… Corvus… L196… Vancou… 2019…     100  49.3 -123.
#>  5 bkcchi      Black-… Poecil… L196… Vancou… 2019…      16  49.3 -123.
#>  6 bushti      Bushtit Psaltr… L196… Vancou… 2019…      20  49.3 -123.
#>  7 pacwre1     Pacifi… Troglo… L196… Vancou… 2019…       1  49.3 -123.
#>  8 houfin      House … Haemor… L196… Vancou… 2019…       2  49.3 -123.
#>  9 purfin      Purple… Haemor… L196… Vancou… 2019…       3  49.3 -123.
#> 10 amegfi      Americ… Spinus… L196… Vancou… 2019…      15  49.3 -123.
#> 11 daejun      Dark-e… Junco … L196… Vancou… 2019…      37  49.3 -123.
#> 12 sonspa      Song S… Melosp… L196… Vancou… 2019…      12  49.3 -123.
#> 13 spotow      Spotte… Pipilo… L196… Vancou… 2019…       1  49.3 -123.
#> 14 rewbla      Red-wi… Agelai… L196… Vancou… 2019…       6  49.3 -123.
#> # … with 18 more variables: obsValid <lgl>, obsReviewed <lgl>,
#> #   locationPrivate <lgl>, subnational2Code <chr>, subnational2Name <chr>,
#> #   subnational1Code <chr>, subnational1Name <chr>, countryCode <chr>,
#> #   countryName <chr>, userDisplayName <chr>, subId <chr>, obsId <chr>,
#> #   checklistId <chr>, presenceNoted <lgl>, hasComments <lgl>,
#> #   firstName <chr>, lastName <chr>, hasRichMedia <lgl>
```

## Information on a given region or hotspot

Obtain detailed information on any valid eBird region

``` r
ebirdregioninfo("CA-BC-GV")
#> # A tibble: 1 x 5
#>   region                                     minX  maxX  minY  maxY
#>   <chr>                                     <dbl> <dbl> <dbl> <dbl>
#> 1 Metro Vancouver, British Columbia, Canada -123. -122.  49.0  49.6
```

or hotspot

``` r
ebirdregioninfo("L196159")
#> # A tibble: 1 x 16
#>   locId name  latitude longitude countryCode countryName subnational1Name
#>   <chr> <chr>    <dbl>     <dbl> <chr>       <chr>       <chr>           
#> 1 L196… Vanc…     49.3     -123. CA          Canada      British Columbia
#> # … with 9 more variables: subnational1Code <chr>, subnational2Code <chr>,
#> #   subnational2Name <chr>, isHotspot <lgl>, locName <chr>, lat <dbl>,
#> #   lng <dbl>, hierarchicalName <chr>, locID <chr>
```

## `rebird` and other packages

### How to use `rebird`

This package is part of a richer suite called [spocc - Species
Occurrence Data](https://github.com/ropensci/spocc), along with several
other packages, that provide access to occurrence records from multiple
databases. We recommend using `spocc` as the primary R interface to
`rebird` unless your needs are limited to this single source.

### `auk` vs. `rebird`

Those interested in eBird data may also want to consider
[`auk`](https://github.com/CornellLabofOrnithology/auk), an R package
that helps extracting and processing the whole eBird dataset. The
functions in `rebird` are faster but mostly limited to accessing recent
(i.e. within the last 30 days) observations, although `ebirdfreq()` does
provide historical frequency of observation data. In contrast, `auk`
gives access to the full set of \~ 500 million eBird observations. For
most ecological applications, users will require `auk`; however, for
some use cases, e.g. building tools for birders, `rebird` provides a
quicker and easier way to access data. `rebird` and `auk` are both part
of the rOpenSci project.

## API requests covered by `rebird`

The 2.0 APIs have considerably been expanded from the previous version,
and `rebird` only covers some of them. The webservices covered are
listed below; if you’d like to contribute wrappers to APIs not yet
covered by this package, feel free to submit a pull request\!

### data/obs

  - [x] Recent observations in a region: `ebirdregion()`
  - [x] Recent notable observations in a region: `ebirdnotable()`
  - [x] Recent observations of a species in a region: `ebirdregion()`
  - [x] Recent nearby observations: `ebirdgeo()`
  - [x] Recent nearby observations of a species: `ebirdgeo()`
  - [x] Nearest observations of a species: `nearestobs()`
  - [x] Recent nearby notable observations: `ebirdnotable()`
  - [x] Historic observations on a date: `ebirdhistorical()`

### product

  - [ ] Top 100
  - [ ] Checklist feed on a date
  - [ ] Recent checklists feed
  - [ ] Regional statistics on a date
  - [ ] View Checklist BETA

### ref/geo

  - [ ] Adjacent Regions

### ref/hotspot

  - [ ] Hotspots in a region
  - [ ] Nearby hotspots

### ref/taxonomy

  - [x] eBird Taxonomy: `ebirdtaxonomy()`
  - [ ] Taxonomic Forms
  - [ ] Taxonomy Versions
  - [ ] Taxonomic Groups

### ref/region

  - [x] Hotspot Info: `ebirdregioninfo()`
  - [x] Region Info: `ebirdregioninfo()`
  - [ ] Sub Region List

## Meta

  - Please [report any issues or
    bugs](https://github.com/ropensci/rebird/issues).
  - License: MIT
  - Get citation information for `rebird` in R doing `citation(package =
    'rebird')`

[![ropensci\_footer](http://ropensci.org/public_images/github_footer.png)](http://ropensci.org)
