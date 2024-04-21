# vsa_eventbrite_script


# Requires Current Exported data from Eventbrite
```{r}
vsa_data <- read_delim("apr20_vsa.csv")
```
# Required Columns
```
Name, Email, Pronouns, Indicate school, food allergies, accommodations
```
# Rename columns
```{r}
vsa_data <- vsa_data %>%
  rename(pronoun = `What are your pronouns?`,
         related = `Please indicate any school/organization that you are a part of. If you're a CS staff family member, please put staff name and your relation to the staff`,
         allergy = `Do you have any food/medical allergies? If no, please put N/A. If yes, please list them`,
         accommodation = `Does you need any accommodations regarding seating? (Ex. wheelchair, crutches, sensitivity to loud noises, etc) If not, please put N/A. If yes, please list them`) %>%
  select(`First Name`,`Last Name`, Email, pronoun, related, allergy, accommodation)

head(vsa_data, 5)
```
# Merges First and Last Name Columns
```{r}
vsa_data <- vsa_data %>%
  mutate(Name = paste(`First Name`, `Last Name`)) %>%
  select(Name, Email, pronoun, related, allergy, accommodation)
head(vsa_data, 5)
  
```
# Filter out "Info Requested"
```{r}
vsa_data <- vsa_data %>%
  filter(Name != "Info Requested Info Requested")
```
# Export the modified data frame to a CSV file
```{r}
write_csv(vsa_data, "modified_vsa_data.csv")
```
