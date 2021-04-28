# UN Sustainable Development Goals Text Classification
Source: https://zindi.africa/competitions/sustainable-development-goals-sdgs-text-classification-challenge

The United Nations created 17 measurable indicators for development called the Sustainable Development Goals or SCG, they consists of: 

1. No Poverty
2. Zero Hunger
3. Good Health and Well Being
4. Quality Education
5. Gender Equality
6. Clean Water and Sanitation
7. Affordable and Clean Energy
8. Decent Work and Economic Growth
9. Industry, Innovation, and Infrastructure
10. Reduced Inequalities
11. Sustainable Cities and Communities
12. Responsible Consumption and Production
13. Climate Action
14. Life Below Water
15. Life on Land
16. Peace, Justice and String Institutions
17. Partnerships


Each SDG has several “targets”, or social outcomes that the UN hopes to achieve by 2030. Each of these targets is measured using a set of indicators. These indicators represent the quantitative measurements that will be used to judge whether each SDG target has been achieved or not by 2030.

I will be classifying text content by its relevance to the measurable indicators of the United Nations’ Sustainable Development Goal 3: Health and Well-Being or being refer as SDG 3. SDG 3 has 14 targets and 27 indicators.

### The SDG 3 Indicators

Theere are 27 possible SDG 3 indicators, in the dataset each indicator will be refer using code, e.g. "3.1.1":

- 3.1.1 - Maternal mortality ratio
- 3.1.2 - Proportion of births attended by skilled health personnel
- 3.2.1 - Under-5 mortality rate
- 3.2.2 - Neonatal mortality rate
- 3.3.1 - Number of new HIV infections per 1 000 uninfected population, by sex, age and key populations
- 3.3.2 - Tuberculosis incidence per 100 000 population
- 3.3.3 - Malaria incidence per 1 000 population
- 3.3.4 - Hepatitis B incidence per 100 000 population
- 3.3.5 - Number of people requiring interventions against neglected tropical diseases
- 3.4.1 - Mortality rate attributed to cardiovascular disease, cancer, diabetes or chronic respiratory disease
- 3.4.2 - Suicide mortality rate
- 3.5.1 - Coverage of treatment interventions (pharmacological, psychosocial and rehabilitation and aftercare services) for substance use disorders
- 3.5.2 - Harmful use of alcohol, defined according to the national context as alcohol per capita consumption (aged 15 years and older) within a calendar year in litres of pure alcohol
- 3.6.1 - Death rate due to road traffic injuries
- 3.7.1 - Proportion of women of reproductive age (aged 15–49 years) who have their need for family planning satisfied with modern methods
- 3.7.2 - Adolescent birth rate (aged 10–14 years; aged 15–19 years) per 1 000 women in that age group
- 3.8.1 - Coverage of essential health services (defined as the average coverage of essential services based on tracer interventions that include reproductive, maternal, newborn and child health, infectious diseases, non-communicable diseases and service capacity and access, among the general and the most disadvantaged population)
- 3.8.2 - Proportion of population with large household expenditures on health as a share of total household expenditure or income
- 3.9.1 - Mortality rate attributed to household and ambient air pollution
- 3.9.2 - Mortality rate attributed to unsafe water, unsafe sanitation and lack of hygiene (exposure to unsafe Water, Sanitation and Hygiene for All (WASH) services)
- 3.9.3 - Mortality rate attributed to unintentional poisoning
- 3.a.1 - Age-standardized prevalence of current tobacco use among persons aged 15 years and older
- 3.b.1 - Proportion of the target population covered by all vaccines included in their national programme
- 3.b.2 - Total net official development assistance to medical research and basic health sector
- 3.b.3 - Proportion of health facilities that have a core set of relevant essential medicines available and affordable on a sustainable basis
- 3.c.1 - Health worker density and distribution
- 3.d.1 - International Health Regulations (IHR) capacity and health emergency preparedness

# The Problem

Many government bodies, donors and developers sttrugle to identify which tenders, programs or news articles are related to which of the 27 SDG 3 indicators and if they are worth investment or partnership. 

The aim for this project is to create a classifier that labels text content by the 27 SDG 3 indicators that are most “relevant.” 

The model that classifies text content by relevant SDG indicators would be used to help algorithmically identify development projects, organizations, and documents that relate to the same SDG targets and indicators.

This would be useful for governments, development donors, and development implementers seeking to align resources, find partners, or perform outcomes-focused research.

# The Data
The training data includes 2,995 web-scraped text from tenders, programs, and documents, as well as news articles about international development and humanitarian aid, and finally text descriptions of organizations involved in those sectors.

This dataset includes the "outcome" which in this competition is the "label(s)," i.e. which of the 27 indicators are relevant to the given text. This dataset includes the columns: 
- ID: Unique ID of text to be classified
- Type: The type or source of the text.
- Text
- Label_1 through Label_12

From exploring the different type of text we see that:
- Grant: Description of the grant, what it is and how much they are giving
- Organization: Description of the organization, what they are doing
- Contract: Title of the contract
- Tender: Description of the tender, who they are, what are they doing, which government or organizational body
- Funding Info: Description of the fund, what they fund can do, who they are giving it to
- Open Opportunity: Description of different opprotunity available, what they do, who they are
- News: The news headline
- Profram: Description of the program, what they do, where they are, who are they

Label_1 through Label_12: These columns are populated starting at Label_1 increasing incrementally until all relevent classifications are populated, to a maximum of 12 Labels. The remaining Labels are left blank.

The testing data includes 998 web-scrapped texthe columns ID, Type, and Text.
 
# Text Preprocessing and Feature Engineering
From looking at the text I normalized the text by:
- Removing html markingd
- Rmoving &nbsp
- Removing non letters
- Lowering casing the text
- Removing stopwords

I decided that lemmatizing and keeping the root of the word works best for this text data set

# The Model
For the models, I started with ones that are able to do multi label classifications which were KNN, decision tree, mlp and random forest. Then wanted to experiment with model that can take the multi output classifier function which were logistic and ridge classifier to allow the mode to do multilabel classification.

I evaluated the model with Hamming Loss score which is, the fraction of labels that are incorrectly predicted, i.e., the fraction of the wrong labels to the total number of labels.

It reports how many times on average, the relevance of an example to a class label is incorrectly predicted. Therefore, hamming loss takes into account the prediction error (an incorrect label is predicted) and missing error (a relevant label not predicted), normalized over total number of classes and total number of examples.

We would expect the hamming loss to be 0, which would imply no error. This means practically the smaller the value of hamming loss, the better the performance of the learning algorithm.

Each model Hamming Score were:
- KNN: 0.053
- Logistic Regression: 0.052
- Decision Tree: 0.066
- MLP Classifier: 0.049
- Random Forest Classifier: 0.056
- Ridge Classifier CV: 0.0474

# Recommendation
When testing the model with the actual testing data provided the Ride Classifier CV with the Multi Output Classficier function performed the best with TF-IDF Vectorizer and it's deafult parameters. 

It's hamming score were 0.0447 meaning it predicted 96% of the predicted indicaors correctly.

This mean that the best model to use for text classification to clasify which text description for which tenders or organization and its relevance to which indicators is the Ridge Classifier CV with the Multi Output Classficier function.
