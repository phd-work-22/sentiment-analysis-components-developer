# Title: Analysing the developer's sentiment in software components: a decade-long study of an Apache project

Here are the steps of conducting of our work based on the paper titled above.

1. Data extracting
   1.1 Extracting Mailing list of Apache HTTP Server Development
   
   The mailing list retrieved from the archives of the mailing list used for discussions about the actual development of the server. The archive is shared in the following link:       https://marc.info/?l=apache-httpd-dev. The retrieved archives of the mailing list were kept in files. Source code: _scrapp_marc_mail-revised.py_. Output of the code is a list 
   of *.csv files containing Apache's email yearly.
   Emails collected: 47,104 emails from January 2015 to May 2024

   1.2. Extracting Apache commits data
   
   The commits data were retrieved from the repository in the following link: https://github.com/apache/httpd.git. The retrieved archives of the commits were kept in files. Source       code: _ApacheCommitsLoading.py_
   Commits collected: 162,289 commits from January 2015 to May 2024

   All of the files were converted into tables for further analysis.
   
3. Data preprocessing
   During preprocessing, we focused on cleaning up message body content. We removed lines starting with ‘>’, URLs, names, signatures, and greetings. Additionally, lines containing    code syntax or HTML/XML tags were eliminated.

   Given that some developers in the mailing list used multiple email addresses, we standardised these to a single address per developer for consistency. Similarly, we applied a       standardisation method to developers' names. When an email address lacked an associated name, we derived the name from the email account.

4. Sentiment classification

   To classify messages from the mailing list to sentiments classes, negative and positive, we employed the Sentistrength-SE tool, designed specifically for Software Engineering       and utilised in previous studies. This tool provides positive and negative sentiment scores ranging from +1 to +5 and -1 to -5, respectively.

   Email messages typically contain multiple sentences, each with its own sentiment score.  We divided all email messages into individual sentences (source codes:          
   _NormalisedApacheMListsReport.java_; output file: _mlist_apache-all_nomrald_FINAL.txt_). Sentence-ending punctuation (e.g., periods, question marks, exclamation points) served as
   indicators to delineate sentence boundaries~\cite{richter2010words}. We, then, gave the scores both positive and negative sentences with SentiStrength-SE (output file:
   _sentiment_labelling_apache-FINAL.txt_).
   
5. Lingking the datasets
   We linked the commits and sentiment datasets based on timestamps (year, month, day) and developer’s name, particularly focusing on the sentiment dataset’s strong positive and       negative scores. As a developer may have different names because of the inconsistency of the format used in both datasets, and she/he may have different emails, we checked each
   of them. Then, we manually unified the names and the emails in both datasets before linking them.
   
   Any neccessary linkage between two datasets (mailing list and commits) for further analysis were done in SQL and R. All visualization in the study was done in R.
   
6. Measuring complexity
   For complexity measurement, we utilised cyclomatic complexity provided by the PyDriller library.

   Our approach involved three stratified steps: firstly calculating yearly mean complexity values for each file due to potential yearly fluctuations; secondly 
   determining yearly mean complexity values aggregated across all files within each component; thirdly calculating mean complexity values across years for each 
   component. This final step was crucial for addressing RQ2.
   
7.  Constructing the components network
    In our component network analysis, we undertook several steps to construct the network. Initially, we gathered all files from the Apache ‘httpd’ project up to May 2024. These 
    files were analysed using a static code analysis tool to ascertain dependencies, utilising ‘calls’ and ‘called by’ methods (Source code: _streamline_network.sh_). Subsequently, 
    we mapped  these file dependencies as node pairs with weights and applied the Infomap tool to generate the component network. The 
    resulting network comprised  35 distinct components, with our analysis focusing on the 20 largest components due to space constraints.

8.  Manipulating tables to answer Research Questions
    RQ1 - Comparisons between Apache developers.
    
    Figure 1. Boxplots of the top 10 of active developers, DWNs, DWPs regarding a relative number of commits done (a) and of positive and negative sentences written (b). (Source 
    code:boxblots-negpos-4plots.R)

    Figure 2. The Infomap dependency network is created with the Infomap Network Navigator (https://www.mapequation.org/navigator/). The file of ftree file used is _A-B_w-
    httpd.free_.

    Figure 3. Heatmaps of the number of differences between positive and negative messages and the number of complexity in twenty components yearly (Source code: _analysing
    complexity dependency.R_). 

All datasets are available by request.

    
