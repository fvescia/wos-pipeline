## Context

I began this pipeline in [MACS 30113 Big Data and High Performance Computing](https://github.com/macs30113-s24/course-materials) at the University of Chicago. Since then, the UChicago library has made Web of Science data available in [TDM Studio](https://guides.lib.uchicago.edu/tdmstudio), a cloud-based tool for text mining and analysis. This negates the need for a custom solution, but `#TODOs` throughout my code note how I would have approached developing this into a fully functional pipeline.

This sample demonstrates my programming style and how I structure technical projects. 

## Citation Analysis and the Web of Science

Citation analysis is a well-established form of social science inquiry used to explore knowledge creation and dissemination. For example, [McMahan and McFarland (2021)](https://journals.sagepub.com/doi/full/10.1177/0003122421996323#fn6-0003122421996323), the inspiration for this work, show that publications cited in formal reviews typically experience a dramatic loss in future citations, since future works tend to cite the reviews rather than the publications featured in them. They argue the reviews, which receive disproportionate attention, “perform curatorial work that substantially transforms the research communities they aim to summarize.”

Clarivate Analytics’ [Web of Science (WoS)](https://clarivate.com/products/scientific-and-academic-research/research-discovery-and-workflow-solutions/webofscience-platform/), a rich trove of publication and citation data, is a common source for citation analyses. One way to access WoS data is via Clarviate’s Web of Science [APIs](https://clarivate.com/products/scientific-and-academic-research/research-analytics-evaluation-and-management-solutions/web-of-science-apis/). Institutions can also purchase WoS data for their researchers. For example, structured [XML data](https://guides.lib.uchicago.edu/textmining/citations#s-lg-box-28004408) of the entire Web of Science database from 1900-2023 is available through the University of Chicago Library. 

There is no need for researchers  with institutional access to the XML data to pay for Web of Science API access unnecessarily. Here, I develop an alternative workflow employing Amazon Web Services EMR clusters for them to consider. First, I describe why cluster computing is particularly well-suited to this task. I then detail my workflow and discuss directions for future work.

## Why Cluster Computing? 

The scale of analyses that explore whole bodies of literature is necessarily vast. When I began this work, I planned to explore a subset of the WoS XML locally, to gain familiarity with the data, before scaling up to an AWS cluster. Downloading the 2023 WoS data zip file from the library website took twenty minutes. First, I tried to read one of the twenty-five XML files from the zip file into a Jupyter notebook. My call to `open()` ran for another twenty minutes before crashing my Jupyter kernel. Next, I tried to open the XML file in Microsoft Word to excerpt a portion of it; the file was too large for Word to open. I finally managed to open the file in TextEdit and copy a subset of the records in it over into a smaller XML file, which I was able to read into my notebook.

In short, working locally with a *partial* year of WoS data required clumsy workarounds. And any rigorous citation analysis will likely cover a much larger time frame. For example, McMahan and McFarland “limit” their analysis to work published from 1990 through 2016. Working on a computing cluster, researchers have more space and power to store and process extremely large files. Additionally, even high-end personal computers are quite limited in the number of processes they can run concurrently. Clusters, in contrast, facilitate extensive parallelism, enabling researchers to better exploit the inherent parallelism in many large data processing and analysis tasks. 

## Step 0: Retrieve WoS XML

The first step to working with the WoS data is to retrieve it from the UChicago library website. Here, I work with the Annuals, ZIP files of WoS Core Collection data by year. The files are *large*: unzipped, the first of the twenty-five files in `2023_CORE` is 3.28 GB. Ideally, I would scrape them from a cluster, parallelizing the work with [Pyspark](https://medium.com/@siladityaghosh/web-scraping-with-python-parallizing-and-scaling-with-spark-b7d2166602b7). Unfortunately, the files are restricted, and scraping with authentication is beyond the scope of this project. (Throughout my code, `#TODOs` note potential extensions of this work, such as using GET and POST requests to programmatically log into the library website to scrape the restricted files.) For now, I download the `2023_CORE` ZIP file manually to my local system, then programmatically unzip it and write a portion of its contents to an AWS S3 bucket.

## Step 1: Write WoS XML to S3 
[xml-to-s3.ipynb](https://github.com/fvescia/wos-pipeline/blob/main/xml-to-s3.ipynb) | System: Local

Unzipped, `2023_CORE` contains twenty-five zipped XML files. Writing the first of these files, unzipped, to S3 from my local system took twenty minutes, so I work with a portion of the 2023 data, writing three of the unzipped files to S3 to demonstrate how a for loop can be used to automatically process multiple files. The loop unzips the first file, writes it to S3, and deletes it, then repeats the process with the second and third files; I had to delete each file before unzipping the next to avoid running out of space on my system.

## Step 2: Convert WoS  XML to Parquet
[xml-to-parquet.ipynb](https://github.com/fvescia/wos-pipeline/blob/main/xml-to-parquet.ipynb) | System: EMR Cluster | Kernel: Python

Parquet is a column-based data format that is more space-efficient and faster to query than row-based XML. Ideally, this workflow would use BlackRock’s open source [XML-to-Parquet converter](https://github.com/blackrock/xml_to_parquet) to convert the WoS XML to Parquet. Unfortunately, while I was able to run the module’s [test job]( https://github.com/blackrock/xml_to_parquet/tree/master/test) on an EMR cluster, I could not get it to work with the WoS XML. I believe the issue is with schema specification; the test job references a single `xsd` file, while the WoS XML is defined by a hierarchy of schemas (per p. 6-7 of the [WoS XML user guide]( https://guides.lib.uchicago.edu/ld.php?content_id=75223913)), which the converter may not understand how to work with. If the WoS XML could be successfully converted, the resulting Parquet files could be written back to S3 in parallel using the `concurrent.futures` module. I demonstrate this is possible by making several copies of the test Parquet file and writing them to S3 in parallel.

## Step 3: Analyze
[parse-parquet.ipynb](https://github.com/fvescia/wos-pipeline/blob/main/parse-parquet.ipynb) | System: EMR Cluster | Kernel: PySpark

Concurrent.futures also works to read Parquet files from S3 into a Spark session in parallel.  The files can be merged into a single dataframe, which can then be analyzed with pyspark.sql. Since I could not convert the WoS XML to Parquet, I demonstrate parallel reading with files from the XML-to-Parquet converter test job and analyzing nested Parquet data with open-source, auto-generated [JSON data](https://jsoneditoronline.org/indepth/datasets/json-file-example/) read into my Spark session as a Parquet file. For example, I show how to identify distinct names within a nested Parquet file, which would be very useful for citation analysis.

## Future Directions

As I note above, the ideal version of this  workflow would scrape ZIP files from the University of Chicago Library website programmatically and successfully run the WoS XML through a XML-to-Parquet converter. Future work could explore the possibility of using GET and POST requests to navigate the library’s authentication process and determine whether the converter can work with multi-file, hierarchical schemas. Once the workflow is optimized, it would be valuable to calculate the time, effort, and cost of analyzing WoS data on EMR clusters, so researchers can determine whether this approach or purchasing WoS API access is a better use of their resources.

**LLM Reference**  
OpenAI. (2023). ChatGPT (Feb 13 version) [Large language model]. https://chat.openai.com/chat.

