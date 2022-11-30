<img src="Cyclistic.png" style="width:30.0%" />

## About the company

Cyclistic is a bike-share program that features more than 5,800 bicycles
and 700 docking stations across Chicago. The bikes can be unlocked from
one station and returned to any other station in the system anytime.

Cyclistic’s marketing strategy relied on building general awareness and
appealing to broad customer segments. One approach that helped make
these things possible was the flexiblity of its pricing plans:
single-ride passes, full-day passes, and annual membership. Customers
who purchase **single-ride or full-day passes are referred to as casual
riders**. Customers who purchase **annual membership are Cyclistic
members**.

The director of marketing believes the company’s future success depends
on maximizing the number of annual membership. Therefore, as a Data
Analyst, our job is to find and analyze any pattern or trend in
Cyclistic historical bike trip data to understand how casual riders and
annual members use Cyclistic bikes differently. From these insights, we
can create a new marketing strategy to convert casual riders to annual
members.

## Business Task

**Identify any pattern or trend** in Cyclistic historical bike trip data
to understand the **difference between casual riders and annual
members** in using Cyclistic bikes so we can create a new marketing
strategy to **convert casual riders to annual members**.

## Key Stakeholders:

-   **Lily Moreno**: The director of marketing and your manager. Moreno
    is responsible for the development of campaigns and initiatives to
    promote the bike-share program. These may include email, social
    media, and other channels.
-   **Cyclistic marketing analytics team**: A team of data analysts who
    are responsible for collecting, analyzing, and reporting data that
    helps guide Cyclistic marketing strategy. You joined this team six
    months ago and have been busy learning about Cyclistic’s mission and
    business goals — as well as how you, as a junior data analyst, can
    help Cyclistic achieve them.
-   **Cyclistic executive team**: The notoriously detail-oriented
    executive team will decide whether to approve the recommended
    marketing program.

## Data Preparation

The dataset I use was acquired from [Divvy
Tripdata.](https://divvy-tripdata.s3.amazonaws.com/index.html) The data
has been made available for public by Motivate International Inc. under
this [license.](https://ride.divvybikes.com/data-license-agreement). For
this capstone project, I use data from October 2021 to September 2022
(12 months). I use R for combining and cleaning the dataset that
contains a lot of rows (more than 5 million) which Spreadsheet cannot
handle.

#### Prepare the environment

First, we need to load the library.

    #Load the library
    library(tidyverse)
    library(lubridate)
    library(summarytools)
    library(data.table)
    library(hms)
    #Set the working directory
    setwd('D:/DatSci & Analyst/Google Data Analytics/CAPSTONE Project')

Then, we have to combine the Cyclistic Bike-Share Datatrip.

    #Combine the Cyclistic Bike Dataset from October 2021 to September 2022
    filenames <- list.files(path='D:/DatSci & Analyst/Google Data Analytics/CAPSTONE Project/Dataset', full.names=TRUE)

    #Read all csv files from filenames
    cyclist<-rbindlist(lapply(filenames,fread))

Before we clean our data, we need a summary statistics for all variables
in the dataframe. We use dfSummary function from summarytools library
and set ASCII to false for better printing. A summary table are useful
for checking data type, validity, and missing data.

    plain.ascii = FALSE
    print(dfSummary(cyclist, graph.magnif = 0.75), method='render')

<div class="container st-container">
<h3>Data Frame Summary</h3>
<h4>cyclist</h4>
<strong>Dimensions</strong>: 5828235 x 13
  <br/><strong>Duplicates</strong>: 0
<br/>
<table class="table table-striped table-bordered st-table st-table-striped st-table-bordered st-multiline ">
  <thead>
    <tr>
      <th align="center" class="st-protect-top-border"><strong>No</strong></th>
      <th align="center" class="st-protect-top-border"><strong>Variable</strong></th>
      <th align="center" class="st-protect-top-border"><strong>Stats / Values</strong></th>
      <th align="center" class="st-protect-top-border"><strong>Freqs (% of Valid)</strong></th>
      <th align="center" class="st-protect-top-border"><strong>Graph</strong></th>
      <th align="center" class="st-protect-top-border"><strong>Valid</strong></th>
      <th align="center" class="st-protect-top-border"><strong>Missing</strong></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td align="center">1</td>
      <td align="left">ride_id
[character]</td>
      <td align="left" style="padding:8;vertical-align:middle"><table style="border-collapse:collapse;border:none;margin:0"><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">1. 00000123F60251E6</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">2. 00000179CF2C4FB5</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">3. 0000047373295F85</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">4. 000004C3185FDDE9</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">5. 000005B1F6F86B03</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">6. 000008FF2B1BB8EC</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">7. 00000B26583EB490</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">8. 00000E22FBA89D81</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">9. 00000E408DED6BFB</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">10. 00000EBBC119168C</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">[ 5828225 others ]</td></tr></table></td>
      <td align="left" style="padding:0;vertical-align:middle"><table style="border-collapse:collapse;border:none;margin:0"><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">1</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.0%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">1</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.0%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">1</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.0%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">1</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.0%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">1</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.0%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">1</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.0%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">1</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.0%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">1</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.0%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">1</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.0%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">1</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.0%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">5828225</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">100.0%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr></table></td>
      <td align="left" style="vertical-align:middle;padding:0;background-color:transparent;"><img style="border:none;background-color:transparent;padding:0;max-width:max-content;" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAHwAAADKBAMAAACVoznlAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqbw8PD///+xh0SBAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5gseDB0ex6brmQAAAINJREFUaN7t1LEVgCAQREFasAS1A+2/NxMbYOEhPmbzCS64X8ra27dsOI7/j4e6E//2dhzHR/JQqw2Or8dDPQdvvB3H8ZE81HKB43glD7Xa4DheyUOtNjiOV/JQqw2Or8dDPQdvvB3H8ZE81HKB43glD3UnfoR7+XlHu3AcT3jjw666BwRfSgCp4Ik5AAAAJXRFWHRkYXRlOmNyZWF0ZQAyMDIyLTExLTMwVDEyOjI5OjMwKzAwOjAwFHaOPQAAACV0RVh0ZGF0ZTptb2RpZnkAMjAyMi0xMS0zMFQxMjoyOTozMCswMDowMGUrNoEAAAAASUVORK5CYII="></td>
      <td align="center">5828235
(100.0%)</td>
      <td align="center">0
(0.0%)</td>
    </tr>
    <tr>
      <td align="center">2</td>
      <td align="left">rideable_type
[character]</td>
      <td align="left" style="padding:8;vertical-align:middle"><table style="border-collapse:collapse;border:none;margin:0"><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">1. classic_bike</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">2. docked_bike</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">3. electric_bike</td></tr></table></td>
      <td align="left" style="padding:0;vertical-align:middle"><table style="border-collapse:collapse;border:none;margin:0"><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">2740516</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">47.0%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">192475</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">3.3%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">2895244</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">49.7%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr></table></td>
      <td align="left" style="vertical-align:middle;padding:0;background-color:transparent;"><img style="border:none;background-color:transparent;padding:0;max-width:max-content;" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAEQAAAA7BAMAAAAwSTGVAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqbw8PD///+xh0SBAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5gseDB0i6MmXHgAAAEdJREFUSMdjYBhcQAknUBSEKlE2xgVGlQwhJUTEtCAeQFUlSkoElSgbjSoZBkqIiGlBQTopUcIDCOYjY6NRJUNGCRExPVgAAKe0xVuwsVrMAAAAJXRFWHRkYXRlOmNyZWF0ZQAyMDIyLTExLTMwVDEyOjI5OjM0KzAwOjAw4DmqLgAAACV0RVh0ZGF0ZTptb2RpZnkAMjAyMi0xMS0zMFQxMjoyOTozNCswMDowMJFkEpIAAAAASUVORK5CYII="></td>
      <td align="center">5828235
(100.0%)</td>
      <td align="center">0
(0.0%)</td>
    </tr>
    <tr>
      <td align="center">3</td>
      <td align="left">started_at
[POSIXct, POSIXt]</td>
      <td align="left" style="padding:8;vertical-align:middle"><table style="border-collapse:collapse;border:none;margin:0"><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">min : 2021-10-01 00:00:09</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">med : 2022-06-08 06:41:28</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">max : 2022-09-30 23:59:56</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">range : 11m 29d 23H 59M 47S</td></tr></table></td>
      <td align="left" style="vertical-align:middle">4875181 distinct values</td>
      <td align="left" style="vertical-align:middle;padding:0;background-color:transparent;"><img style="border:none;background-color:transparent;padding:0;max-width:max-content;" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAHQAAABUBAMAAAChGA4iAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqbw8PD///+xh0SBAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5gseDB8XjEwxvwAAAOtJREFUWMPt1WEOgyAMBWCvMG6wxw3o/e82iqiDZIw+s6kRfmAMfApi22m6V3v0NKC4tVAf/k8dEt1WbaASafYU1S4+AGYKKI19MFMJSvXKUfBUrkHTx+VoJgvFs4+6OLGislEdbWwTaFBpUi+DHkbjb8ZSHwYdtKb4nCe+US9V1bbQqvS2KUBTH85KY4amqRxDtciRNA9aqZvrKkXz7F/SKnjSkXbSKmjTuXTTorqbaFlATLRcM091CSSdN8DSdfagp6Bb7TFTL3toDh+K6osdSLqkMyN9jxgjTdmCpmuw2Wk1a9DdFESb7tVepFoEDPVqxrIAAAAldEVYdGRhdGU6Y3JlYXRlADIwMjItMTEtMzBUMTI6MzE6MjMrMDA6MDA7bQsKAAAAJXRFWHRkYXRlOm1vZGlmeQAyMDIyLTExLTMwVDEyOjMxOjIzKzAwOjAwSjCztgAAAABJRU5ErkJggg=="></td>
      <td align="center">5828235
(100.0%)</td>
      <td align="center">0
(0.0%)</td>
    </tr>
    <tr>
      <td align="center">4</td>
      <td align="left">ended_at
[POSIXct, POSIXt]</td>
      <td align="left" style="padding:8;vertical-align:middle"><table style="border-collapse:collapse;border:none;margin:0"><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">min : 2021-10-01 00:03:11</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">med : 2022-06-08 06:55:07</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">max : 2022-10-05 19:53:11</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">range : 1y 0m 4d 19H 50M 0S</td></tr></table></td>
      <td align="left" style="vertical-align:middle">4884765 distinct values</td>
      <td align="left" style="vertical-align:middle;padding:0;background-color:transparent;"><img style="border:none;background-color:transparent;padding:0;max-width:max-content;" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAHQAAABUBAMAAAChGA4iAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqbw8PD///+xh0SBAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5gseDCECoFTOKQAAAOpJREFUWMPt11sOhCAMBVC2MO7Ayw7o/vc25SERJjH0zkMn0vhj9KSBUArO3SseAwG07wbqw+/pgkgXrAQVpYCw1IdIS2oTRaTLltpEJVEJJNWcLNXnL6gOkaXVJZoqYYTGJdDStJw3ul+cr8MEjqgcUS+TnkebncNGfZh00p72nddAtaWsNBUD1TQslctS3aBpKufQ2ORIGr8xtPRVhpb/v0nRFYAW0yj1Xd58tBulTXc30baBmGg7VzRNh2eSlmMsSWXSS9Hae+y0Xt0omhNTNF/8QNJ0E7LTUjEMlbcoeCqTfpiCCHeveAKhVvqBiZM8+AAAACV0RVh0ZGF0ZTpjcmVhdGUAMjAyMi0xMS0zMFQxMjozMzowMiswMDowMNvK1/4AAAAldEVYdGRhdGU6bW9kaWZ5ADIwMjItMTEtMzBUMTI6MzM6MDIrMDA6MDCql29CAAAAAElFTkSuQmCC"></td>
      <td align="center">5828235
(100.0%)</td>
      <td align="center">0
(0.0%)</td>
    </tr>
    <tr>
      <td align="center">5</td>
      <td align="left">start_station_name
[character]</td>
      <td align="left" style="padding:8;vertical-align:middle"><table style="border-collapse:collapse;border:none;margin:0"><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">1. (Empty string)</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">2. Streeter Dr & Grand Ave</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">3. DuSable Lake Shore Dr & M</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">4. DuSable Lake Shore Dr & N</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">5. Michigan Ave & Oak St</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">6. Wells St & Concord Ln</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">7. Millennium Park</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">8. Clark St & Elm St</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">9. Kingsbury St & Kinzie St</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">10. Theater on the Lake</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">[ 1582 others ]</td></tr></table></td>
      <td align="left" style="padding:0;vertical-align:middle"><table style="border-collapse:collapse;border:none;margin:0"><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">895032</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">15.4%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">75985</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">1.3%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">42035</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.7%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">40592</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.7%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">40119</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.7%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">39352</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.7%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">36791</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.6%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">36784</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.6%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">35147</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.6%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">33533</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.6%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">4552865</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">78.1%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr></table></td>
      <td align="left" style="vertical-align:middle;padding:0;background-color:transparent;"><img style="border:none;background-color:transparent;padding:0;max-width:max-content;" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAGQAAADKBAMAAACh2vj5AAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqbw8PD///+xh0SBAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5gseDCEH0D46pgAAAIRJREFUaN7t2jEOQEAURVFbsATsgP3vTYHaPOQHOa92EsXcZjJd96+Nx/rz7WRats0I8mly4fA3fPkAafkhBEFUiSAFJBCqRJASEgiJIUgJCYQqEaSEBOIGGRAEaSaBUCWC5CQQqkSQEhIIiSFICQnEnaubYDs5njc0bEaQ95ILh/8vWwGB2qe/2cAY8AAAACV0RVh0ZGF0ZTpjcmVhdGUAMjAyMi0xMS0zMFQxMjozMzowNyswMDowMIny+FkAAAAldEVYdGRhdGU6bW9kaWZ5ADIwMjItMTEtMzBUMTI6MzM6MDcrMDA6MDD4r0DlAAAAAElFTkSuQmCC"></td>
      <td align="center">5828235
(100.0%)</td>
      <td align="center">0
(0.0%)</td>
    </tr>
    <tr>
      <td align="center">6</td>
      <td align="left">start_station_id
[character]</td>
      <td align="left" style="padding:8;vertical-align:middle"><table style="border-collapse:collapse;border:none;margin:0"><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">1. (Empty string)</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">2. 13022</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">3. 13300</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">4. LF-005</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">5. 13042</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">6. TA1308000050</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">7. 13008</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">8. TA1307000039</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">9. KA1503000043</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">10. TA1308000001</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">[ 1293 others ]</td></tr></table></td>
      <td align="left" style="padding:0;vertical-align:middle"><table style="border-collapse:collapse;border:none;margin:0"><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">895032</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">15.4%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">75985</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">1.3%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">42035</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.7%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">40592</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.7%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">40119</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.7%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">39352</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.7%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">36791</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.6%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">36784</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.6%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">35147</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.6%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">33533</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.6%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">4552865</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">78.1%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr></table></td>
      <td align="left" style="vertical-align:middle;padding:0;background-color:transparent;"><img style="border:none;background-color:transparent;padding:0;max-width:max-content;" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAGQAAADKBAMAAACh2vj5AAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqbw8PD///+xh0SBAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5gseDCEL2Yh2jQAAAIRJREFUaN7t2jEOQEAURVFbsATsgP3vTYHaPOQHOa92EsXcZjJd96+Nx/rz7WRats0I8mly4fA3fPkAafkhBEFUiSAFJBCqRJASEgiJIUgJCYQqEaSEBOIGGRAEaSaBUCWC5CQQqkSQEhIIiSFICQnEnaubYDs5njc0bEaQ95ILh/8vWwGB2qe/2cAY8AAAACV0RVh0ZGF0ZTpjcmVhdGUAMjAyMi0xMS0zMFQxMjozMzoxMSswMDowMCaIzf0AAAAldEVYdGRhdGU6bW9kaWZ5ADIwMjItMTEtMzBUMTI6MzM6MTErMDA6MDBX1XVBAAAAAElFTkSuQmCC"></td>
      <td align="center">5828235
(100.0%)</td>
      <td align="center">0
(0.0%)</td>
    </tr>
    <tr>
      <td align="center">7</td>
      <td align="left">end_station_name
[character]</td>
      <td align="left" style="padding:8;vertical-align:middle"><table style="border-collapse:collapse;border:none;margin:0"><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">1. (Empty string)</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">2. Streeter Dr & Grand Ave</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">3. DuSable Lake Shore Dr & N</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">4. Michigan Ave & Oak St</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">5. DuSable Lake Shore Dr & M</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">6. Wells St & Concord Ln</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">7. Millennium Park</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">8. Clark St & Elm St</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">9. Kingsbury St & Kinzie St</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">10. Theater on the Lake</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">[ 1600 others ]</td></tr></table></td>
      <td align="left" style="padding:0;vertical-align:middle"><table style="border-collapse:collapse;border:none;margin:0"><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">958227</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">16.4%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">76510</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">1.3%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">42621</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.7%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">40643</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.7%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">40633</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.7%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">39196</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.7%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">37113</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.6%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">36252</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.6%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">33592</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.6%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">33535</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.6%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">4489913</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">77.0%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr></table></td>
      <td align="left" style="vertical-align:middle;padding:0;background-color:transparent;"><img style="border:none;background-color:transparent;padding:0;max-width:max-content;" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAGMAAADKBAMAAABDBuOAAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqbw8PD///+xh0SBAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5gseDCEOqeKCAgAAAIhJREFUaN7t2rENgzAARFFGSDbAZITsvxuNk9YnE5Bx3tW8Bvk3lpdlrm2flWdrj0pe7zoEuTXpOPzND39CNgRBYpILVSLIsIl1kGH/GIKcTXIhMQRRJYL8J8nFEVIQBIlJLlSJIB0kF6pEkGtILiSGINeQXBy6usm3VvJ93tAeggxMOg7/LNsBI5GnG7C8QYgAAAAldEVYdGRhdGU6Y3JlYXRlADIwMjItMTEtMzBUMTI6MzM6MTQrMDA6MDB0sOJaAAAAJXRFWHRkYXRlOm1vZGlmeQAyMDIyLTExLTMwVDEyOjMzOjE0KzAwOjAwBe1a5gAAAABJRU5ErkJggg=="></td>
      <td align="center">5828235
(100.0%)</td>
      <td align="center">0
(0.0%)</td>
    </tr>
    <tr>
      <td align="center">8</td>
      <td align="left">end_station_id
[character]</td>
      <td align="left" style="padding:8;vertical-align:middle"><table style="border-collapse:collapse;border:none;margin:0"><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">1. (Empty string)</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">2. 13022</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">3. LF-005</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">4. 13042</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">5. 13300</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">6. TA1308000050</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">7. 13008</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">8. TA1307000039</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">9. KA1503000043</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">10. TA1308000001</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">[ 1300 others ]</td></tr></table></td>
      <td align="left" style="padding:0;vertical-align:middle"><table style="border-collapse:collapse;border:none;margin:0"><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">958227</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">16.4%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">76510</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">1.3%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">42621</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.7%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">40643</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.7%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">40633</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.7%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">39196</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.7%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">37113</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.6%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">36252</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.6%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">33592</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.6%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">33535</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">0.6%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">4489913</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">77.0%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr></table></td>
      <td align="left" style="vertical-align:middle;padding:0;background-color:transparent;"><img style="border:none;background-color:transparent;padding:0;max-width:max-content;" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAGMAAADKBAMAAABDBuOAAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqbw8PD///+xh0SBAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5gseDCESvePeTQAAAIhJREFUaN7t2rENgzAARFFGSDbAZITsvxuNk9YnE5Bx3tW8Bvk3lpdlrm2flWdrj0pe7zoEuTXpOPzND39CNgRBYpILVSLIsIl1kGH/GIKcTXIhMQRRJYL8J8nFEVIQBIlJLlSJIB0kF6pEkGtILiSGINeQXBy6usm3VvJ93tAeggxMOg7/LNsBI5GnG7C8QYgAAAAldEVYdGRhdGU6Y3JlYXRlADIwMjItMTEtMzBUMTI6MzM6MTgrMDA6MDCzEIguAAAAJXRFWHRkYXRlOm1vZGlmeQAyMDIyLTExLTMwVDEyOjMzOjE4KzAwOjAwwk0wkgAAAABJRU5ErkJggg=="></td>
      <td align="center">5828235
(100.0%)</td>
      <td align="center">0
(0.0%)</td>
    </tr>
    <tr>
      <td align="center">9</td>
      <td align="left">start_lat
[numeric]</td>
      <td align="left" style="padding:8;vertical-align:middle"><table style="border-collapse:collapse;border:none;margin:0"><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">Mean (sd) : 41.9 (0)</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">min &le; med &le; max:</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">41.6 &le; 41.9 &le; 45.6</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">IQR (CV) : 0 (0)</td></tr></table></td>
      <td align="left" style="vertical-align:middle">601238 distinct values</td>
      <td align="left" style="vertical-align:middle;padding:0;background-color:transparent;"><img style="border:none;background-color:transparent;padding:0;max-width:max-content;" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAHQAAABUBAMAAAChGA4iAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqbw8PD///+xh0SBAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5gseDCEn61AabgAAAEZJREFUWMPt0LEBACAIA0FWwA2MG8j+u2lnjZ3y31+RmNXKvUndUx0aAYVCoVAoFAqFQqFQKPQHKumWjpgP0j347qZ0VqsFERAkXhWqXBoAAAAldEVYdGRhdGU6Y3JlYXRlADIwMjItMTEtMzBUMTI6MzM6MzgrMDA6MDDxNY9TAAAAJXRFWHRkYXRlOm1vZGlmeQAyMDIyLTExLTMwVDEyOjMzOjM5KzAwOjAwJh88WwAAAABJRU5ErkJggg=="></td>
      <td align="center">5828235
(100.0%)</td>
      <td align="center">0
(0.0%)</td>
    </tr>
    <tr>
      <td align="center">10</td>
      <td align="left">start_lng
[numeric]</td>
      <td align="left" style="padding:8;vertical-align:middle"><table style="border-collapse:collapse;border:none;margin:0"><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">Mean (sd) : -87.6 (0)</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">min &le; med &le; max:</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">-87.8 &le; -87.6 &le; -73.8</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">IQR (CV) : 0 (0)</td></tr></table></td>
      <td align="left" style="vertical-align:middle">569536 distinct values</td>
      <td align="left" style="vertical-align:middle;padding:0;background-color:transparent;"><img style="border:none;background-color:transparent;padding:0;max-width:max-content;" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAHQAAABUBAMAAAChGA4iAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqbw8PD///+xh0SBAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5gseDCICi3md6gAAADVJREFUWMPtyzEBACAMA7BZwMJwgH9xPBgYb5M/VVm619ir+6iqqqqqqqqqqqqqqibV/lBZLinFGNSI0nuRAAAAJXRFWHRkYXRlOmNyZWF0ZQAyMDIyLTExLTMwVDEyOjM0OjAyKzAwOjAwORbMhwAAACV0RVh0ZGF0ZTptb2RpZnkAMjAyMi0xMS0zMFQxMjozNDowMiswMDowMEhLdDsAAAAASUVORK5CYII="></td>
      <td align="center">5828235
(100.0%)</td>
      <td align="center">0
(0.0%)</td>
    </tr>
    <tr>
      <td align="center">11</td>
      <td align="left">end_lat
[numeric]</td>
      <td align="left" style="padding:8;vertical-align:middle"><table style="border-collapse:collapse;border:none;margin:0"><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">Mean (sd) : 41.9 (0)</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">min &le; med &le; max:</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">41.4 &le; 41.9 &le; 42.4</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">IQR (CV) : 0 (0)</td></tr></table></td>
      <td align="left" style="vertical-align:middle">217893 distinct values</td>
      <td align="left" style="vertical-align:middle;padding:0;background-color:transparent;"><img style="border:none;background-color:transparent;padding:0;max-width:max-content;" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAHQAAABUBAMAAAChGA4iAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqbw8PD///+xh0SBAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5gseDCIWkaNJlwAAAG5JREFUWMPt1zEKgDAMRuFcQW9gvIG5/93Uojg0QYkolL5/6FK+pdDhifS1IdioOkV3d9QM+jVV1SydbYFCoVAoFAqFQqF/Uz+xHlG/HaAvaaneJN1BY3SLyCwtqk3qpXNErxc6j+rTHlQTk762AuzcP89ISUY3AAAAJXRFWHRkYXRlOmNyZWF0ZQAyMDIyLTExLTMwVDEyOjM0OjIwKzAwOjAw7Kza0wAAACV0RVh0ZGF0ZTptb2RpZnkAMjAyMi0xMS0zMFQxMjozNDoyMiswMDowMApuc0YAAAAASUVORK5CYII="></td>
      <td align="center">5822391
(99.9%)</td>
      <td align="center">5844
(0.1%)</td>
    </tr>
    <tr>
      <td align="center">12</td>
      <td align="left">end_lng
[numeric]</td>
      <td align="left" style="padding:8;vertical-align:middle"><table style="border-collapse:collapse;border:none;margin:0"><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">Mean (sd) : -87.6 (0)</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">min &le; med &le; max:</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">-89 &le; -87.6 &le; -87.3</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">IQR (CV) : 0 (0)</td></tr></table></td>
      <td align="left" style="vertical-align:middle">207375 distinct values</td>
      <td align="left" style="vertical-align:middle;padding:0;background-color:transparent;"><img style="border:none;background-color:transparent;padding:0;max-width:max-content;" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAHQAAABUBAMAAAChGA4iAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqbw8PD///+xh0SBAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5gseDCIoUMJUPAAAAFFJREFUWMPt1rERABAQRNFrgQ6sDui/NyMRCbiAMf6PNnn5mv1VWE0ac5fmAoVCoVDoCRrlpxUKhUKhUCj0MSq5aS7XqJKb1rO0f8sJlSP7qwbZ/CEjJWlBcAAAACV0RVh0ZGF0ZTpjcmVhdGUAMjAyMi0xMS0zMFQxMjozNDo0MCswMDowMCrD01QAAAAldEVYdGRhdGU6bW9kaWZ5ADIwMjItMTEtMzBUMTI6MzQ6NDArMDA6MDBbnmvoAAAAAElFTkSuQmCC"></td>
      <td align="center">5822391
(99.9%)</td>
      <td align="center">5844
(0.1%)</td>
    </tr>
    <tr>
      <td align="center">13</td>
      <td align="left">member_casual
[character]</td>
      <td align="left" style="padding:8;vertical-align:middle"><table style="border-collapse:collapse;border:none;margin:0"><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">1. casual</td></tr><tr style="background-color:transparent"><td style="padding:0;margin:0;border:0" align="left">2. member</td></tr></table></td>
      <td align="left" style="padding:0;vertical-align:middle"><table style="border-collapse:collapse;border:none;margin:0"><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">2401286</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">41.2%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr><tr style="background-color:transparent"><td style="padding:0 5px 0 7px;margin:0;border:0" align="right">3426949</td><td style="padding:0 2px 0 0;border:0;" align="left">(</td><td style="padding:0;border:0" align="right">58.8%</td><td style="padding:0 4px 0 2px;border:0" align="left">)</td></tr></table></td>
      <td align="left" style="vertical-align:middle;padding:0;background-color:transparent;"><img style="border:none;background-color:transparent;padding:0;max-width:max-content;" src="data:image/png;base64, iVBORw0KGgoAAAANSUhEUgAAAE4AAAAqBAMAAADv4XBiAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAAAD1BMVEX////9/v2mpqbw8PD///+xh0SBAAAAAnRSTlMAAHaTzTgAAAABYktHRACIBR1IAAAAB3RJTUUH5gseDCIuuaHxCQAAADpJREFUOMtjYBj8QAk7UBSEAag6ZWOsYFTdqDqs6ohNV4KEAInqlAgB/P6AA6NRdaPqkNQRm64GMwAASpS5RfrmtUQAAAAldEVYdGRhdGU6Y3JlYXRlADIwMjItMTEtMzBUMTI6MzQ6NDYrMDA6MDBJE+ZuAAAAJXRFWHRkYXRlOm1vZGlmeQAyMDIyLTExLTMwVDEyOjM0OjQ2KzAwOjAwOE5e0gAAAABJRU5ErkJggg=="></td>
      <td align="center">5828235
(100.0%)</td>
      <td align="center">0
(0.0%)</td>
    </tr>
  </tbody>
</table>
<p>Generated by <a href='https://github.com/dcomtois/summarytools'>summarytools</a> 1.0.1 (<a href='https://www.r-project.org/'>R</a> version 4.2.1)<br/>2022-11-30</p>
</div>

## Data Cleaning and Data Manipulation

#### Removing NA values

We can see from dfSummary table that there’s 5844 NA value in end\_lat
and end\_lng. We have to validate them first.

    sum(is.na(cyclist))

    ## [1] 11688

    colSums(is.na(cyclist))

    ##            ride_id      rideable_type         started_at           ended_at 
    ##                  0                  0                  0                  0 
    ## start_station_name   start_station_id   end_station_name     end_station_id 
    ##                  0                  0                  0                  0 
    ##          start_lat          start_lng            end_lat            end_lng 
    ##                  0                  0               5844               5844 
    ##      member_casual 
    ##                  0

It is true that there’s 5844 NA value in end\_lat and end\_lng. After
validating the data, we have to remove the NA value and check the NA
value again.

    #Remove the NA Value from end_lat and end_lng column
    cyclist <- na.omit(cyclist)
    #Check the NA Value again
    sum(is.na(cyclist))

    ## [1] 0

We have successfully removed the NA value.

#### Removing duplicate

After we remove the NA value, then we have to remove duplicate data from
dataset. First, we need to see dataset’s total rows. After that, we
remove the duplicate and check the total rows again.

    #See total row
    dim(cyclist)

    ## [1] 5822391      13

    #Remove duplicate
    cyclist %>% distinct()

    ##                   ride_id rideable_type          started_at            ended_at
    ##       1: 620BC6107255BF4C electric_bike 2021-10-22 12:46:42 2021-10-22 12:49:50
    ##       2: 4471C70731AB2E45 electric_bike 2021-10-21 09:12:37 2021-10-21 09:14:14
    ##       3: 26CA69D43D15EE14 electric_bike 2021-10-16 16:28:39 2021-10-16 16:36:26
    ##       4: 362947F0437E1514 electric_bike 2021-10-16 16:17:48 2021-10-16 16:19:03
    ##       5: BB731DE2F2EC51C5 electric_bike 2021-10-20 23:17:54 2021-10-20 23:26:10
    ##      ---                                                                       
    ## 5822387: 32ECA2B32C4B6F85  classic_bike 2022-09-05 17:59:21 2022-09-05 18:19:07
    ## 5822388: 14801F713026AEAE  classic_bike 2022-09-30 17:20:54 2022-09-30 17:34:40
    ## 5822389: 7CCAF5D6E88E45C0 electric_bike 2022-09-04 11:39:37 2022-09-04 11:50:55
    ## 5822390: AF9A129D9AFAA40B electric_bike 2022-09-28 13:42:45 2022-09-28 13:52:59
    ## 5822391: 60B56F4897429FCE electric_bike 2022-09-01 20:07:04 2022-09-01 20:18:01
    ##                start_station_name start_station_id
    ##       1: Kingsbury St & Kinzie St     KA1503000043
    ##       2:                                          
    ##       3:                                          
    ##       4:                                          
    ##       5:                                          
    ##      ---                                          
    ## 5822387:  Lincoln Ave & Winona St     KA1504000078
    ## 5822388:     Broadway & Ridge Ave            15578
    ## 5822389:     Broadway & Ridge Ave            15578
    ## 5822390:  Lincoln Ave & Winona St     KA1504000078
    ## 5822391:  Lincoln Ave & Winona St     KA1504000078
    ##                                             end_station_name end_station_id
    ##       1:                                                                   
    ##       2:                                                                   
    ##       3:                                                                   
    ##       4:                                                                   
    ##       5:                                                                   
    ##      ---                                                                   
    ## 5822387: Broadway & Wilson - Truman College Vaccination Site          13074
    ## 5822388: Broadway & Wilson - Truman College Vaccination Site          13074
    ## 5822389: Broadway & Wilson - Truman College Vaccination Site          13074
    ## 5822390: Broadway & Wilson - Truman College Vaccination Site          13074
    ## 5822391: Broadway & Wilson - Truman College Vaccination Site          13074
    ##          start_lat start_lng  end_lat   end_lng member_casual
    ##       1:  41.88919 -87.63850 41.89000 -87.63000        member
    ##       2:  41.93000 -87.70000 41.93000 -87.71000        member
    ##       3:  41.92000 -87.70000 41.94000 -87.72000        member
    ##       4:  41.92000 -87.69000 41.92000 -87.69000        member
    ##       5:  41.89000 -87.71000 41.89000 -87.69000        member
    ##      ---                                                     
    ## 5822387:  41.97491 -87.69250 41.96522 -87.65814        member
    ## 5822388:  41.98404 -87.66027 41.96522 -87.65814        member
    ## 5822389:  41.98411 -87.66027 41.96522 -87.65814        member
    ## 5822390:  41.97492 -87.69273 41.96522 -87.65814        member
    ## 5822391:  41.97490 -87.69265 41.96522 -87.65814        member

    #See total row after removing duplicate
    dim(cyclist)

    ## [1] 5822391      13

We see that there is no row removed, that means there is no duplicate
data in the dataset.

#### Creating date variable

We create the date variable from started at and ended at column. We also
create time of day column.

    #Create data variable
    cyclist <- cyclist %>% mutate(start_year = year(started_at),
                                  start_month = month(started_at),
                                  start_day = weekdays(started_at),
                                  start_hour = hour(started_at),
                                  start_time_of_day = case_when(start_hour>= 5 & start_hour <=12 ~ "Morning",
                                                          start_hour>=13 & start_hour <=17 ~ "Afternoon",
                                                          start_hour>=18 & start_hour <=22 ~ "Evening",
                                                          start_hour>= 0 & start_hour <=4 | start_hour ==23 ~ "Night"),
                                  end_year = year(ended_at),
                                  end_month = month(ended_at),
                                  end_day = weekdays(ended_at),
                                  end_hour = hour(ended_at),
                                  end_time_of_day = case_when(end_hour>= 5 & end_hour <=12 ~ "Morning",
                                                          end_hour>=13 & end_hour <=17 ~ "Afternoon",
                                                          end_hour>=18 & end_hour <=22 ~ "Evening",
                                                          end_hour>= 0 & end_hour <=4 | end_hour ==23 ~ "Night")
                                  )

#### Creating ride length variable

We create another variable to show how long a person use Cyclistic bike

    #Create ride length
    cyclist <- cyclist %>% mutate(ride_length_mins = difftime(ended_at, started_at, units="mins"))

Check if there are negative time values in the data

    #Check the negative time values count
    cyclist %>% filter(ride_length_mins<0) %>% count()

    ##      n
    ## 1: 108

We found 108 negative time values. So we have to subset negative time
values data and check them.

    negative_time <- cyclist %>% filter(ride_length_mins<0) %>% select(started_at, ended_at)
    head(negative_time, 10)

    ##              started_at            ended_at
    ##  1: 2021-11-07 01:40:02 2021-11-07 01:05:46
    ##  2: 2021-11-07 01:52:53 2021-11-07 01:05:22
    ##  3: 2021-11-07 01:40:13 2021-11-07 01:00:29
    ##  4: 2021-11-07 01:34:03 2021-11-07 01:17:13
    ##  5: 2021-11-07 01:54:25 2021-11-07 01:03:44
    ##  6: 2021-11-07 01:54:04 2021-11-07 01:25:57
    ##  7: 2021-11-07 01:51:52 2021-11-07 01:22:53
    ##  8: 2021-11-07 01:54:12 2021-11-07 01:05:09
    ##  9: 2021-11-07 01:54:36 2021-11-07 01:03:11
    ## 10: 2021-11-07 01:51:21 2021-11-07 01:07:59

Negative time values comes when a person end time bike use is less than
start time bike use (ended\_at&lt;started\_at), which suggest that this
negative time values data was input incorrectly (the start and end time
are swapped), so we have to create a new calculation:
difftime(started\_at, ended\_at)

    cyclist <- cyclist %>% mutate(ride_length_mins = ifelse(ended_at>started_at, 
                                                            difftime(ended_at, started_at, units='mins'), 
                                                            (difftime(started_at, ended_at, units='mins'))))

Check the count of negative time values again

    cyclist %>% filter(ride_length_mins<0) %>% count()

    ##    n
    ## 1: 0

All the negative time values are successfully converted to positive time
values

#### Removing the outliers

An outlier is an observation that lies an abnormal distance from other
values in a random sample from a population. In a sense, this definition
leaves it up to the analyst (or a consensus process) to decide what will
be considered abnormal (ITL NIST). We will use Interquartile Range (IQR)
method to remove the outliers. First, we need to create the upper and
lower bound.

    #Removing outliers in ride_length column  with IQR method
    Q1 <- quantile(cyclist$ride_length, .25)
    Q3 <- quantile(cyclist$ride_length, .75)
    IQR <- IQR(cyclist$ride_length)

    upper_bound <- Q3+1.5*IQR
    lower_bound <- Q1-1.5*IQR

Then, we remove the outliers. Any values that fall outside the lower and
upper bound are considered outliers.

    cyclist_clean <- cyclist %>% subset(ride_length_mins>lower_bound & ride_length_mins<upper_bound)

Now the dataset are clean, we are ready to visualize and analyze the
data. The clean cyclistic data will be exported as csv and will get a
data visualization on Tableau.

#### Create new dataframe to analyze top 5 start and end station

First, we need to create a data frame that consist the most used start
station coordinate, then we create a data frame that consist the most
used start station name for member and casual. The data frame will be
exported as csv and we will manually input the coordinate for the top 5
start station names. We found that there’s a blank station name from
dfSummary table, so **We also need to exclude the blank station name**.

    #Create coordinate data start station
    start_station_coord<-cyclist_clean %>% 
      filter(start_station_name != "") %>% 
      group_by(start_lat, start_lng, start_station_name, member_casual) %>% 
      summarize(count=n()) %>% 
      arrange(desc(count))

    #Create top 5 start station for member
    start_station_name_member<-cyclist_clean %>% 
      filter(start_station_name != "") %>% 
      group_by(start_station_name, member_casual) %>% 
      summarize(count=n()) %>% 
      arrange(desc(count)) %>% 
      filter(member_casual=="member")
    top_5_start_station_name_member<-start_station_name_member %>% head(5)

    #Create top 5 start station for casual
    start_station_name_casual<-cyclist_clean %>% 
      filter(start_station_name != "") %>% 
      group_by(start_station_name, member_casual) %>% 
      summarize(count=n()) %>% 
      arrange(desc(count)) %>% 
      filter(member_casual=="casual")
    top_5_start_station_name_casual<-start_station_name_casual %>% head(5)

We will do the same method for end station.

    #Create coordinate data end station
    end_station_coord<-cyclist_clean %>% 
      filter(end_station_name != "") %>% 
      group_by(end_lat, end_lng, end_station_name, member_casual) %>% 
      summarize(count=n()) %>% 
      arrange(desc(count))


    #Create top 5 end station for member
    end_station_name_member<-cyclist_clean %>% 
      filter(end_station_name != "") %>% 
      group_by(end_station_name, member_casual) %>% 
      summarize(count=n()) %>% 
      arrange(desc(count)) %>% 
      filter(member_casual=="member")
    top_5_end_station_name_member<-end_station_name_member %>% head(5)

    #Create top 5 end station for casual
    end_station_name_casual<-cyclist_clean %>% 
      filter(end_station_name != "") %>% 
      group_by(end_station_name, member_casual) %>% 
      summarize(count=n()) %>% 
      arrange(desc(count)) %>% 
      filter(member_casual=="casual")
    top_5_end_station_name_casual<-end_station_name_casual %>% head(5)

## Analyze

Tableau was used for visualize the data, and now its time to analyze! We
have to identify any pattern or trends from the data viz and find
insights from it. The insights will help us solve the business task.

Let us take a look at dashboard that already been created on Tableau.

<!DOCTYPE html>
<html>
    <head>
    </head>
    <body>
    <h4>Cyclistic Bike-Share Dashboard</h4>
    <div class='tableauPlaceholder' id='viz1666021462757' style='position: relative'><noscript><a href='#'><img alt='Dashboard 1 ' src='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Go&#47;GoogleDataAnalyticsCapstoneProjectCyclisticBike-Share&#47;Dashboard1_1&#47;1_rss.png' style='border: none' /></a></noscript><object class='tableauViz'  style='display:none;'><param name='host_url' value='https%3A%2F%2Fpublic.tableau.com%2F' /> <param name='embed_code_version' value='3' /> <param name='site_root' value='' /><param name='name' value='GoogleDataAnalyticsCapstoneProjectCyclisticBike-Share&#47;Dashboard1_1' /><param name='tabs' value='no' /><param name='toolbar' value='yes' /><param name='static_image' value='https:&#47;&#47;public.tableau.com&#47;static&#47;images&#47;Go&#47;GoogleDataAnalyticsCapstoneProjectCyclisticBike-Share&#47;Dashboard1_1&#47;1.png' /> <param name='animate_transition' value='yes' /><param name='display_static_image' value='yes' /><param name='display_spinner' value='yes' /><param name='display_overlay' value='yes' /><param name='display_count' value='yes' /><param name='language' value='en-US' /></object></div>                
    </body>
</html>

<br> We have **main dashboard**, which contains the daily and monthly
chart of the number of rides and average ride length between casual and
annual members. We also have a pie chart for comparing annual members
vs. casual riders and rideable type. The next dashboard is **Maps**,
containing the top 5 start and end stations’ names, including Google
street view. Last, we have **Final Report**, containing the business
task, key takeaways, and some business strategy to convert casual riders
to annual members.

#### **Key takeaways:**

-   Cyclistic has more annual members than casual riders (61.45% vs
    38.55%).

-   Casual riders, on average, use Cyclistic bikes longer than annual
    member.

-   Casual riders are more active on weekends than annual member, which
    suggest casual riders are more likely ride for leisure.

-   Annual members tend to use Cyclistic bikes on weekdays, which
    suggest annual riders are more likely ride to commute to work each
    day.

-   Annual members tend to use Cyclistic bikes in morning and afternoon,
    this indicates that annual members are more likely to use Cyclistic
    bikes to commute from home to office (morning) and from office to
    home (afternoon).

-   Busiest stations (top 5 stations) are in close proximity, suggesting
    that casual and annual members usually drive around the same place.

-   Casual riders mainly use Cyclistic bikes around June-August.

-   Annual members mainly use Cyclistic bikes around May-October.

-   Both casual and annual members Cyclistic bike-share usage drops
    significantly around January and February.

-   Docked bikes are rarely used, never even used by annual members.

-   Casual riders prefer electric bikes, while annual members use
    electric bikes and classic bikes almost evenly.

## Recommendations

Here are some recommendations that can be implemented to convert casual
riders to annual members:

-   Make regular membership discount to casual riders, especially from
    June to August.

-   Run promotions during the weekends to reach out more casual riders.

-   Provide and promote additional perks for having a Cyclistic
    membership account, such as holding a membership only events and
    prizes.

-   Run promotions at the top 5 stations that casual riders often use.

-   Create some informative promotions and banners to inform casual
    riders, such as how cost-effective for them to use Cyclistic bikes
    as an annual member for commute to work on weekdays.

-   Increase the bikes’ renting price for casual riders during the
    weekends, especially electic bikes.

<br>

##### The Data Viz can be accessed from [here.](https://public.tableau.com/app/profile/muhammad.hafidz.roihan/viz/GoogleDataAnalyticsCapstoneProjectCyclisticBike-Share/Dashboard1_1#1)

<br> -Muhammad Hafidz Roihan, 2022.
