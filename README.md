# Data-512-a2: Bias in Data

**Goal**
  Explore the concept of bias through data on Wikipedia articles - specifically, articles on political figures from a variety of countries

**Data sources:**
  * **Article data** from [politicians by country dataset](https://figshare.com/articles/dataset/Untitled_Item/5513449)
    * Attribute list:
      * page: article page title
      * country: sanitised country name
      * rev_id:revision id (edit id) of the last edit to the page
    * Cleann the data to exclude page names that start with "Template:". These are not real articles.
  * **World population data** from [WPDS_2020_data.csv](https://docs.google.com/spreadsheets/d/1CFJO2zna2No5KqNm9rPK5PCACoXKzb-nycJFhV689Iw/edit?usp=sharing). It is sourced from (world population data sheet)[https://www.prb.org/international/indicator/population/table/]
    * Attribute list:
      * FIPS: entity Code, either Country Code/Sub-Region Code/World
      * Name: entity name, either Country Name/Sub-Region Name/World
      * Type: Type of entity, either Country/Sub-Region/World
      * TimeFrame: Data collcted Year
      * Data (M): Population in millions
      * Population: Population number 
    * Data is listed such that all countries for a give Sub-Rejion are listed under that Sub-Rejion
  
  * **Article quality predictions**
    * Get the predicted quality scores for each article in the Wikipedia dataset using a machine learning system called ORES
    * The article quality estimates are, from best to worst:
        FA - Featured article
        GA - Good article
        B - B-class article
        C - C-class article
        Start - Start-class article
        Stub - Stub-class article
    * Using (ORES REST API)[https://ores.wikimedia.org/v3/#!/scoring/get_v3_scores_context_revid_model] get the prediction.
    * API parameters: revision Id (rev_id from page_data set) , model ("articlequality")
    * capture API return value - "prediction"
    * Send multiple rev_ids as batch to get response (based on expirement 50 rev_ids in one batch is best option)
* **Data process**
  * Merge the wikipedia data and population data together using country field
  * Exclude not matching records - either population dataset does not have an entry for the equivalent Wikipedia country, or vise versa. Output them to wp_wpds_countries-no_match.csv
  * Output final data set to wp_wpds_politicians_by_country.csv
  * Final data set schema:
      country
      article_name
      revision_id
      article_quality_est
      population
  * 
**Code:** 
  * hcds-a2-bias.ipynb script has complete code to analyze data from above 3 sources and generates final data set for analysis
  * Users can use data included in repo for analysis directly instead of processing from source
**Analysis:**
  * For analysis include sub-region
  * Find high quality articles using prediction value (high quality article if prediction is FA or GA)
  * Calculate percentage of articles-per-population using [total articles from a country]/[country population]
  * Ex: if a country has a population of 10,000 people, and you found 10 articles about politicians from that country, then the percentage of articles-per-population would be .1%.
  * Calculate percentage of high-quality articles-per-population using [high-quality articles from a country]/[country population]
Ex: if a country with 10,000 population has 10 articles about politicians, and 2 of them are FA or GA class articles, then the percentage of high-quality articles would be 20%

**Result:**
  * Resut tables are available in this project /result folder
  
**Reflection and Implications**
* Top & bottom percentage of articles-per-population are mostly from countries with very low & high population. So there is relation between population and percentage of articles-per-population.

* Oceania & Europe region countries has high percentage of articles-per-population

* North Amrica region countries, US (Rank 163) & Canada (Rank 69) has less percentage of articles-per-population eventhough they are well developed countries.

* We have many countries (~37) with out any high quality articles, thats why we see 0 percentage for Q4.

* We have some countries with out any articles, we excluded them from final analysis.

What biases did you expect to find in the data (before you started working with it), and why?
Depends on sample data collected year we may see bias towords countries which has peak wiki usage/contribution. In our class we saw the wiki contribution/usage pattern changes over the years.

**Questions**
  * **What biases did you expect to find in the data (before you started working with it), and why?**
    * Depends on sample data collected year we may see bias towords countries which has peak wiki usage/contribution. In our class we saw the wiki contribution/usage pattern changes over the years.
    * Expected United States, Canada, United Kingdom, Germany or Spain will have most articles and high quality articles compare to other countries. These countries are democratic, has very high education rate and access to internet. So expected more & good quality artciles.
    * Majority of articles might be from english speaking countries, so expevted bias in model results towords articles from english speaking countries.

  * **What (potential) sources of bias did you discover in the course of your data processing and analysis?**
    * We don't know politicians and population ratio. So if a country with small population and high percentage of politicians might have more articles coverage rate compare to other.

  * Using Q5 & Q6 results we can see high quality articles per population are from OCEANIA and Europe regions
  
  * **What might your results suggest about (English) Wikipedia as a data source?**
    * I think we don't see high quality articles for most of the countries, especially non english speaking countries due to this.

  * **What might your results suggest about the internet and global society in general?**
    * Surprisingly US & Canada is not in top 10 percentage of articles-per-population, so it shows there is no positive relation with overall internet and global society
