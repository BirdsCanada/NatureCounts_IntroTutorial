# Understanding and Viewing Data {#Data3}



This chapter begins with a brief introduction to the structure of the NatureCounts database, followed by a description of access levels and how to create a user account. We then provide instructions on how to view data from various collections and apply filters. 

## Data Structure {#Data3.1}

The [Bird Monitoring Data Exchange](https://www.birdscanada.org/birdmon/default/nc_bmde.jsp) (BMDE) was developed to be a standardized data exchange schema to promote the sharing and analysis of avian observational data. The schema is the core sharing standard of the [Avian Knowledge Network](http://www.avianknowledge.net).The BMDE (currently version 2.0) includes 169 *core* fields that are capable of capturing all metrics and descriptors associated with a bird observation. The BMDE schema was extended in 2018, and the *complete* version now includes 265 fields.

By default, the NatureCounts package downloads the data with the *minimum* set of fields/columns. However, for more advanced applications, users may wish to specify which fields/columns to return using the `field_set` and `fields` options in the `nc_data_dl` function. For help with this feature, see the GitHub article ['Selecting columns and fields to download'](https://birdstudiescanada.github.io/naturecounts/articles/selecting-fields.html).   

## Levels of Data Access {#Data3.2}

NatureCounts host many datasets, representing in excess of 140 million occurrence records, with a primary focus on Canadian bird monitoring data. Many of those datasets are from project lead by Birds Canada and/or its partners. While we thrive to make our data as openly available as possible, we also need to recognize the need of our partners and funders.

NatureCounts has five [Levels of Data Access](https://www.birdscanada.org/birdmon/default/nc_access_levels.jsp), which define how each dataset can be used. Those levels are set individually for each dataset, in consultation with the various partners and data custodians involved.

  - Level 0: most restricted (archival only)
  
  - Level 1: archival only, metadata visible
  
  - Level 2: data used for visualizations only
  
  - Level 3: data available to third parties by request
  
  - Level 4: data shared with external portals and available by request
  
  - Level 5: open access

All contributing members of NatureCounts have complete authority over the use of the data they have provided, and can withhold data at any time from any party or application. All users of any NatureCounts data must clearly acknowledge the contribution of the members who are making data available. Each dataset comes with its own [Data Sharing Policy](https://www.birdscanada.org/birdmon/default/nc_data_sharing.jsp) that defines the various conditions for data usage.

You can view the Data Access Level for each collection on the [NatureCounts Datasets](https://www.birdscanada.org/birdmon/default/datasets.jsp) page or using the [metadata](#Data3.5) function (see akn_level):


```r
meta_collections() 
```

## Authorizations {#Data3.3}

To access data using the NatureCounts R package, you must [sign up](https://www.birdscanada.org/birdmon/default/register.jsp) for a **free** account. Further, if you would like to access Level 3 or 4 collections you must make a [data request](https://www.birdscanada.org/birdmon/default/searchquery.jsp). For step-by-step visual instructions, we encourage you to watch: [NatureCounts: An Introductory Tutorial](link to be provided).

## Viewing information about NatureCounts collections {#Data3.4}

First, lets use the NatureCounts R package to view the number of records available for different collections. To do this we use the `nc_count` function. You can view *all* the available collections and the number of observations using the default setting. 

If a username is provided, the collections are filtered to only those available to the user. Otherwise all counts from all data sources are returned (default: show = "all").


```r
nc_count() 
```

Or you can view the collections for which you have access using your username/password. Here we use the *sample* account. If you use your own account, you will be prompted for a password. 


```r
nc_count(username = "sample")
```

Further refinements can be applied to the `nc_count` function using [filters](#Download4)  Options include: `collections`, `project_id`, `species`, `years`, `doy` (day-of-year), `region`, and `site_type`. 

## Metadata codes and decriptions {#Data3.5}

There are [metadata](https://birdstudiescanada.github.io/naturecounts/reference/meta.html) associated with the various arguments used in the `nc_count` and `nc_data_dl` functions, the latter you will use in [Chapter 4](#Download4). These are stored locally and can be accessed anytime to help filter your data view or download query. They include: 

  - meta_country_codes: country codes
  
  - meta_statprov_codes: state/Province codes
  
  - meta_subnational2_codes: subnational2 codes
  
  - meta_iba_codes: Important Bird Area (IBA) codes
  
  - meta_bcr_codes: Bird Conservation Region (BCR) codes
  
  - meta_utm_squares: UTM Square codes
  
  - meta_species_authority: species taxonomic authorities
  
  - meta_species_codes: alpha-numeric codes for avian species
  
  - meta_species_taxonomy: codes and taxonomic information for all species
  
  - meta_collections: collections names and descriptions
  
  - meta_breeding_codes: breeding codes and descriptions
  
  - meta_project_protocols: project protocols
  
  - meta_projects: projects ids, names, websites, and descriptions
  
  - meta_protocol_types: protocol types and descriptions

Comprehensive reference articles are available online for retrieving [Region](https://birdstudiescanada.github.io/naturecounts/articles/region-codes.html) and [Species](https://birdstudiescanada.github.io/naturecounts/articles/species-codes.html) codes. We do not repeat those material here, but encourage you to review these prior to proceeding. 

You can explore the metadata materials using one line of code. For example, you can view the Important Bird Area (iba) metadata using:


```r
meta_iba_codes()
```

## Examples {#Data3.6}

Here are a few examples for you to work through to become familiar with the `nc_count` function.  

*Example 1*: Determine the number of collections and records for a specific *region*. The options include: country, statprov, subnational2, iba, bcr, utm_squares, bbox. You can find details and examples on how to [search_region](https://birdstudiescanada.github.io/naturecounts/articles/region-codes.html) at the link provided.

The following code will retrieve all available collections and number of records for British Columbia

```r
search_region("British Columbia", type = "statprov")
nc_count(region = list(statprov = "BC"))
```

*Example 2*: Determine the number of records for a specific *species*. You can find details and examples on how to [search_species_code](https://birdstudiescanada.github.io/naturecounts/reference/search_species_code.html) based on common names and [search_species](https://birdstudiescanada.github.io/naturecounts/reference/search_species.html) based on 4 letter alpha code at the links provided.  

The following code will retrieve all available collections and number of records for Red-headed Woodpecker  

```r
search_species("Red-headed Woodpecker")
nc_count(species = 10060)
```

*Example 3*: We can further refine the Red-headed Woodpecker example (above) by filtering the species-specific data by region (e.g., [Bird Conservation Region](http://nabci-us.org/assets/images/bcr_map2.jpg) 11), time period (e.g., 2015-2019), or a combination of both.


```r
nc_count(species = 10060, region=list(bcr="11"))

nc_count(species = 10060, year=c(2015, 2019))

nc_count(species = 10060, region=list(bcr="11"), year=c(2015, 2019))
```

## Exercises {#Data3.7}

Now apply your newly acquired skills!

*Exercise 1*: If you are interesting in doing a research project on Snowy Owls in Quebec, which three collections are you most likely to consider using (i.e., which have the most data)?

Answer: EBird-CA-QC, OISEAUXQC, CBC 

*Exercise 2*: How many records of Gadwal are in the [British Columbia Coastal Waterbird Survey](https://www.birdscanada.org/birdmon/atowls/datasets.jsp?code=BCCWS) collection? What if you are only interest in records from 2010-2019, how many records are available? 

Answer: 702, 389



