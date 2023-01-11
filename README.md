ps9591lab
================

## Introduction

This package contains two sample datasets for use in PS 9591 labs at
Western University (Winter 2023).

- 2019 Canadian Election Study, Online Survey
- Polity 5 dataset (year 2018 only)

## How to install

Run the code below in an `R` or `RStudio` session, and it will install
the package. You’ll need the `{remotes}` package, if you don’t already
have it (which is why it’s included in the code). If you already have
`{remotes}`, then just skip the first line.

    install.packages("remotes")
    library(remotes)
    remotes::install_github("jb-santos/ps9591lab")

## How to use

To use the package, first load it into your `R` session.

    library(ps9591lab)

To load the datasets into your `R` session, use the `data()` command, as
shown below.

    data(ces)
    data(polity)

## Code used to create datasets

For transparency and replicability, the code used to create the sample
datasets is below.

    # CES --------------------------------------------------------------------------

    ces <- rio::import("2019 Canadian Election Study - Online Survey v1.0.dta")

    myces <- data.frame(matrix(ncol=0, nrow=nrow(ces)))
    myces$weight <- ces$cps19_weight_general_all
    myces$dob <- ces$cps19_yob + 1919
    myces$age <- 2019 - myces$dob
    myces$agegrp <- car::recode(myces$age, 
                                '18:34="18-34"; 35:54="35-54"; 55:99="55+"', as.factor=TRUE)
    myces$age_18to34 <- car::recode(myces$age, '18:34=1; else=0')
    myces$age_35to54 <- car::recode(myces$age, '35:54=1; else=0')
    myces$age_55plus <- car::recode(myces$age, '55:99=1; else=0')
    myces$gender <- car::recode(ces$cps19_gender, '1="Man"; 2="Woman"; else=NA', as.factor=TRUE)
    myces$woman <- car::recode(ces$cps19_gender, '1=0; 2=1; else=NA')
    myces$lang <- car::recode(ces$pes19_lang, 
                              '68="English"; 69="French"; else="Other"', as.factor=TRUE)
    myces$province <- car::recode(ces$cps19_province, 
                                  '18="NL"; 23="PE"; 20="NS"; 17="NB";
                                  24="QC"; 22="ON"; 
                                  16="MB"; 25="SK"; 14="AB"; 15="BC"; else=NA', as.factor=TRUE)
    myces$region <- car::recode(myces$province, 
                                'c("NL","NS","PE","NB")="Atlantic";
                                "QC"="Quebec"; "ON"="Ontario"; 
                                c("BC","AB","SK","MB")="West"', as.factor=TRUE)
    myces$region <- factor(myces$region, levels = c("Ontario", "Quebec", "Atlantic", "West"))
    myces$reg_on <- car::recode(myces$region, '"Ontario"="1"; else="0"')
    myces$reg_qc <- car::recode(myces$region, '"Quebec"="1"; else="0"')
    myces$reg_atl <- car::recode(myces$region, '"Atlantic"="1"; else="0"')
    myces$reg_west <- car::recode(myces$region, '"West"="1"; else="0"')
    myces$educ <- car::recode(ces$cps19_education, 
                              '1:5="HS or less"; 6:8="Some uni"; 9="Bachelors"; 
                              10:11="Postgrad"; else=NA', as.factor=TRUE)
    myces$educ <- factor(myces$educ, 
                         levels = c("HS or less", "Some uni", "Bachelors", "Postgrad"))
    myces$relig <- car::recode(ces$cps19_religion, 
                               '1:2="None"; 3:7="Other"; 8:9="Christian"; 10="Catholic";
                               11:21="Christian"; 22="Other"; else=NA', as.factor=TRUE)
    myces$relig <- factor(myces$relig, level = c("Catholic", "Christian", "Other", "None"))
    myces$rel_catholic <- car::recode(myces$relig, '"Catholic"=1; else=0')
    myces$rel_christian <- car::recode(myces$relig, '"Christian"=1; else=0')
    myces$rel_other <- car::recode(myces$relig, '"Other"=1; else=0')
    myces$rel_none <- car::recode(myces$relig, '"None"=1; else=0')
    myces$income <- ces$cps19_income_number
    myces$incomegrp <- ces$cps19_income_cat

    myces$mostimp <- ces$cps19_imp_iss
    myces$natret <- car::recode(ces$cps19_econ_retro, 
                                '1="Better"; 3="Worse"; 2="Same"; else=NA', as.factor=TRUE)
    myces$demsat <- car::recode(ces$cps19_demsat, '1=4; 2=3; 3=2; 4=1; else=NA')
    myces$leftright <- ces$cps19_lr_scale_bef

    myces$feel_lib <- ces$cps19_party_rating_23
    myces$feel_cpc <- ces$cps19_party_rating_24
    myces$feel_ndp <- ces$cps19_party_rating_25
    myces$feel_bloc <- ces$cps19_party_rating_26
    myces$feel_green <- ces$cps19_party_rating_27
    myces$feel_ppc <- ces$cps19_party_rating_28

    myces$feel_trudeau <- ces$cps19_lead_rating_23
    myces$feel_scheer <- ces$cps19_lead_rating_24
    myces$feel_singh <- ces$cps19_lead_rating_25
    myces$feel_blanchet <- ces$cps19_lead_rating_26
    myces$feel_may <- ces$cps19_lead_rating_27
    myces$feel_bernier <- ces$cps19_lead_rating_28

    myces$pid <- car::recode(ces$cps19_fed_id,
                             '1="LPC"; 2="CPC"; 3="NDP"; 4="BQ"; 5="GPC"; 6="PPC"; 
                             7="Other"; 8="None"; 9=NA')
    myces$pid <- factor(myces$pid, 
                        levels = c("None", "Liberal", "Conservative", "NDP", 
                        "Bloc Quebecois", "Green", "PPC", "Other"))

    myces$vote_cps <- car::recode(ces$cps19_votechoice, 
                                  '1="Liberal"; 2="Conservative"; 3="NDP"; 
                                  4="Bloc Quebecois"; 5="Green"; 6="PPC"; 
                                  7="Other"; else=NA', as.factor=TRUE)
    myces$vote_cps <- factor(myces$vote_cps, 
                             levels = c("Liberal", "Conservative", "NDP", 
                             "Bloc Quebecois", "Green", "PPC", "Other"))
    myces$vote_pes <- car::recode(ces$pes19_votechoice2019, 
                                  '1="Liberal"; 2="Conservative"; 3="NDP";
                                  4="Bloc Quebecois"; 5="Green"; 6="PPC"; 
                                  7="Other"; else=NA', as.factor=TRUE)
    myces$vote_pes <- factor(myces$vote_pes, 
                             levels = c("Liberal", "Conservative", "NDP", 
                             "Bloc Quebecois", "Green", "PPC", "Other"))

    myces$polinterest <- ces$pes19_interest

    myces$changefptp <- car::recode(ces$cps19_pos_fptp, '6=NA')

    myces$reduceinequal <- car::recode(ces$pes19_govt_act_ineq, '1=5; 2=4; 3=3; 4=2; 5=1; 6=NA') # Rev
    myces$findjob       <- car::recode(ces$pes19_deserve1, '1=1; 2=2; 3=3; 4=4; 5=5; 6=NA')
    myces$lesswilling   <- car::recode(ces$pes19_deserve2, '1=1; 2=2; 3=3; 4=4; 5=5; 6=NA')
    myces$blameself     <- car::recode(ces$pes19_blame, '1=1; 2=2; 3=3; 4=4; 5=5; 6=NA')
    myces$marketlib <- ((myces$reduceinequal + myces$findjob +
                           myces$lesswilling + myces$blameself) - 4)

    myces$womenhome <- car::recode(ces$pes19_womenhome, '6=NA')
    myces$famvalues <- car::recode(ces$pes19_famvalues, '6=NA')
    myces$toofar <- car::recode(ces$pes19_equalrights, '6=NA')
    myces$lifestyles <- car::recode(ces$pes19_newerlife, '6=NA')
    myces$moraltrad <- ((myces$womenhome + myces$famvalues +
                           myces$toofar + myces$lifestyles) - 4)

    myces$sdo1 <- car::recode(ces$pes19_sdo1, '6=NA')
    myces$sdo2 <- car::recode(ces$pes19_sdo2, '1=5; 2=4; 3=3; 4=2; 5=1; 6=NA') # Reversed
    myces$sdo3 <- car::recode(ces$pes19_sdo3, '1=5; 2=4; 3=3; 4=2; 5=1; 6=NA') # Reversed
    myces$sdo4 <- car::recode(ces$pes19_sdo4, '6=NA')
    myces$sdo <- ((myces$sdo1 + myces$sdo2 + myces$sdo3 + myces$sdo4) - 4)

    myces$help_racial <- car::recode(ces$pes19_donerm, '1=5; 2=4; 3=3; 4=2; 5=1; 6=NA')
    myces$help_women <- car::recode(ces$pes19_donew, '1=5; 2=4; 3=3; 4=2; 5=1; 6=NA')
    myces$help_lgb <- car::recode(ces$pes19_donegl, '1=5; 2=4; 3=3; 4=2; 5=1; 6=NA')
    myces$helpgroups <- ((myces$help_racial + myces$help_women + myces$help_lgb) - 3)

    library(tidyverse)

    ces <- myces %>%
      select(-c(dob, reduceinequal:blameself, womenhome:lifestyles, sdo1:sdo4, help_racial:help_lgb))

    summary(ces)
    save(ces, file = "ces.Rda")


    # Polity  ----------------------------------------------------------------------

    polity <- rio::import("p5v2018.sav") %>%
      filter(year==2018)

    save(polity, file = "polity.Rda")
