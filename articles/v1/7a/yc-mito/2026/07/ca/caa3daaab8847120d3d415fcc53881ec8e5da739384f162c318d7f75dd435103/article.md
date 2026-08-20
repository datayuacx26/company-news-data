---
schema_version: "1.0.0"
document_id: "caa3daaab8847120d3d415fcc53881ec8e5da739384f162c318d7f75dd435103"
company_key: "yc-mito"
company: "Mito"
source_id: "yc-mito-news-import-220d1fd2c6bc"
canonical_url: "https://www.trymito.io/blog/how-to-convert-ipynb-to-pdf"
published_at: null
first_seen_at: "2026-07-22T04:33:06.243498+00:00"
fetched_at: "2026-07-28T21:20:14.720808+00:00"
content_hash: "sha256:3bb096b2baa34def87974226fc4aff947fadd640fc58f889566d5852c58837e1"
---

# How to Convert ipynb to PDF

Turn data into insights and reports 4x faster with Mito AI


[Download Mito](https://www.trymito.io/downloads)


So you’ve got a Jupyter Notebook, and you’re ready to share it with your colleagues. One problem, sending it over as an .ipynb is risky. Does your manager have the same version of Python, what packages does he have installed, can he install packages. Better send over a PDF instead.


Tried, tested and not exactly straightforward. Generating PDFs from a Jupyter notebook can be done in a few ways. Before you get started you’ll want to decide if you want to a *quick and dirty* PDF, or you’re willing to open up the terminal and write a quick script — also pretty quick, to be fair.


## Option 1: Using the print menu in Jupyter


This method is fairly straightforward, you open your Notebook and then select *print* . Then when the print dialog appears, select “Save as PDF” under *Destination* .


Pros:


- Quick as it gets.
- Not additional code/packages required.


Cons:


- Not all outputs are included.
- Lack of customizability — can’t hide code cells.


## Option 2: Installing nbconvert to Generate PDF from ipynb


This method is a bit more involved, but not much more than you’d think.


First, install` nbconvert` by running this command in your terminal:


```text
pip install nbconvert[webpdf]


```


If you encounter an error, try:


```text
pip install nbconvert\\[webpdf\\]


```


Once installed, you can convert your notebook by running:


```text
jupyter nbconvert --to webpdf --allow-chromium-download YOUR_NOTEBOOK_NAME.ipynb


```


Replace` YOUR_NOTEBOOK_NAME.ipynb` with your actual file name.


### Customizing the nbconvert Export


To create a PDF without the code cells, add the` --no-input` option:


```text
jupyter nbconvert --to webpdf --allow-chromium-download --no-input YOUR_NOTEBOOK_NAME.ipynb


```


To execute the code cells in your Jupyter Notebook:


```text
jupyter nbconvert --to webpdf --execute YOUR_NOTEBOOK_NAME.ipynb


```


## Option 3: Creating a Script to Convert ipynb to PDF


Maybe opening the terminal every time you want to create a PDF is too much of a hassle. No worries, we can use nbconvert to create a script to create our PDF.


Let’s start by importing the packages we’ll use:


```text
import nbformat
from nbconvert.exporters.webpdf import WebPDFExporter


```


Next, we’ll open the notebook:


```text
with open("YOUR_NOTEBOOK.ipynb", "r", encoding="utf-8") as file:
notebook_content = nbformat.read(file, as_version=4)


```


Next, we create and use the exporter:


```text
pdf_exporter = WebPDFExporter(
allow_chromium_download=True,
exclude_input=False
)


pdf_data, resources = pdf_exporter.from_notebook_node(notebook_content)


```


Finally, we can save the PDF:


```text
with open("OUTPUT_PATH/YOUR.PDF", "wb") as file:
file.write(pdf_data)


```


Here’s the full script, slightly expanded, and turned into a function:


```text
import nbformat
from nbconvert.exporters.webpdf import WebPDFExporter


def convert_notebook_to_pdf(notebook_path, output_path=None, hide_code=False):
"""
Convert a Jupyter notebook to PDF using nbconvert library


Args:
notebook_path: Path to the notebook file
output_path: Path for the output PDF file (optional)
hide_code: If True, code cells will be hidden in the output (optional)


Returns:
Path to the generated PDF file
"""
# Check if file exists
if not os.path.exists(notebook_path):
raise FileNotFoundError(f"Notebook file '{notebook_path}' not found.")


print(f"Converting notebook: {notebook_path} to PDF...")


# Read the notebook
with open(notebook_path, "r", encoding="utf-8") as file:
notebook_content = nbformat.read(file, as_version=4)


# Create the exporter with parameters
pdf_exporter = WebPDFExporter(
allow_chromium_download=True,
exclude_input=hide_code  # This parameter hides code cells
)


# Export to PDF
pdf_data, resources = pdf_exporter.from_notebook_node(notebook_content)


# Determine output path
if output_path is None:
output_path = os.path.splitext(notebook_path)[0] + ".pdf"


# Save the PDF
with open(output_path, "wb") as file:
file.write(pdf_data)


print(f"✓ Conversion successful! PDF saved to: {output_path}")
if hide_code:
print("Note: Code cells have been hidden in the PDF.")
return output_path


```


## Wrapping up


Converting a Jupyter Notebook to a PDF doesn’t have to be a hassle. Whether you need a quick export using the print menu, a more structured approach with` nbconvert` , or a fully automated script, there’s a method to fit your workflow. If you just need a basic PDF, the print option is the easiest. For more control,` nbconvert` allows customization, including hiding code cells and executing notebooks before exporting. And if you find yourself frequently converting notebooks, automating the process with a script can save time in the long run.


No matter which method you choose, these options ensure that sharing your work with colleagues is as smooth and professional as possible—without worrying about dependencies or setup issues.


## More Like This


[Automating Spreadsheets with Python 101 How to tell the difference between a good and bad Python automation target.](https://www.trymito.io/blog/automating-spreadsheets-with-python-101)[10 Mistakes To Look Out For When Transitioning from Excel To Python 10 Common Mistakes for new programmers transitioning from Excel to Python](https://www.trymito.io/blog/10-mistakes-to-look-out-for-when-transitioning-from-excel-to-python)[Research shows Mito speeds up by 400% We're always on the hunt for tools that improve our efficiency at work. Tools that let us accomplish more with less time, money, and resources.](https://www.trymito.io/blog/quantifying-mitos-impact-on-analyst-python-productivity)[3 Rules for Choosing Between SQL and Python Analysts at the world's top banks are automating their manual Excel work so they can spend less time creating baseline reports, and more time building new analyses that push the company forward.](https://www.trymito.io/blog/choosing-between-sql-and-python-best-practices-for-data-analytics-workflows)


Turn data into insights and reports 4x faster with Mito AI


[Download Mito](https://www.trymito.io/downloads)
