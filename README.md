
install.packages("curl")

library("curl")

install.packages("httr")

library("httr")

install.packages("rvest")

library("rvest")

# Call the get_wiki_covid19_page function and print the response
covid19_url <- "https://en.wikipedia.org/w/index.php?title=Template:COVID-19_testing_by_country"
response <- GET(covid19_url)
response

# Get the root html node from the http response in task 1 
covid19_root_node <- read_html( "https://en.wikipedia.org/w/index.php?title=Template:COVID-19_testing_by_country") 
covid19_root_node

# Get the table node from the root html node
covid19_table_node <- html_node(covid19_root_node, "table")
covid19_table_node

# Read the table node and convert it into a data frame, and print the data frame for review
covid19_data_frame <- html_table(covid19_table_node)
head(covid19_data_frame)

# Print the summary of the data frame
summary(covid19_data_frame)

# call `preprocess_covid_data_frame` function and assign it to a new data frame
wiki_covid19_data_frame <- preprocess_covid_data_frame(covid19_data_frame)
wiki_covid19_data_frame

# Print the summary of the processed data frame again
summary(wiki_covid19_data_frame)

# Export the data frame to a csv file
write.csv(wiki_covid19_data_frame, file = "covid.csv", row.names = FALSE)

# Get working directory
wd <- getwd()
# Get exported 
file_path <- paste(wd, sep="", "/covid.csv")
# File path
print(file_path)
file.exists(file_path)

## Download a sample csv file
#covid_csv_file <- download.file("https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-RP0101EN-Coursera/v2/dataset/covid.csv", destfile="covid.csv")
#covid_data_frame_csv <- read.csv("covid.csv", header=TRUE, sep=",")

# Read covid_data_frame_csv from the csv file
covid_data_frame_csv <- read.csv("covid.csv", header=TRUE, sep=",")

# Get the 5th to 10th rows, with two "country" "confirmed" columns
covid_data_frame_csv[ 5:10, c( "country", "confirmed") ]

# Get the total confirmed cases worldwide
confirmed_cases <- covid_data_frame_csv[ , 4]
confirmed_cases
total_confirmed_cases <- sum(confirmed_cases)
total_confirmed_cases

# Get the total tested cases worldwide
tested_cases <- covid_data_frame_csv[ , 3]
tested_cases
total_tested_cases <- sum(tested_cases)
total_tested_cases

# Get the positive ratio (confirmed / tested)
positive_ratio <- total_confirmed_cases/total_tested_cases
positive_ratio

# Get the `country` column
country_column <- covid_data_frame_csv[ , 1]
country_column

# Check its class ( should be Factor)
class(country_column)

# Conver the country column into character so that you can easily sort them
as.character ( country_column)

# Sort the countries AtoZ
sort(country_column)

# Sort the countries ZtoA
 Country_ZtoA <- sort(country_column, decreasing = TRUE)
 Country_ZtoA

# Print the sorted ZtoA list
print( Country_ZtoA)

# Use a regular expression `United.+` to find matches
matches <- regexpr("United.+", covid_data_frame_csv[ ,"country"])
countires_start_with_United<- regmatches(covid_data_frame_csv[ ,"country"], matches)
countires_start_with_United

# Print the matched country names
print(countires_start_with_United)

# Select a subset (should be only one row) of data frame based on a selected country name and columns
wiki_covid19_data_frame[1, c( "country", "confirmed", "confirmed.population.ratio") ]

# Select a subset (should be only one row) of data frame based on a selected country name and columns
wiki_covid19_data_frame[ 20, c("country", "confirmed", "confirmed.population.ratio") ]

if (49621 > 1491) {
print( "Afghanistan has larger ratio of confirmed cases to population")
} else {
print( "Bhutan has larger ratio of confirmed cases to population")
}

# Get a subset of any countries with `confirmed.population.ratio` less than the threshold
threshold = "lessRisk"
if (threshold == "lessRisk"){
subset(wiki_covid19_data_frame, confirmed.population.ratio < .01)
} else {
subset(wiki_covid19_data_frame, confirmed.population.ratio > .01)    
}
