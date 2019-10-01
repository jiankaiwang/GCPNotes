# Dataprep: Qwik Start



## QuickNote

Bucket name requirements

*   <https://cloud.google.com/storage/docs/bucket-naming#requirements>



## Create a Cloud Storage bucket in your project

Create a bucket named `qwik-dataprep` on Storage service.



## Initialize Cloud Dataprep

Select **Navigation menu** > **Dataprep**.

Allow Google Dataprep Terms of Service and accept sharing data with Trifacta (Data Wrangling Tools).



## Create a flow

In the following prgress, we are going to use this tool `Trifacta`.

Steps:

*   Click `Create Flow`.
*   This lab uses 2016 data from  [United States Federal Elections Commission 2016](https://classic.fec.gov/finance/disclosure/ftpdet.shtml#a2015_2016). We name the flow `FEC-2016` and describe the flow as `United States Federal Elections Commission 2016`.



## Import datasets

Steps:

*   Click **Import & Add datasets**.

*   In the left panel, click `GCS` to import datasets from Google Cloud Storage. Click the pencil to edit the file path.
*   Insert `gs://dataprep-samples/us-fec` in the text box and click `Go`.
*   Select `cn-2016.txt` t0 create a dataset shown in the right panel and rename it `Candidate Master 2016`.
*   Select `itcont-2016.txt` to create a dataset and rename it `Campaign Contributions 2016`.
*   Click `import & Add to Flow`.



## Prep the candidate file

Steps:

*   Click `Add New Recipe`.
*   Click `Edit Recipe`.

*   Select 2016-2017 on Column5 and click `Add` on the **Keep rows** on the right panel.
*   Change the datatype into **String** on the column 6 to avoid the incorrect mismathing on states and country.
*   Select the 'P' on ccolumn7 (stands for the msimatching on column6) and click `Add` on the **Keep rows** on the right panel.



## Join the Contributions file

Steps:

*   Click on `FEC-2016`.
*   Click to select `Campaign Contributions 2016`.
*   `Add New Recipe` and then click `Edit Recipe`.

*   Click the `recipe` icon at the top of the page, then click `Add New Step`.

*   Insert the following Wrangle language command in the Search box and click `Add`.

```text
replacepatterns col: * with: '' on: `{start}"|"{end}` global: true
```

*   Click `New Step` and type `join` in the Search box.
*   Click `Join datasets`.
*   Click on `Candidate Master 2016-2` to join with Campaign Contributions-2, then Accept in the bottom right.
*   In the Join keys section, then click on the pencil close to the column name.
*   Click `column2 = column11` on the bottom `Suggested join keys`.
*   Click `Save and Continue`.
*   Click `Next`, select all to add all columns of both datasets to the joined dataset.
*   Click `Preview` and `Add to Recipe` to return to the grid view.



## Summary of data

Steps:

*   Click on **New Step** and type the following in the Transformation Search box.

```text
pivot value:sum(column16),average(column16),countif(column16 > 0) group: column2,column24,column8
```

*   Click `Add`.



## Rename columns

Steps:

*   Click on **New Step** and type the following in the Search box.

```text
rename type: manual mapping: [column24,'Candidate_Name'], [column2,'Candidate_ID'],[column8,'Party_Affiliation'], [sum_column16,'Total_Contribution_Sum'], [average_column16,'Average_Contribution_Sum'], [countif,'Number_of_Contributions']
```

*   Click `Add`.
*   Add a `New Step` to round the Average Contribution amount.

```text
set col: Average_Contribution_Sum value: round(Average_Contribution_Sum)
```













