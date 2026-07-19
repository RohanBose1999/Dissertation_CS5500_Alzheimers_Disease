<h1>Alzheimer's Disease Prediction Using Machine Learning and Deep Learning</h1>

<p>This project built a complete, reproducible software workflow that predicts Alzheimer's disease from ordinary tabular patient data, the kind of structured information that is already collected in routine care. Instead of relying on expensive brain scans or invasive tests, I wanted to see how far careful data preparation and well-chosen models could go using data that is cheap and widely available. The short answer: a gradient boosting model called CatBoost reached <b>82.8% accuracy</b> with strong and stable performance under cross-validation.</p>

<br>

<h2>The Problem I Set Out To Solve</h2>

<p>Alzheimer's disease is the most common cause of dementia, and it is difficult to detect early. Progression is slow, and the tests that confirm it, such as neuroimaging and cerebrospinal fluid biomarkers, are costly and often invasive. With an ageing population, there is a real need for tools that are affordable, accessible, and can flag risk early using data that clinicians already have.</p>
<p>My project explores a practical alternative. Rather than working with scans, I used structured tabular data covering demographics, lifestyle factors, cognitive test scores, biomarkers, and symptom indicators. The aim was to test whether disciplined data curation and honest evaluation could produce reliable predictions from this kind of everyday data.</p>
<br>

<h2>My Research Question and Goals</h2>

<p>The question guiding the whole project was: <b><i>How can Machine Learning and Deep Learning techniques be applied within R and Python-based software to accurately and efficiently predict Alzheimer's disease?</i></b></p>

<p>To answer it, I set myself six goals: review recent work in the field, find and combine suitable datasets, clean and explore the data, look for hidden structure using unsupervised methods, build a range of predictive models, and compare them fairly using a consistent set of metrics.</p>
<br>

<h2>The Data I Used</h2>

<p>No single public dataset had everything I needed, so I combined two from Kaggle. The first was an Alzheimer's Disease dataset with detailed clinical and cognitive variables. The second was a larger global Alzheimer's prediction dataset rich in lifestyle and demographic features. I balanced the second dataset so both classes were equally represented, harmonised the labels, removed identifiers and duplicates, and merged the two into a single analysis-ready table.
The final merged dataset held <b>7,149 patient records across 47 variables</b>. This gave me a broad picture of each individual, from blood pressure and cholesterol to memory complaints, family history, and cognitive test scores.</p>
<br>

<h2>How I Built The Workflow</h2>

<p>I designed the project as a modular pipeline, meaning each stage does one job cleanly and passes its output to the next. This makes the work easy to follow, easy to audit, and easy to reproduce. I used R for the data work and Python for the modelling, playing to the strengths of each.</p>

### Merging and handling missing data
<p>Combining two datasets always leaves gaps. I filled missing values using MICE (Multiple Imputation by Chained Equations), a principled method that estimates each missing value from the other variables rather than just dropping rows or guessing an average. Numeric gaps were filled using a random forest approach and categorical gaps using logistic regression. Crucially, I kept the diagnosis label out of this process so the model could never accidentally learn the answer from the imputation, a common and subtle source of cheating known as data leakage.</p>

### Cleaning and exploring
<p>After merging, I screened for outliers, re-imputed where needed, and ran exploratory data analysis to understand the shape of the data: distributions, relationships between variables, and any warning signs before modelling. Every diagnostic was saved as a report so the reasoning is transparent.</p>

### Finding structure in the data
<p>Before predicting anything, I looked at the data with unsupervised methods. Principal Component Analysis compressed the many variables into a smaller set of directions that capture most of the variation, and K-means and hierarchical clustering checked whether patients naturally fall into groups that line up with the diagnosis. This step builds intuition and helps with feature selection.</p>

### Building and comparing models
<p>I then trained the predictive models three separate times, on three different views of the data: the full original feature set, the compressed PCA version, and a compact set of the 19 most important features ranked by a random forest. Comparing the same models across these three views shows whether more data or smarter feature selection actually helps.</p>
<br>

<h2>The Models I Tested</h2>

<p><b>I compared eight models spanning classic statistics, tree ensembles, and deep learning:</b>

+ Logistic Regression, a transparent statistical baseline 
+ Support Vector Machine with a polynomial kernel
+ Random Forest, an ensemble of decision trees
+ XGBoost, LightGBM, and CatBoost, three modern gradient boosting methods
+ A feedforward Neural Network
+ A one-dimensional Convolutional Neural Network built in TensorFlow

Every model was trained on the same stratified 80/20 split, with fixed random seeds so results can be reproduced exactly.</p>
<br>

<h2>What I Found</h2>

<p>Tree-based ensembles came out on top. On the original feature set, CatBoost gave the best overall result, with the other boosting methods and Random Forest close behind. The deep learning models were competitive but did not beat the ensembles on this kind of structured data, which is a well-known pattern in the field.</p>


| Model | Accuracy | AUC | Notes|
| ------------- | ------------- | ------------- | ------------- |
| CatBoost | 0.822 | 0.893 | BEST |
| Random Forest	| 0.819	| 0.884 | |
| XGBoost	| 0.819	| 0.884 | |
| LightGBM	| 0.814	| 0.889 | |
| Logistic Regression	| 0.789	| 0.869 | |
| Neural Network	| 0.782	| 0.858 | |
| SVM (Polynomial)	| 0.767	| 0.851 | |
| CNN (1D)	| 0.755	| 0.829 | |

<p>To make sure this was not a lucky split, I ran 10-fold cross-validation on the winning CatBoost model. It held up well, averaging 82.8% accuracy and an AUC of 0.906 with low variation across folds. That consistency is what tells me the model generalises rather than memorising.</p>

<p>One useful finding was that the compressed PCA view lost several points of accuracy, which means the full feature detail genuinely carried signal. The compact 19-feature set, by contrast, nearly matched the full set, showing that a small, well-chosen group of variables can do most of the work.
</p>
<br>

<h2>Why The Approach Matters</h2>
<p>The headline is not just that a model reached good accuracy. It is that the whole process is transparent, reproducible, and honest. Fixed seeds, careful controls against data leakage, versioned outputs, and a configuration-driven design mean every result can be reproduced and audited, and the same framework can be pointed at other binary clinical prediction problems with minimal changes.</p>
<p>It also makes a practical point: well-curated tabular data, the kind that is cheap to collect, can support meaningful Alzheimer's screening without imaging. That matters for accessibility and cost in real healthcare settings.</p>
<br>

<h2>Tools and Skills This Demonstrates</h2>
<p><b>Languages and libraries:</b> R (tidyverse, mice, dlookr, ggplot2) for data engineering and EDA; Python (scikit-learn, XGBoost, LightGBM, CatBoost, TensorFlow, pandas, matplotlib) for modelling and evaluation.</p>
<p><b>Techniques:</b> data integration, principled missing-data imputation, feature engineering and selection, dimensionality reduction, clustering, supervised classification, cross-validation, and a full evaluation framework using accuracy, AUC, precision, recall, specificity, F1, and Cohen's kappa.</p>
<p><b>Practices:</b> reproducible, modular software design, version control on GitHub, leakage prevention, and clear documentation from raw data to final result.</p>
<br>
  
<h2>Dissertation</h2>
<p>The full dissertation report is included in the main branch of this repository as a PDF. It documents the complete methodology, literature review, results, and analysis behind this project, including every figure, metric table, and the reasoning at each stage of the pipeline.</p>

### Read the full dissertation (PDF) in the Github Main Repository

<p>Alzheimer's Disease Prediction with R and Python-Based Software Using Machine Learning and Deep Learning. MSc Data Science and Analytics, Brunel University London (2024-2025).</p>
