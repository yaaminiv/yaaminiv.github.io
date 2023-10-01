---
layout: post
comments: true
title: Green Crab Experiment Part 18
tags: green-crab respirometry
---

## Analyzing respirometry data (finally)

After many iterations, I finally finished analyzing my respirometry data! I meant to post updates along the way but...oops. Here's just one big post of trials and tribulations I encountered as I followed an example script from fellow WHOI postdoc Chris Murray.

### Importing and formatting data

The respirometry data are all stored in various text files. Each file as data from one respirometry run (aka tank), with data for multiple probes (aka crab samples). I only ran three crabs per tank, but there are four probe ports, so one is always blank.

I imported the data for each day separately. First, I created a list of the six data files I needed for each day. Then, I manually went through and selected the columns I needed and renamed others:

```{r}
files20220708 <- fs::dir_ls("../../data/respiration/2022-07-08-resp/", glob = "*tank?.txt", recurse = TRUE) #Create a list of respirometry files from specified date. Use recurse = TRUE to prompt search through child directories. Only select files that end in tank[NUMBER].txt. ? = single character wildcard

dat20220708 <- read_delim(files20220708, id = "tank", delim = "\t", skip = 72, col_names = FALSE) %>%
  select(., c(1:8, 13, 18, 20:26, 31, 36, 38:44, 49, 54)) #Import data from all files in the designated list. Specify tab-delimited files and skip the first 72 lines. There are no column names. Select data from probes 1-3 (probe 4 was never used). Use all date/time columns since some probes ran for longer than others.
colnames(dat20220708) <- c("path",
                           "date_1", "time_1", "dt_s_1", "perc_air_sat_1", "dphi_1", "signal_int_mV_1", "amb_light_mV_1", "temp_C_1", "pressure_mbar_1",
                           "date_2", "time_2", "dt_s_2", "perc_air_sat_2", "dphi_2", "signal_int_mV_2", "amb_light_mV_2", "temp_C_2", "pressure_mbar_2",
                           "date_3", "time_3", "dt_s_3", "perc_air_sat_3", "dphi_3", "signal_int_mV_3", "amb_light_mV_3", "temp_C_3", "pressure_mbar_3") #Rename columns
```

For each file, I had information about the date, time, delta time, percent air saturation, delta phi, signal intensity, ambient light, temperature, and pressure! I needed some of this information, but not all. So my next step was to reformat these columns. My biggest need was to collapse the date, time, and delta time columns since this information was collected for each probe. However, since some probes were run for longer than others, or some probes started collecting data before others, I couldn't just consisently or programmatically keep the information for the probe that was run the longest (or atleast, I couldn't figure out a way to do this that made sense). I also needed to add salinity values I obtained using a refractometery manually.

```{r}
dat20220708mod <- dat20220708 %>%
  select(., -c(5:7, 14:16, 23:25)) %>%
  mutate(., date = case_when(is.na(date_1) == FALSE ~ date_1,
                             is.na(date_1) == TRUE ~ date_2)) %>%
  mutate(., date = case_when(is.na(date_1) == FALSE ~ date_1,
                             is.na(date_1) == TRUE ~ date_3)) %>%
  filter(., is.na(date_1) == FALSE) %>%
  mutate(., time = case_when(is.na(time_1) == FALSE ~ time_1,
                             is.na(time_1) == TRUE ~ time_2)) %>%
  mutate(., time = case_when(is.na(time_1) == FALSE ~ time_1,
                             is.na(time_1) == TRUE ~ time_3)) %>%
  mutate(., dt_s = case_when(is.na(dt_s_1) == FALSE ~ dt_s_1,
                             is.na(dt_s_1) == TRUE ~ dt_s_2)) %>%
  mutate(., dt_s = case_when(is.na(dt_s_1) == FALSE ~ dt_s_1,
                             is.na(dt_s_1) == TRUE ~ dt_s_3)) %>%
  select(., -c(date_1, date_2, date_3,
               time_1, time_2, time_3,
               dt_s_1, dt_s_2, dt_s_3)) %>%
  mutate(., datetime = dmy_hms(paste(date, time))) %>%
  mutate(., salinity = case_when(tank == "tank1" ~ 34,
                                 tank == "tank2" ~ 37,
                                 tank == "tank3" ~ 34,
                                 tank == "tank4" ~ 34,
                                 tank == "tank5" ~ 37,
                                 tank == "tank6" ~ 34)) %>%
  group_by(., treatment, tank) %>% arrange(., .by_group = TRUE) %>% ungroup(.) %>%
  pivot_longer(., cols = c(perc_air_sat_1, perc_air_sat_2, perc_air_sat_3), names_to = "og_perc", values_to = "perc_air_sat") %>%
  pivot_longer(., cols = c(temp_C_1, temp_C_2, temp_C_3), names_to = "og_temp", values_to = "temp_C") %>%
  pivot_longer(., cols = c(pressure_mbar_1, pressure_mbar_2, pressure_mbar_3), names_to = "og_pressure", values_to = "pressure_mbar") %>%
  mutate(., og_perc = gsub("perc_air_sat_", replacement = "", x = og_perc)) %>%
  mutate(., og_temp = gsub("temp_C_", replacement = "", x = og_temp)) %>%
  mutate(., og_pressure = gsub("pressure_mbar_", replacement = "", x = og_pressure)) %>%
  filter(., og_perc == og_temp) %>% filter(og_temp == og_pressure) %>%
  dplyr::rename(., probe_num = og_perc) %>%
  select(., -c(og_temp, og_pressure))
#Remove extraneous columns. Use case_when to combine date information across the different probes. Remove any rows without date. Repeat case_when series for time and delta time. Remove extraneous columns. Create a new datetime column for easy plotting. Add salinity information for all tanks based on lab notebook posts. Arrange by treatment and tank. Pivot all air saturation, temperature, and pressure columns, keeping the original column name as a new columm. Remove text before numbers and match based on probe numbers. Rename one column as the probe number and remove the other temperature and pressure columns.
```

Once I had all of the columns from the FireSting data formatted the way I wanted, I used the `respirometry` package to calculate dissolved oxygen/µmol/L based on percent air saturation, temperature, salinity, and atmospheric pressure:

```{r}
dat20220708mod <- dat20220708mod %>%
  mutate(., DO_umol_L = conv_o2(o2 = perc_air_sat, from = "percent_a.s.",
                                to = "umol_per_l",
                                temp = temp_C,
                                sal = salinity,
                                atm_pres = pressure_mbar)) #Create a new DO variable in units umol/L from %saturation using the function conv_o2() in the respirometry package
```

### Manually cleaning data

Now the fun part...manually cleaning data. I stuck to a few rules to clean data:

1. Although I recorded percent air saturation down to 75% before switching to measuring background respiration, I only wanted to calculate slopes from 100% to 80% air saturation.
2. We started recording before the probes were connected to chambers and placed in the appropriate temperature, so I needed to remove readings associated with these timepoints.
3. If probes were faultily connected to chambers in the process of setting up, I kept values with minimal noise that trended downward.

There was really no good way to do this besides inspecting the data manually for each tank and day. I plotted the data from each day just to get a feel for what I was working with:

```{r}
dat20220708mod %>%
  filter(., perc_air_sat <= 100) %>%
  group_by(tank, probe_num) %>%
  filter(., cumsum(perc_air_sat <= 80) == 0) %>%
  ungroup(.) %>%
  ggplot(., mapping = aes(x = (dt_s/60), y = perc_air_sat, color = treatment)) +
  geom_point(size = 0.5) +
  geom_smooth(method = lm, se = FALSE, lty = 1, color = "grey50")+
  facet_grid(cols = vars(treatment, tank), rows = vars(probe_num), scales = "free_x") +
  xlab("Time (min)") + ylab("Oxygen Air Saturation (%)") +
  scale_y_continuous(breaks = seq(80, 100, 10),
                     limits = c(80, 100)) +
  scale_color_manual(values = c(plotColors[2], plotColors[1], plotColors[3]),
                     name = "Temperature (ºC)",
                     breaks = c("13C", "30C", "5C"),
                     labels = c("13", "30", "5")) +
  theme_classic(base_size = 15) + theme(strip.text = element_blank(),
                                        axis.line = element_line()) #Filter all air saturation values above 100. Group by tank and probe. Create a conditional to remove all rows after perc_air_sat reaches 80 for the first time. perc_air_sat <= 80 is a logical vector, where TRUE = 0 and FALSE = 80. cumsum calculates the cumulative sum of the logical vector. Once perc_air_sat <= 80 is FALSE, a 1 gets recorded for the cumsum. This effectively removes all rows after 80 is hit for the first time. Modify x and y axis labels, y axis breaks. Facet such that each column is a treatment/tank, and each row is a probe. Use scales = free_x to have x-axes that vary across tanks. This provides two columns per treatment.
```

Then, I'd zoom into a tank:

```{r}
dat20220708mod %>%
  filter(., tank == "tank1") %>%
  filter(., perc_air_sat <= 100) %>%
  group_by(tank, probe_num) %>%
  filter(., cumsum(perc_air_sat <= 80) == 0) %>%
  ungroup(.) %>%
  ggplot(., mapping = aes(x = (dt_s), y = perc_air_sat, color = treatment)) +
  geom_point(size = 0.5) +
  geom_smooth(method = lm, se = FALSE, lty = 1, color = "grey50")+
  facet_grid(cols = vars(treatment, tank), rows = vars(probe_num), scales = "free_x") +
  xlab("Time (s)") + ylab("Oxygen Air Saturation (%)") +
  scale_y_continuous(breaks = seq(80, 100, 10),
                     limits = c(80, 100)) +
  scale_color_manual(values = c(plotColors[2], plotColors[1], plotColors[3]),
                     name = "Temperature (ºC)",
                     breaks = c("13C", "30C", "5C"),
                     labels = c("13", "30", "5")) +
  theme_classic(base_size = 15) + theme(strip.text = element_blank(),
                                        axis.line = element_line())
```

![Screenshot 2023-06-21 at 4 23 48 PM](https://github.com/yaaminiv/green-crab-metabolomics/assets/22335838/6e89e85e-b142-4fd6-b57c-af4d4bdde8a0)

**Figure 1**. Example of uncleaned data from Tank 1 on 7/8/2022

In the example above, I needed to remove values from the beginning of each recording that were associated with us setting up the chambers. The consistent downward slopes are from the crab respiring, while the spotty ups and downs are from us setting up and placing the chamber at the appropriate temperature.

To remove these measurements, I needed to identify the delta time associated with when the real data started, then filter it out from the dataset. I got this code after a lot of finagling and arguing with ChatGPT about it providing the exact *opposite* answer to waht I wanted without it realizing that it did that (especially for the `cumsum` code).

```{r}
dat20220708clean <- dat20220708mod %>%
  filter(., perc_air_sat <= 100) %>%
  group_by(tank, probe_num) %>%
  filter(., cumsum(perc_air_sat <= 80) == 0) %>%
  ungroup(.) %>%
  filter(., !(tank == "tank1" & probe_num == 1 & dt_s < 870)) %>%
  filter(., !(tank == "tank1" & probe_num == 2 & dt_s < 1125)) %>%
  filter(., !(tank == "tank1" & probe_num == 3 & dt_s < 1216)) %>%
  filter(., !(tank == "tank3" & probe_num == 3 & dt_s < 35)) %>%
  mutate(., dt_s = ifelse(tank == "tank1" & probe_num == 1, dt_s - 870, dt_s)) %>%
  mutate(., dt_s = ifelse(tank == "tank1" & probe_num == 2, dt_s - 1125, dt_s)) %>%
  mutate(., dt_s = ifelse(tank == "tank1" & probe_num == 3, dt_s - 1216, dt_s)) %>%
  mutate(., dt_s = ifelse(tank == "tank3" & probe_num == 3, dt_s - 35, dt_s)) %>%
  ggplot(., mapping = aes(x = (dt_s/60), y = perc_air_sat, color = treatment)) +
  geom_point(size = 0.5) +
  geom_smooth(method = lm, se = FALSE, lty = 1, color = "grey50")+
  facet_grid(cols = vars(treatment, tank), rows = vars(probe_num), scale = "free_x") +
  xlab("Time (min)") + ylab("Oxygen Air Saturation (%)") +
  scale_y_continuous(breaks = seq(80, 100, 10),
                     limits = c(80, 100)) +
  scale_color_manual(values = c(plotColors[2], plotColors[1], plotColors[3]),
                     name = "Temperature (ºC)",
                     breaks = c("13C", "30C", "5C"),
                     labels = c("13", "30", "5")) +
  theme_classic(base_size = 15) + theme(strip.text = element_blank(),
                                        axis.line = element_line()) #Filter all air saturation values above 100. Group by tank and probe. Create a conditional to remove all rows after perc_air_sat reaches 80 for the first time. perc_air_sat <= 80 is a logical vector, where TRUE = 0 and FALSE = 80. cumsum calculates the cumulative sum of the logical vector. Once perc_air_sat <= 80 is FALSE, a 1 gets recorded for the cumsum. This effectively removes all rows after 80 is hit for the first time. Check for values where tank == tank 1, probe_num == 1, and dt_s <= 870 Use the sub-conditions in parentheses and negate the condition with ! to retain all rows that don't meet this condition, but modify values for tank 1, probe 1. Do something similar such that tank 1, probe 2 starts at 1125 and tank 1, probe 3 starts at dt_s = 1216. Use an ifelse statement within mutate to change dt_s for tank 1 based on the actual dt_s start time. Modify x and y axis labels, y axis breaks. Facet such that each column is a treatment/tank, and each row is a probe. Use scales = free_x to have x-axes that vary across tanks. This provides two columns per treatment.
```

After filtering and plotting the values to double check, I saved my changes as a new dataframe:

```{r}
dat20220708data <- dat20220708mod %>%
  filter(., perc_air_sat <= 100) %>%
  group_by(tank, probe_num) %>%
  filter(., cumsum(perc_air_sat <= 80) == 0) %>%
  ungroup(.) %>%
  filter(., !(tank == "tank1" & probe_num == 1 & dt_s < 870)) %>%
  filter(., !(tank == "tank1" & probe_num == 2 & dt_s < 1125)) %>%
  filter(., !(tank == "tank1" & probe_num == 3 & dt_s < 1216)) %>%
  filter(., !(tank == "tank3" & probe_num == 3 & dt_s < 35)) %>%
  mutate(., dt_s = ifelse(tank == "tank1" & probe_num == 1, dt_s - 870, dt_s)) %>%
  mutate(., dt_s = ifelse(tank == "tank1" & probe_num == 2, dt_s - 1125, dt_s)) %>%
  mutate(., dt_s = ifelse(tank == "tank1" & probe_num == 3, dt_s - 1216, dt_s)) %>%
  mutate(., dt_s = ifelse(tank == "tank3" & probe_num == 3, dt_s - 35, dt_s)) #Save cleaned data as a new object
```

And I repeated this painstaking and iterative process for the remaining three days!

### Calculating slopes

Now for the important step: caculating slopes. So far, I have data that shows oxygen consumption by crabs at different temperatures. But, we want to compare rates of oxygen consumption between temperatures. To do that, I need to extract the regression slopes from my cleaned data. This is, arguably, the part of the code I understand the least.

First, I calculated the regression slopes. With my dataframe of cleaned data, I nested the data by treatment, tank, and probe number. This is important because it gives the following functions information on how to handle replicates or completely different samples. Obviously, I learned this the heard way because I initially only nested by treatment and tank, which gave me three separate values for each treatment since it collapsed the values with the same probe numbers.

```{r}
dat20220708_MO2_slopes <- dat20220708data %>%
  nest(data = -c(treatment, tank, probe_num)) %>%
  mutate(fit = map(data, ~lm(DO_umol_L ~ dt_s, data = .))) %>%
  mutate(tidied = map(fit, tidy)) %>%
  unnest(tidied) #Create a list object that stores the regression results for each chamber. Nest data by treatment, tank, and probe number so the regression slopes are calculated appropriately. Use map to extract linear model fit information for DO_umol_L by time. Extract slopes using broom::tidy and unnest.
```

After getting linear model fit information, I also got information from `glance`. I think the point of this is to adjusted R-squared information and some variables that aren't in the regression information above. But again, this is the part I'm least sure about.

```{r}
dat20220708_MO2_glance <- dat20220708data %>%
  nest(data = -c(treatment, tank, probe_num)) %>%
  mutate(fit = map(data, ~lm(DO_umol_L ~ dt_s, data = .))) %>%
  mutate(glanced = map(fit, glance)) %>%
  unnest(glanced) #Similar to above, but calculate the adjusted R-squared values by nesting treatment and probe information
```

Finally, I combined the information!

```{r}
dat20220708_MO2_slope_results <- dat20220708_MO2_slopes %>%
  filter(term == "dt_s") %>%
  mutate(slope_nmol_hr = estimate*1000*60) %>%
  dplyr::select(treatment, tank, probe_num, slope_nmol_hr, p.value) %>%
  mutate(r.squared = (dat20220708_MO2_glance$r.squared)) #Get the regression information for dt_s and not the intercept. Creates new a variable for respiration rate as nmol_per_hour (slope_nmol_hr). Select treatment, tank, probe number, slope over the hour, and p-value. Add the r-squared information from glance.
```

At this point, I decided that 1) the script was good enough to send to Julia so she could get some information for her SSF poster and 2) to create some initial plots. I sent a plot to Chris to see what he thought.

### Blank correction

Alright! It's roughly ~1ish months after I sent the script to Julia and I'm gearing to finish up this analysis so I can dive into my newly-obtained metabolomics dataset. Time to move onto correcting the slopes I obtained above for blanks.

After we ran each crab in the respirometry chamber, we removed the crab and ran the chamber sans crab but with the same water for at least 20 minutes. This will allow me to calculate a background respiration rate. On the crab's shell or in the water, there are bound to be microscopic organisms with respiration rates of their own. They're likely minimal, but it's a good sanity check to calculate a background respiration rate and subtract it from the rates we calculated above.

How did I get those slopes? By manually identifying times when the chamber was running with the blanks and pulling the last ten minutes of data! So back to the grind of manually checking each tank each day and identifying time codes. For the most part, I used the same time codes for each probe in the same tank, but there were some instances where maybe a chamber was removed early. I used the same `filter` code as I did when identifying the data to calculate crab respiration slopes:

```{r}
dat20220708blanks <- dat20220708mod %>%
  filter(., !(tank == "tank1" & dt_s < 3505)) %>%
  filter(., !(tank == "tank2" & dt_s < 4480)) %>%
  filter(., !(tank == "tank3" & probe_num == 1 & dt_s < 840)) %>%
  filter(., !(tank == "tank3" & probe_num == 2 & dt_s < 710)) %>%
  filter(., !(tank == "tank3" & probe_num == 2 & dt_s > 1310)) %>%
  filter(., !(tank == "tank3" & probe_num == 3 & dt_s < 840)) %>%
  filter(., !(tank == "tank4" & dt_s < 3105)) %>%
  filter(., !(tank == "tank5" & probe_num == 1 & dt_s < 7920)) %>%
  filter(., !(tank == "tank5" & probe_num == 2 & dt_s < 6585)) %>%
  filter(., !(tank == "tank5" & probe_num == 2 & dt_s > 7185)) %>%
  filter(., !(tank == "tank5" & probe_num == 3 & dt_s < 7920)) %>%
  filter(., !(tank == "tank6" & dt_s < 882)) %>%
  mutate(., dt_s = ifelse(tank == "tank1", dt_s - 3505, dt_s)) %>%
  mutate(., dt_s = ifelse(tank == "tank2", dt_s - 4480, dt_s)) %>%
  mutate(., dt_s = ifelse(tank == "tank3" & probe_num == 1, dt_s - 840, dt_s)) %>%
  mutate(., dt_s = ifelse(tank == "tank3" & probe_num == 2, dt_s - 710, dt_s)) %>%
  mutate(., dt_s = ifelse(tank == "tank3" & probe_num == 3, dt_s - 840, dt_s)) %>%
  mutate(., dt_s = ifelse(tank == "tank4", dt_s - 3105, dt_s)) %>%
  mutate(., dt_s = ifelse(tank == "tank5" & probe_num == 1, dt_s - 7920, dt_s)) %>%
  mutate(., dt_s = ifelse(tank == "tank5" & probe_num == 2, dt_s - 6585, dt_s)) %>%
  mutate(., dt_s = ifelse(tank == "tank5" & probe_num == 3, dt_s - 7920, dt_s)) %>%
  mutate(., dt_s = ifelse(tank == "tank6", dt_s - 882, dt_s))
```

Once I had the dataset, I used the same `fit` and `glance` code to get regression slope information:

```{r}
dat20220708_MO2_slopes_blanks <- dat20220708blanks %>%
  nest(data = -c(treatment, tank, probe_num)) %>%
  mutate(fit = map(data, ~lm(DO_umol_L ~ dt_s, data = .))) %>%
  mutate(tidied = map(fit, tidy)) %>%
  unnest(tidied) #Create a list object that stores the regression results for each chamber. Nest data by treatment, tank, and probe number so the regression slopes are calculated appropriately. Use map to extract linear model fit information for DO_umol_L by time. Extract slopes using broom::tidy and unnest.
```

```{r}
dat20220708_MO2_glance_blanks <- dat20220708blanks %>%
  nest(data = -c(treatment, tank, probe_num)) %>%
  mutate(fit = map(data, ~lm(DO_umol_L ~ dt_s, data = .))) %>%
  mutate(glanced = map(fit, glance)) %>%
  unnest(glanced) #Similar to above, but calculate the adjusted R-squared values by nesting treatment and probe information
```

```{r}
dat20220708_MO2_slopes_blanks_results <- dat20220708_MO2_slopes_blanks %>%
  filter(term == "dt_s") %>%
  mutate(slope_nmol_hr = estimate*1000*60) %>%
  dplyr::select(treatment, tank, probe_num, slope_nmol_hr, p.value) %>%
  mutate(r.squared = (dat20220708_MO2_glance_blanks$r.squared)) #Get the regression information for dt_s and not the intercept. Creates new a variable for respiration rate as nmol_per_hour (slope_nmol_hr). Select treatment, tank, probe number, slope over the hour, and p-value. Add the r-squared information from glance.
```

Then, I used `left_join` to combine the crab respiration and blank respiration slope information. In the same command, I subtracted the blank slope from the crab slope. I also calculated a blank/crab ratio.

```{r}
dat20220708_MO2_slope_results <- dat20220708_MO2_slope_results %>%
  left_join(., dat20220708_MO2_slopes_blanks_results, by = c("treatment", "tank", "probe_num"), suffix = c("", "_blank")) %>%
  mutate(., slope_nmol_hr_corrected = slope_nmol_hr - dat20220708_MO2_slopes_blanks_results$slope_nmol_hr) %>%
  mutate(., blank_ratio = abs(slope_nmol_hr_blank / slope_nmol_hr)) #Join dataframes by treatment, tank, and probe_num. Create a new column for the the corrected slope (uncorrected slope - blank slope) and the blank ratio (blank / corrected slope)
```

I saved data tables with the regression slope information from each day in [this folder](https://github.com/yaaminiv/green-crab-metabolomics/tree/main/output/04-respirometry-analysis). Chris' script suggests that blanks should be < 0.05. I definitely have readings where the blank ratios are > 0.05, so I emailed Chris to ask if ratios larger than a certain threshold could be considered outliers and the associated slopes removed from the dataset.

### Statistical testing and plotting

Alright, I have my FINAL data! I two rounds of statistical testing and plotting: treatment effects at each time point, and treatment x time effects. I'll only talk about the treatment x time effects in this post because they're the ones that matter!

First, I combined all of the slope result information from each day into one large dataframe. I used the `bind` command which allows me to add a "day" identifier column. When entering days, I had to use characters, so I used "04", "08", "15", and "22" to ensure my days would be considered alphanumerically when I eventually created a plot:

```{r}
all_MO2_slope_results <- bind_rows("04" = dat20220708_MO2_slope_results,
                                   "08" = dat20220712_MO2_slope_results,
                                   "15" = dat20220719_MO2_slope_results,
                                   "22" = dat20220726_MO2_slope_results,
                                   .id = "day") #Create a new dataframe with all uncorrected slope results, including a day
```

Then, I imported demographic information for each crab I did respirometry with. This information was in the [TTR spreadsheet](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/data/time-to-right.csv). I imported the data, then manipulated the columns I wanted so they had similar formatting to the slope results table:

```{r}
demTTRdata <- read.csv("../../data/time-to-right.csv", header = TRUE) %>%
  dplyr::select(., -c(sampling.ID, notes)) %>%
  filter(., is.na(probe.number) == FALSE) %>%
  dplyr::rename(., probe_num = probe.number) %>%
  mutate(., day = case_when(date == "7/8/2022" ~ "04",
                            date == "7/12/2022" ~ "08",
                            date == "7/15/2022" ~ "11",
                            date == "7/19/2022" ~ "15",
                            date == "7/22/2022" ~ "18",
                            date == "7/26/2022" ~ "22")) %>%
  filter(., day != "11" & day!= "18") %>%
  mutate(., treatment = case_when(treatment.tank == "1" | treatment.tank == "4" ~ "13C",
                                  treatment.tank == "2" | treatment.tank == "5" ~ "5C",
                                  treatment.tank == "3" | treatment.tank == "6" ~ "30C")) %>%
  dplyr::rename(., tank = treatment.tank) %>%
  mutate(., size = carapace.width/carapace.length) %>%
  mutate(., missing.legs = case_when(number.legs < 10 ~ "Y",
                                     number.legs == 10 ~ "N")) %>%
  select(., c(day, treatment, tank, probe_num, sex, integument.color, weight, size, missing.legs)) #Import TTR data and modify to include only the desired demographic data and average TTR data for crabs used during respiration trials
```

Initially, I thought I would include TTR data in this dataframe, but then I decided against it since I figured they'd probably be correlated in some way. I used `left_join` to combine the two dataframes by day, treatment, tank, and probe number, using a sneaky `case_when` so tank identification would be consistent:

```{r}
all_MO2_slope_results_demTTRdata <- all_MO2_slope_results %>%
  mutate(tank = case_when(tank == "tank1" ~ 1,
                          tank == "tank2" ~ 2,
                          tank == "tank3" ~ 3,
                          tank == "tank4" ~ 4,
                          tank == "tank5" ~ 5,
                          tank == "tank6" ~ 6,
                          tank == "Tank_1" ~ 1,
                          tank == "Tank_2" ~ 2,
                          tank == "Tank_3" ~ 3,
                          tank == "Tank_4" ~ 4,
                          tank == "Tank_5" ~ 5,
                          tank == "Tank_6" ~ 6)) %>%
  mutate(probe_num = as.numeric(probe_num)) %>%
  left_join(., demTTRdata, by = c("day", "treatment", "tank", "probe_num")) #Use case_when to include only the tank # in the tank column. Convert probe_num to a numeric column and left_join with demographic and TTR data using day, treatment, tank, and probe_num
```

Before running the final model, I checked the oxygen consumption data for normality. As expected, it was a distribution with a significant right tail. I tried both log and square root transformations. They looked equally the same, so I settled on a log transformation. And of course, by "before running the final model," I mean I ran the model without incorporating the transformation correctly, got bamboozled by the model results, and then realized I didn't do my data transformation.

I initially tried a model with the log of -1 * slope (this way positive value = more respiration by the crab) as the response variable and treatment x day, sex, integument color, size, weight, and whether or not the crab was missing legs as explanatory variables. Then, I used `step` to perform stepwise backwards deletion and identify the most parsimonious model with the lowest AIC:


```{r}
lmAllSlopeResults <- aov(log(-slope_nmol_hr_corrected) ~ treatment*as.numeric(day) + sex + weight + size + integument.color + missing.legs, data = all_MO2_slope_results_demTTRdata) #Examine the influence of treatment, day, and various demographic factors on respiration rate
lmAllSlopeResultsStep <- step(lmAllSlopeResults, direction = "backward") #Use backwards-deletion approach to identify the most significant model (lowest AIC)
```

The most parsimonious model included temperature and day, but not the interaction between the two:

```{r}
lmAllSlopeResultsSig <- aov(log(-slope_nmol_hr_corrected) ~ treatment + as.numeric(day), data = all_MO2_slope_results_demTTRdata) #Run the most significant model identified by step
summary(lmAllSlopeResultsSig) #Significant impact of treatment and day (not not day x treatment)
```

>               
 Df Sum Sq Mean Sq F value   Pr(>F)    
treatment        2  43.72  21.862   60.27 8.73e-16 ***
as.numeric(day)  1   5.09   5.091   14.03 0.000372 ***
Residuals       68  24.67   0.363                     
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

After checking that the residuals were normally distributed and displayed heteroskedasticity, I used a Tukey HSD posthoc test to examine pairwise differences in temperature treatment. I found that there was a significant difference between 5ºC and the other two temperatures, but only a marginally significant difference between 30ºC and 13ºC. Using pairwise t-tests, I found a marginally significant difference between days 4 and 22. I saved the output from all statistical tests using `broom::tidy` [in this folder](https://github.com/yaaminiv/green-crab-metabolomics/blob/main/output/04-respirometry-analysis/).

Now for the fun stuff: plotting! I used `facet_wrap` with a dataframe of all slope results to easily create a multipanel plot. The dataframe was similar to the one I ran the ANOVA on, but I renamed the "5C" treatment as "05C" so it would be recognized first alphanumerically. This let me display boxplots in increasing temperature order so I could add a dashed line representing a TPC connecting the boxplot medians for each treatment at each timepoint. I used colors and shapes consistent with my TTR plot:

```{r}
all_MO2_slope_results_reorder <- all_MO2_slope_results %>%
  mutate(., treatment = gsub(x = treatment, pattern = "5C", replacement = "05C"))
#Reorder temperature treatments for plotting by renaming 5C as 05C so it is the first alphanumerically
tail(all_MO2_slope_results_reorder)
```

```{r}
facet_labels <- c("04" = "Day 4",
                  "08" = "Day 8",
                  "15" = "Day 15",
                  "22" = "Day 22") #Assign labels for facets
```

```{r}
all_MO2_slope_results_reorder %>%
  ggplot(., aes(x = treatment, y = -slope_nmol_hr_corrected, color = treatment)) +
  geom_boxplot() + geom_point(aes(shape = treatment), size = 2) +
  xlab("") + ylab("Oxygen Consumption (nmol/hr)") +
  stat_summary(fun = median,
               geom = "line",
               aes(group = 1),
               col = "black", lty = 2) +
  facet_wrap(. ~ day, ncol = 4, labeller = labeller (day = facet_labels)) +
  scale_x_discrete(breaks = c("05C", "13C", "30C"),
                   labels = c("", "", "")) +
  scale_color_manual(values = c(plotColors[3], plotColors[2], plotColors[1]),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "13C", "30C"),
                     labels = c("5", "13", "30")) +
  scale_shape_manual(values = c(15, 19, 17),
                     name = "Temperature (ºC)",
                     breaks = c("05C", "13C", "30C"),
                     labels = c("5", "13", "30")) +
  theme_classic(base_size = 15) + theme(axis.title = element_blank(),
                                        axis.ticks.x = element_blank(),
                                        strip.text.x = element_text(size = 15),
                                        strip.background = element_rect(colour = "white"))
ggsave("figures/multipanel-oxygen-consumption-corrected.pdf", height = 8.5, width = 11)
#Create a boxplot for oxygen consumption (µmol/hr). Use -slope_nmol_hr so the larger the slope, the faster the oxygen consumption over the hour. Add a line connecting each boxplot midpoint. Facet wrap and use the labeller to relabel facets.
```

<img width="1134" alt="Screenshot 2023-09-30 at 10 31 38 PM" src="https://github.com/yaaminiv/green-crab-metabolomics/assets/22335838/e22f900d-7324-4d96-967c-09cd30ef32c7">

**Figure 2**. Oxygen consumption (nmol/hr) for crabs at 5ºC, 13ºC, and 30ºC at days 4, 8, 15, and 22 of the experiment.

Aside from the temperature and time trends, I noticed that the crabs at 5ºC have less variability in their respiration rates than crabs at 13ºC and 30ºC. This could signify that at 5ºC, all the crabs are slowing down. Since they're all exhibiting similar acclimation responses, their respiration values are more similar. At 13ºC and 30ºC, there may be several metabolic pathways that the crabs can use to maintain homeostasis. It will be really interesting to pair this data with the metabolomic data to see what was happening! I think some of that variability could be due to not using the same crab individuals (aka nuclear or mitochondrial genotype variation), so I'm glad we're able to track individual variation in this summer's experiment!

### Going forward

1. Determine if there's a way to remove respirometry outliers
3. Update methods
4. Update results

{% if page.comments %}

<div id="disqus_thread"></div>
<script>

/**
*  RECOMMENDED CONFIGURATION VARIABLES: EDIT AND UNCOMMENT THE SECTION BELOW TO INSERT DYNAMIC VALUES FROM YOUR PLATFORM OR CMS.
*  LEARN WHY DEFINING THESE VARIABLES IS IMPORTANT: https://disqus.com/admin/universalcode/#configuration-variables*/
/*
var disqus_config = function () {
this.page.url = PAGE_URL;  // Replace PAGE_URL with your page's canonical URL variable
this.page.identifier = PAGE_IDENTIFIER; // Replace PAGE_IDENTIFIER with your page's unique identifier variable
};
*/
(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');
s.src = 'https://the-responsible-grad-student.disqus.com/embed.js';
s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript">comments powered by Disqus.</a></noscript>

{% endif %}

<script id="dsq-count-scr" src="//the-responsible-grad-student.disqus.com/count.js" async></script>
