# Creating a Data Transformation Pipeline with Cloud Dataprep (GSP430)



This tutorial introduces how to use Google Dataprep (Trifacta) transforming the dataset via the data source `BigQuery`.

The most content is related to the introduction of the tool `Trifacta`. Three basic concepts within the steps are `Loading`, `Cleaning` and `Enriching` the dataset.

Trifacta is a web-based tool (suggested using Chrome as the browser). **The most significant characteristic of the tool is providing descriptive statistics to each column (attribute)**. It is convenient for you to transform the data based on statistical information. In addition, it provided lots of built-in formulas to enrich the data, like numeric calculation, etc. The basic workflow in Trifacta is to create a `Flow`. That means the whole workflow from the beginning to the end. Each step in the flow is also transformed by a `Recipe`. In each recipe, you can clean the data, like removing the null column, or you can enrich the data, like calculation, etc. After transforming the data, you can assign the output like `BigQuery` or `csv file` as the target. 



keywords: `Dataprep`, `BigQuery`, `Trifacta`



## Reference

* Qwiklabs: <https://www.qwiklabs.com/focuses/4415?parent=catalog>



