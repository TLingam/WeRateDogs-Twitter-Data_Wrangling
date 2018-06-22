# WeRateDogs-Twitter-Data_Wrangling

## Overview

The task is to aggregate data from various sources:
1. given files
2. programmatically downloaded ones
3. data extracted via the Twitter API.

Some of the wording for the task was a little ambiguous, but I interpreted it as using the tweet_id's in the given file, extract additional information for them via the API, and then join it with the image data.

Then an open-ended analysis was to be performed.

## Process

### Gather
- The first data source, twitter_archive_enhanced.csv, was “on-hand” and available for direct download.
- The second piece, the image_predictions.tsv file, was hosted on a Udacity server, so the Request library was used to download it.
- The third piece had to be created. Tweet data was needed that could be found on Twitter itself, so this data was obtained using the Tweepy library to work with the Twitter API.
- This information was returned as JSON data, and stored in a text file.
- After gathering the data, each file was read into its own data frame and then assessed.

### Assess

- The twitter_archive information had a number of data type issues for the columns (e.g. - 'doggo', 'floofer', 'puppo', and 'pupper' should be categories, not strings), which were fairly straightforward, but the biggest hurdle was the rating system.
- The numerators spanned from 0 to 1776, and the denominator had values from 0 to 170. Upon closer investigation, several issues emerged.
- The ratings were obtained from the tweet text, but if another piece of the text contained a “#/#” format, such as “24/7” or “9/11”, it could get erroneously pulled into the rating.
- There were many ratings with denominators greater than 10. Specifically, they were multiples of 10.
- The column headers in the image_predictions data could be more descriptive and a couple of data type issues.

### Clean

- To fix the erronous ratings in the twitter_archive data, I extracted the rating from the text, using a more specific regular expression. That cleared up a number of formerly problematic ratings.
- For the tweets having ratings with denominators greater than 10, the text indicated that the picture had more than one dog, which lead me to believe that the rating was multiplied by the number of dogs in the picture. To fix this issue, I created a column to show the number of dogs, and normalized each rating to have a denominator of 10.
- I dropped the rows with missing ratings.
- Fixed the data type issues in both the dataframes.
- Changed the column headers in image_predictions dataframe to be more descriptive.
- I used the twitter_archive table as the base, and then joined the image_predictions and tweet_data tables with LEFT JOINs ON tweet_id.
- The resulting table was saved into a csv called ‘twitter_archive_master.csv’.
