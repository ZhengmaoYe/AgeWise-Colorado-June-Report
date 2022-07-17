# AgeWise-Colorado-June 2022-Report
### Technology Used: Python
### All data comes from AgeWise Colorado and I get approval for posting this data from Bob Brocker the founder

#### AgeWise Colorado is a powerful new 501c3 non-profit that provides important information about aging in place in Colorado and connects visitors to the resources--the products and services--they need to help older Coloradans live their best lives where they choose to be. 

#### The ipynb file contains information about Data Cleaning and Data Analysis:
##### Package Needed: Pandas, Numpy, Fuzzywuzzy.

### Original Tables:
###### lls: It contains all basic information for Pages that has at least one pageview(Google Analytics) does not show Pages that have 0 views. Derived from All Pages in Google Analytics.
###### Device: Device that visitors view the Webpage with.
###### Source: Source that visitors click to enter Agewise Colorado websites.
###### Users: Users like Age and Gender.
###### City: Location where the visitors locate.

### Related codes and the problem it solved: (The capitalized parts are where you want to replace with your data)
#### 1) Convert similar texts to the same one.
#### Background: Visitors do not always type the same ketwords while looking for the same results. For example, in the csv file, people are looking for information      about "life bio", but some type "lifebio", which brings trouble to data/text analysis.
##### Ways of solveing it(code): 
      1. df['Column'].replace(['TEXT THAT YOU NEED TO CHANGE'],"TEXT YOU WANT TO REPLACE WITH")
         shortcoming: not good if you have too many to put into the square bracket
          
      2. matches=process.extract('TEXT YOU WANT TO REPLACE WITH', unique_types, limit = len(unique_types))
                  for i in matches:
                      if i[1] >= 80:
                          df.loc[df['RELATED COLUMN'] == i[0],'df']='TEXT YOU WANT TO REPLACE WITH'
         about: the code uses the fuzzywuzzy package. The matches will generate a rating score(0-100) about how each element 
                is similar to the text.
                Then we filter out elements that has a score of higher than 80 or other score based on need and replace those 
                with the text you want.
         shortcoming: Although you can use it to replace all elements under a certain rating score, you cannot guarantee if 
                all the elements you want to replace are replaced because you are not sure if the set-up rating score is 
                appropriate. Other than this, it is more recommended than .replace().
#### 2) Filter Text columns that contains certain strings:
##### Background: the Page column contains information about Blogs and Providers. As a Data Analyst, we need to filter out only 
      these Blog/Provider rows to further the analysis.
##### Way to solve it(code):
        df['Column'].str.contains(pat='STRING')
        
#### 3) Delete certain information in a text column:
##### Background: After export data from Google Analytics, column Page Title will contain "- AgeWise Colorado" in all elements. 
      It seems redundant so i want to remove it.
##### Way to solve it(code):
        DF['COLUMN'].str.replace('TEXT YOU WANT TO GET RID OF', '')
      about: '' means NULL, so the code means replace the text you dont want with nothing, which delete the texts. Instead of '', 
      you can use others to tell Python to replace with certain string.
      
#### 4) Perform calculation to columns by another column
##### Background: I was required to find the ranking of Pages hits and Avg Time on Page.
##### Way to solve it(code):
       DF.groupby('COLUMN 1')['COLUMN 2'].sum().sort_values(ascending=False)
       about: in the code, we sum COLUMN 2 based on Unique Column 1 values. Sum() can be replaced with count(), mean(), etc. 
       Then sort_values() can sort column 2 in certain order. COLUMN 1 and 2 can both include more than 1 column. If you decide 
       to have more for COLUMN 2, under sort_values(), you need to add by=one column from COLUMN 2.
