**Data-Driven Maintenance Prioritisation using Machine Performance and Health Data**

This is the code repository for my MSc Data Science dissertation (module M599) at Gisma University of Applied Sciences.

In this project I use the AI4I 2020 predictive maintenance dataset to predict which machines are likely to fail. I then use those predictions to build a ranked maintenance list, so a maintenance team knows which machines to check first and why.

What's in this repository
my_thesis_code.ipynb – the full notebook. I ran it from top to bottom in Google Colab, and all outputs are saved inside it.
requirements.txt – the library versions I used.
outputs/ – every table (CSV) and figure (PNG) the notebook produces, plus the dashboard (maintenance_dashboard.html). To view the dashboard, download the HTML file and open it in a browser.
Data

I used the AI4I 2020 Predictive Maintenance Dataset from the UCI Machine Learning Repository (DOI: 10.24432/C5HS5C).

It has 10,000 rows. 339 of them are machine failures (3.39%) and 9,661 are not, so the data is very imbalanced. I treat a failure as the positive class (1) and a non-failure as the negative class (0).

I left out the five failure-type columns (TWF, HDF, PWF, OSF, RNF), because they directly describe the failure and would leak the answer to the model. I also left out the two ID columns (UDI and Product ID).

Running the notebook
Open my_thesis_code.ipynb in Google Colab.
Run the first cell. It connects Google Drive and creates a thesis_maintenance/outputs folder.
Go to Runtime → Run all. The whole notebook takes around 20–25 minutes.
The tables, figures and dashboard are saved into outputs/.

I set the random seed to 42 everywhere, so you should get the same numbers as in the thesis.

How the notebook links to the thesis
Notebook section	What it does	In the thesis
2	Target variable and class balance	Table 5.1
3	New features and exploratory analysis	Table 5.2
4	Train/test split and preprocessing	Table 4.1
5	Baseline models	Section 5.3
6	Screening of all models	Table 5.3
7	5-fold cross-validation	Table 5.4
8	Hyperparameter tuning	Table 4.2, Section 5.6
9	Comparing the tuned models on the test set	Table 5.5, Figures 5.1–5.2
10	Looking at thresholds on the test set (descriptive only)	Section 5.7
11	Choosing the threshold using out-of-fold predictions	Figure 5.3
12	Calibration checks (Brier score, log loss, ECE) and bootstrap confidence intervals	Figure 5.4, calibration table
13	Global SHAP feature importance	Table 5.6
14	Maintenance priority list and the main risk driver for each machine	Table 5.7, Figures 5.5–5.6
15	Priority groups and fixed inspection capacity (top 5%, 10% and 20%)	Tables 5.8–5.9
16	Maintenance dashboard	Figures 5.7–5.11
17	Library versions	Appendix

Key results

Random Forest was my final model. It reached a PR-AUC of 0.8985 on the test set.
I chose a threshold of 0.42 using out-of-fold predictions from the training data, not the test set. At this threshold the model caught 56 of the 68 failures in the test set, with only 4 false alarms (precision 0.933, recall 0.824).
In the test set, 60 machines landed in the High priority group (56 of them actually failed), 93 in Medium (8 failed) and 1,847 in Low (4 failed).
If the team can only inspect the top 100, 200 or 400 machines, they would find 59, 65 and 65 of the failures. That is 86.8%, 95.6% and 95.6% of all failures, a lift of 17.4, 9.6 and 4.8 over random inspection.
Limitations

The AI4I 2020 data is synthetic. It has no time order, no maintenance costs and no repeated readings for the same machine. So these results show that the approach works in principle, but it would need testing on real factory data before anyone used it in practice.
