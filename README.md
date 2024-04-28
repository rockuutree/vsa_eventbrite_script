# R script for VSA Eventbrite Data 
```
Filters out unnecessary data and returns csv of valid responses
```

## HOW TO USE FOR REGULAR PEOPLE
```
1. USE eventbrite.Rmd
2. Download an IDE - R Studio https://posit.co/download/rstudio-desktop/
3. Export CSV from Eventbrite (Under "Responses to custom questions")
4. Replace INPUT with csv file name to read_delim(<"INPUT.csv">)
5. Run R Script
6. Returns modified_vsa_data.csv
```
## Alternative way to run on terminal/Command Line
```
1. Install homebrew
2. Install pandoc: brew install pandoc
3. cd to directory (in terminal) containing eventbrite.Rmd and cur_vsa.csv
4. run: Rscript -e "rmarkdown::render('eventbrite.Rmd')"
```

### Requires Current Exported data from Eventbrite
```{r}
read_delim("apr20_vsa.csv")
```
### Required Columns
```
First Name,

Last Name,

Email,

What are your pronouns?,

Please indicate any school/organization that you are a part of. If you're a CS staff family member, please put staff name and your relation to the staff,

Do you have any food/medical allergies? If no, please put N/A. If yes, please list them,

Does you need any accommodations regarding seating? (Ex. wheelchair, crutches, sensitivity to loud noises, etc) If not, please put N/A. If yes, please list them
```
### Rename columns
```{r}
vsa_data <- vsa_data %>%
  rename(pronoun = `What are your pronouns?`,
         related = `Please indicate any school/organization that you are a part of. If you're a CS staff family member, please put staff name and your relation to the staff`,
         allergy = `Do you have any food/medical allergies? If no, please put N/A. If yes, please list them`,
         accommodation = `Does you need any accommodations regarding seating? (Ex. wheelchair, crutches, sensitivity to loud noises, etc) If not, please put N/A. If yes, please list them`) %>%
  select(`First Name`,`Last Name`, Email, pronoun, related, allergy, accommodation)

head(vsa_data, 5)
```
### Merges First and Last Name Columns
```{r}
vsa_data <- vsa_data %>%
  mutate(Name = paste(`First Name`, `Last Name`)) %>%
  select(Name, Email, pronoun, related, allergy, accommodation)
head(vsa_data, 5)
  
```
### Filter out "Info Requested"
```{r}
vsa_data <- vsa_data %>%
  filter(Name != "Info Requested Info Requested")
```
### Export the modified data frame to a CSV file
```{r}
write_csv(vsa_data, "modified_vsa_data.csv")
```
