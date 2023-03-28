Actor Matcher
==============================

Evaluate chemistry between couples in romantic k-dramas and recommend the best match for an actor/actress

Contributors: 
- Chen Xingyu @XYChen21
- Jacintha Wee @jacinthawee
- Valencia Lie @valencialie11  

Project Organization
------------

    ├── LICENSE
    ├── README.md 
    ├── scraper              <- scraping scripts         
    ├── data
    │   ├── scraped_raw      <- Raw data scraped from MyDramaList.com.
    │   ├── external         <- Third party sources (Actor images).
    │   ├── interim          <- Intermediate data that has been transformed.
    │   └── processed        <- Final data used for analysis in notebooks.
    │
    ├── preprocessing   
    │   ├── basic_cleaning   <- basic cleaning on scraped results
    │   └── coref_resolution <- resolve pronouns in reviews
    │   
    ├── notebooks            <- Analysis done in Jupyter notebooks. Naming convention is 
    │                           a number (for ordering), the creator's initials, and description.
    │
    ├── results              <- Generated analysis results as csv
    │
    ├── visualization        <- R scripts for dashboard visualization
    │
    ├── requirements_coref.txt <- requirements file for running /preprocessing/coref_resolution
    │
    └── requirements.txt     <- requirements file for the rest of analysis 


--------

## Pipeline explanation
### 1. Scraper
`/scraper/spiders`
- Crawled data including drama information, actor/actress information and reviews from https://mydramalist.com using `scrapy`


1.1. `kdrama_raw.py`: spider for scraping the url of all dramas

1.2. `review_raw.py`: spider for scraping kdrama reviews

1.3. `role_raw.py`: spider for scraping general information

### 2. Preprocessing
`/preprocessing/basic_cleaning`  
- Basic cleaning for scraped data  

2.1. `drama_url_cleaning.ipynb`: basic cleaning of the csv scraped from kdrama_raw.py  

2.2. `filter_drama_cleaning.ipynb`: count number of reviews for each drama and drop the ones with not enough reviews  

2.3. `roles_cleaning.ipynb`: basic cleaning of the csv scraped from role_raw.py  

`/preprocessing/coref_resolution`  
- resolve pronouns in reviews using `neuralcoref`

2.4. `coref-resolve.ipynb`: drop non-English reviews, convert emojis/emoticons, replace pronouns __(this is an essential part which extract sentences with keywords and resolve pronouns to find couples mentioned for analysis later. To run this, set up environment according to `requirements_coref.txt` in root directory)__

### 3. Notebooks
`/notebooks`  

3.1. `1-jac-EDA.ipynb`: Jupyter notebook containing EDA on general information and reviews  

3.2. `2-xy-sentiment.ipynb`: extract lines with keywords from reviews, find pairs from each line using __word dependencies__ from `Spacy` dependency matcher and do sentiment analysis for each pair  

3.3. `3-val-facial_sim.ipynb`: do facial similarity analysis using __VGGFace embeddings__ on all images of actors that we scraped before. Match these actors and actresses with the `/data/processed/senti.csv` produced by `2-xy-sentiment.ipynb` accordingly (e.g. actor A looks the most similar to actor B and actor B is matched with actress C). 

3.4. `4-val-match_recomm.ipynb`: find who are the baseline actors and actresses (those who do not need facial similarity at all and can just be matched naturally using sentiment analysis) as well as the ‘newcomers’ and clean the csv so that it is fit for our dashboard.

### Visualisation 
`/visualization`  

- Visualization of the matches generated (data stored in `results` folder)

4.1. global.R: all the csv we have generated will be imported in this file so that our ui and server file can always access it.

4.2. ui.R: the design of the dashboard as well as its structure

4.3. server.R: the interactive aspect of the dashboard so that it is able to function the way we want to.

To access the dashboard, please go to: https://valencialie11.shinyapps.io/actormatcher/


<p><small>Project based on the <a target="_blank" href="https://drivendata.github.io/cookiecutter-data-science/">cookiecutter data science project template</a>. #cookiecutterdatascience</small></p>
