# Invoice Processing with Azure Form Recognizer

This project demonstrates how to use Azure Form Recognizer to extract information from invoices using Python.

## Table of Contents

- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Setup](#setup)
- [Usage](#usage)
- [Contributing](#contributing)
- [License](#license)

## Introduction

This project leverages Azure Form Recognizer to automate the extraction of key information from invoices. It is a powerful tool for processing large volumes of invoices and extracting relevant data such as invoice number, date, total amount, and more.

## Prerequisites

- Python 3.6 or higher
- An Azure subscription (create one [here](https://azure.microsoft.com/en-us/free/))
- Azure Form Recognizer resource (create one [here](https://portal.azure.com/))

## Setup

1. Clone this repository:
    sh
    git clone https://github.com/yourusername/invoice-processing.git
    cd invoice-processing
    

2. Create a virtual environment and activate it:
    sh
    python -m venv env
    source env/bin/activate  # On Windows, use `env\Scripts\activate`
    

3. Install the required packages:
    sh
    pip install -r requirements.txt
    

4. Set up your Azure Form Recognizer credentials. You can set these as environment variables or directly in your script:
    sh
    export AZURE_FORM_RECOGNIZER_ENDPOINT="your-endpoint"
    export AZURE_FORM_RECOGNIZER_KEY="your-key"
    

## Usage

1. Place the invoices you want to process in the invoices directory.

2. Run the process_invoices.py script:
    sh
    python process_invoices.py
    

3. The extracted data will be printed to the console and saved to a JSON file in the output directory.

## Example

Here is an example of how to use the FormRecognizerClient to process invoices:

```python
from azure.ai.formrecognizer import FormRecognizerClient
from azure.core.credentials import AzureKeyCredential
import os

endpoint = os.getenv("AZURE_FORM_RECOGNIZER_ENDPOINT")
credential = AzureKeyCredential(os.getenv("AZURE_FORM_RECOGNIZER_KEY"))

form_recognizer_client = FormRecognizerClient(endpoint, credential)

with open("invoices/sample-invoice.pdf", "rb") as f:
    invoice_data = f.read()

task = form_recognizer_client.begin_recognize_invoices(invoice_data)
result = task.result()

for invoice in result:
    for field, value in invoice.fields.items():
        print(f"{field}: {value.value}")
