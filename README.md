
# Flempar

Flempar is an interface to the API of the Flemish parliament.

## Installation

You can install the released version of Flempar from this repo with:

``` r
require(devtools)
install_github("PolscienceAntwerp/Flempar")
```

## Usage

Under an open data policy,the Flemish parliament publishes data on
parliamentary proceedings via API. This data source contains a large
amount of data on MPs, legislative work, voting and parliamentary
documents.

To boost performance it is possible to divide the calls over several
workers, enabling to shorten your waiting time.

``` r
library(Flempar)
library(dplyr)

# Get all the written questions between "2022-01-20" and "2022-01-31" in a dataframe
 wq_documents <- get_data(date_range_from="2022-01-20"
                           , date_range_to="2022-01-31"
                           , type="document"
                           , fact="written_questions"
                           , use_parallel=TRUE )
                           
# Get all the basic data on these written questions
 wq_basicdata <- get_data(date_range_from="2022-01-20"
                           , date_range_to="2022-01-31"
                           , type="basicdata"
                           , fact="written_questions"
                           , use_parallel=TRUE )
                           
# Join the content of the written questions with the basic data
  wq_documents %>%
    left_join(wq_basicdata, by=c("id_fact"="id_fact")) -> wq_documents_basicdata

# Get the bio information of the current MPs
  mp_bio <- get_mp(selection="current",fact="bio",use_parallel=TRUE)

# Join the the MP data with the basic data to see who is asking questions     
  wq_basicdata %>%
       left_join(mp_bio, by=c("id_vragensteller"="id_mp")) -> wq_basicdata_mp
             
```
