# AllTrails Project
 Final Project for DS 2515 - Intermediate Programming with Data
 
# Data Preprocessing Utilities

This project provides utility functions for preprocessing data for further analysis. Specifically, it includes functions for converting categorical variables into dummy/indicator variables and for converting string representations of dictionaries into actual Python dictionaries.

## Overview

The `add_dummy_variables` function takes a DataFrame and a column name as input. The specified column should contain lists of categorical variables. The function will create new columns in the DataFrame for each unique category found across all lists, and it will populate these columns with binary data indicating the presence or absence of the category in each list.

The `convert_dict_like_string` function is designed to handle columns in the DataFrame that contain string representations of dictionaries. It converts these strings into actual Python dictionaries.

These preprocessing steps are crucial for many types of data analysis and machine learning tasks, as they transform the data into a format that can be more easily understood by various algorithms.

## Getting Started

### Prerequisites

This project is designed to run in a Python environment. You will need:

- Python (version X.X or later)

Additionally, the following Python libraries are required:

- pandas
- ast
- copy
- math

### Installation

1. Clone this repository to your local machine.
2. Install the prerequisites.

## Usage

<a id='data'></a>
### *Data Description*

We will be utilizing a Kaggle dataset that contains information about trails in National Parks. 

From this dataset, we will be able to obtain characteristics about each trail including: 
- features
- activities
- ratings
- popularity
- location (latitude and longitude)
- etc

For the purposes of this project, we will focus mostly on the following characteristics: 
- features (broken out as dummy/Boolean variables)
    - wildlife
    - dogs
    - kids
    - views
- popularity
- length
- visitor usage
- elevation gain
- difficulty
- latitude 
- longitude

Then we can utilize this information to recommend trails to users based on their preferences and values. The location of the trails can be utilized for Haversine distances to recommend trails that are close to our user as well as just general matches.

<a id='pipeline'></a>
### *Pipeline*
One of the initial things we will do is delete columns from the data frame that don’t have to do with our analysis. For example, the “units” column does not give us any useful data for our prediction model, so we can delete this column. 

Currently, our data frame has columns for features and activities. In each observation in these columns is a list of the features or activities that trail possesses. While the values in each row appear to be lists, they are in fact lists that look like strings, so we need to clean these up and make them into lists that are iterable so we can work with them more easily. Once we create true lists, we need to make the dataset more usable, so we will have to simplify the features and activities columns so that each individual feature and individual activity has a dummy column in our dataframe. We will do this using one-hot encoding to create dummy columns for each feature or activity, with values 0 and 1 representing whether the trail possesses that feature or activity. 

We will accomplish this task with the following functions:
- 'clean_list'
- 'clean_str'
- 'add_dummy_variables'
- 'get_variable_list'

*note: we also added a dummy variable 'dogs-yes' that works as the inverse of 'dogs-no' to make some of our later implementation work more elegantly*

Additionally, we plan to work with the coordinates given by the '_geoloc' column in the original dataset. However, much like the lists in the features and activities columns, while the values in _geoloc look like dictionaries, they are in fact strings that look like dictionaries. Additionally, it would be easier to work with each trail's coordinates if we had separate columns for latitude and longitude. Thus, we will go through the process of converting these strings to true dictionaries, and, once we have them as dictionaries, splitting up their key/value pairs so that latitude and longitude each have their own columns. We will also convert them to radians instead of degrees for use with Haversine Distances. 

We will accomplish this task using the following functions:
- 'convert_dict_like_strings'
- 'value_lists'
- some code implementation of the two functions

Another factor going into data processing is making sure we understand all of the data. Because there was no data dictionary, we had to refer back to the user interface on the All Trails website to determine the units of measurement that the dataset used. 

We determined that the columns “elevation gain” as well as “length” were measured with meters. For example, if we look at the data for the Harding Ice Field Trail, we can see that the recorded elevation gain is 1,116.897 and the recorded length is 15,610.59. On the All Trails website, the length is 9.2 miles, which is equivalent to 14,806 meters, and the elevation gain is 3,461 feet, which is equivalent to 1,109.77 meters. These measurements are similar to those found in our dataset. The slight differences could be accounted for by recent changes in trail length and elevation due to erosion, since the data was collected three years ago and the All Trails website is updated frequently. 

Before beginning our analysis, we removed all rows with missing elements from our data. Before running our clustering, we scale normalized *all* variables that we were clustering on. While there is some debate on whether you should scale normalize dummy variables, when we did not include these variables in the scale normalization process, the weighted aspect of the dataframe made it so that the dummy variables were almost always being "outweighed" by the continuous features and were not being considered heavily in the clustering process. We chose to normalize these in this context just to give these features a chance to influence our clustering when appropriate. 


Try it out!
