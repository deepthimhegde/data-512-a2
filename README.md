# Bias in Wikipedia acricle data 

## Goal 
The goal of this project is to explore the concept of bias through data on Wikipedia articles - specifically, articles on political figures from a variety of countries.

## Datasets used
1. The Wikipedia politicians by country dataset can be found [here](https://figshare.com/articles/Untitled_Item/5513449)
2. The population dataset is drawn from [here](https://www.prb.org/international/indicator/population/table/)

## Dataset Description

## Input files
### The Wikipedia Articles Dataset -
| Column | Description |
|--------|-------------|
| page | The title of the page |
| country | The country of the politician |
| rev_id | The revision ID |

### Population Dataset -
| Column | Description |
|--------|-------------|
| Geography | Location (country or region) |
| Population mid-2018 (millions) | Population of the geographical region in millions |

## Output files 
### Article Quality Data - This file contains the results collected from [ORES](https://www.mediawiki.org/wiki/ORES) 
| Column | Description |
|--------|-------------|
| rev_id | The revision ID |
| article_quality | One of "FA", "GA", "B", "C", "Start" or "Stub" as obtained by the ORES API |

### Main Data for the analysis - 
| Column | Description |
|--------|-------------|
| page | The title of the page |
| country | The country of the politician |
| rev_id | The revision ID |
| article_quality | Quality of the article as determined by ORES |

### Final Data with all fields - 
| Column | Description |
|--------|-------------|
| country | Unique world country |
| all_articles_count | Count of all articles from page_data grouped by country |
| high_qual_articles_count | Count of high quality articles from page_data grouped by country |
| population | Number of people in the country as of mid-2018 |
| coverage | The ratio of the total number of articles to the population of the country |
| quality | The ratio of the number of high-quality articles to the total number of articles in the country |


### 

## API Calls
We use the [ORES](https://www.mediawiki.org/wiki/ORES) API to estimate the quality of an article. 

This API returns a probability value for each of these categories as well as the prediction:
 * **FA** - Featured article
 * **GA** - Good article
 * **B** - B-class article
 * **C** - C-class article
 * **Start** - Start-class article
 * **Stub** - Stub-class article
 
 Here's a sample response from the API call - 
 ```json
{
  "enwiki": {
    "models": {
      "wp10": {
        "version": "0.5.0"
      }
    },
    "scores": {
      "757539710": {
        "wp10": {
          "score": {
            "prediction": "Start",
            "probability": {
              "B": 0.0950995993086368,
              "C": 0.1709859524092081,
              "FA": 0.002534267983331672,
              "GA": 0.005731369423122624,
              "Start": 0.7091352495053856,
              "Stub": 0.01651356137031511
            }
          }
        }
      },
      "783381498": {
        "wp10": {
          "score": {
            "prediction": "Start",
            "probability": {
              "B": 0.020202281665235494,
              "C": 0.040498863202895134,
              "FA": 0.002648428776337411,
              "GA": 0.005101906528059532,
              "Start": 0.4793812253273645,
              "Stub": 0.452167294500108
            }
          }
        }
      }
    }
  }
}
```

## Reflections

What biases to expect to find in the data and why?
One thing to notice is that since English is a language spoken by less than a quarter of the world's population, 
the data source is biased in favor of the English speaking countries. Beyond that, the policies of the nation and governance structure could affect 
number of articles on politicians and this could be independent of the population. For example, multi-party systems have more number of political leaders, which could skew our 'coverage' values.
Population normalization, without accounting for these other factors my give us a wrong sense of coverage and quality of articles.

What might the results suggest about (English) Wikipedia as a data source?
Several countries have a quality measure of 0.0. On examining such cases a little more in depth, I found that these countries did not all have 0 English
Wikipedia articles. However, what was common among them was that they weren't English speaking countries. This validates what we expected to find.
For example, Monaco has a quality index of 0.0 and the percentage of English speakers in the country is merely 8.5% as opposed to Canada where it is 85%!

This dataset can still be useful for an application where our main focus is on the English language as opposed to the metrics for the countires. 

## Acknowledgement 
Several Verbatim has been borrowed from [A2: Bias in data](https://wiki.communitydata.science/Human_Centered_Data_Science_(Fall_2019)/Assignments#A2:_Bias_in_data) 


