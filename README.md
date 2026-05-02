PDF to Excel Extractor

📋 Overview
PDF to Excel Extractor is a Streamlit-based web application that automatically extracts structured data from PDF forms and converts it into organized Excel spreadsheets. The tool intelligently identifies key-value pairs and highlights special information (red-colored text) from PDF documents.

✨ Features
🔤 Data Extraction Capabilities
Form Field Extraction: Automatically extracts values below labeled fields in PDF forms

Color-Coded Text Detection: Specifically captures red-colored text spans for special information

Multi-PDF Processing: Batch process multiple PDF files in a single operation

<img width="810" height="877" alt="image" src="https://github.com/user-attachments/assets/f1c9a214-883b-4d92-8351-789f207f6290" />


Structured Output: Organizes extracted data in a clean, tabular Excel format

<img width="1869" height="116" alt="image" src="https://github.com/user-attachments/assets/bb12e6f8-d2cf-418b-a90a-14a051ddc2c9" />


📊 Excel Output Format
Bold Headers: Column headers are formatted in bold for readability

Color-Coding: Red-colored text from PDFs appears in red font in Excel

File Separation: Empty rows between different PDF files for clear distinction

35+ Data Fields: Comprehensive extraction including customer info, billing, medical data, and more



🎯 Supported Data Fields
The application extracts the following 35 data points (automatically detected and organized):

Category	Fields
Customer Information	Customer Name, Email Id, Mobile No, Country, State, City, Zipcode
Billing & Payment	Ref No, Invoice No, Credit Card Type, Credit Card Number, Total Amount, Discount, Net Amount
Order Details	Product Name, Rate, Quantity, Sales Date, Time
Medical Information	Dosage, Blood Group, Sex, Medicine, Tablets, Date Of Birth
Agency Details	Agency Name, Agency Email Id, Agency Mobile No
Shipping	Courier Name
System Data	Stm Code, Stm Name
Auto-generated	Form No, Start Date
Additional Fields	Amount Word, Remarks

🛠️ Installation
Prerequisites
Python 3.8 or higher

Installation Steps
bash
# Clone or download the project
git clone QC-Workflow-AutomationSystem 

# Create virtual environment (recommended)
python -m venv venv

# Activate virtual environment
# On Windows:
venv\Scripts\activate
# On macOS/Linux:
source venv/bin/activate

# Install required packages
pip install -r requirements.txt

Running the Application
bash
# Navigate to project directory
cd pdf-to-excel-extractor

# Run the Streamlit app
streamlit run app.py
Using the Application
Launch the Application: Open your browser and go to http://localhost:8501

Upload PDF Files: Click "Browse files" to select one or multiple PDF documents

Automatic Processing: The app immediately processes all uploaded PDFs

Download Results: Click "Download Excel" to save the extracted data as an Excel file

Expected PDF Format
The tool works best with PDFs that have:

Label-value pairs (e.g., "Customer Name:" on one line, actual name on next line)

Standardized field labels matching the predefined headers

Optional red-colored text for special annotations

📁 Output Format
Excel File Structure
text
| Form No | Start Date | Ref No | Invoice No | Customer Name | ... |
|---------|------------|--------|------------|---------------|-----|
| [Header Row - Bold]                                           |
| [Extracted Values from PDF - Row 2]                           |
| [Red-colored text from PDF - Row 3, Red font]                 |
| [Empty Row - File Separator]                                  |
| [Next PDF Headers]                                            |
| ...                                                           |
Sample Output
Each PDF generates:

Row 1: Column headers (bold)

Row 2: Extracted field values

Row 3: Red-colored text (if present)

Empty row before next PDF's data

🏗️ Technical Details
Core Dependencies
Streamlit: Web application framework

PyMuPDF (fitz): PDF parsing and text extraction

OpenPyXL: Excel file creation and formatting

Extraction Logic
Text Extraction: Uses PyMuPDF to extract all text from PDF pages

Field Matching: Identifies field labels and extracts the following line as the value

Color Detection: Scans for spans with RGB color 16711680 (pure red)

Data Organization: Maps extracted values to predefined column headers

Key Functions
extract_value(text, field): Extracts value below a field label

extract_red_spans(doc): Captures all red-colored text spans

BytesIO handling: In-memory Excel file creation for efficient download

⚙️ Configuration
Customizing Headers
To modify which fields are extracted, edit the column_headers list:

python
column_headers = [
    "Form No", "Start Date", "Ref No", "Invoice No", 
    # ... add or remove fields as needed
]

exclude_headers = {"Form No", "Start Date", "Amount Word", "Remarks"}
Color Detection
To detect different colors, modify the color code in extract_red_spans():

python
if span.get("color") == 16711680:  # Change this RGB value
🐛 Troubleshooting
Common Issues
Issue	Solution
No data extracted	Check PDF format - ensure labels match exactly
Red text not detected	Verify text is actually red (RGB: 16711680)
Excel download fails	Ensure write permissions in temp directory
Missing fields	Add field labels to column_headers list
Debug Tips
Check PDF text extraction by adding debug prints

Verify field labels exactly match PDF content (including colons/spaces)

Test with a simple PDF first to ensure basic functionality

📈 Performance
Processing Speed: ~1-2 seconds per page (depending on complexity)

Memory Usage: Efficient streaming - processes files without full memory load

Scalability: Can handle multiple PDFs simultaneously

🔧 Extending Functionality
Adding New Fields
python
# Add to column_headers list
column_headers.append("New Field Name")

# The extract_value function will automatically look for this label
Supporting Different Color Codes
python
def extract_colored_spans(doc, rgb_color):
    colored_texts = []
    for page in doc:
        blocks = page.get_text("dict")["blocks"]
        for block in blocks:
            for line in block.get("lines", []):
                for span in line.get("spans", []):
                    if span.get("color") == rgb_color:
                        colored_texts.append(span["text"].strip())
    return colored_texts

# Eg PDF has been given 

# Eg Output has been given
    
🤝 Contributing
Fork the repository

Create a feature branch (git checkout -b feature/improvement)

Commit changes (git commit -am 'Add new feature')

Push to branch (git push origin feature/improvement)

Create a Pull Request

📄 License
This project is available under the MIT License. See the LICENSE file for details.

🙏 Acknowledgments
Streamlit for the amazing web app framework

PyMuPDF for robust PDF processing capabilities

OpenPyXL for Excel file manipulation

📞 Support
For issues or feature requests:

Check the existing issues

Create a new issue with sample PDF (remove sensitive data)

Describe the expected vs actual behavior

Note: This tool is designed for structured PDF forms. Results may vary with scanned or image-based PDFs. For best results, ensure PDFs have extractable text (not just images of text).
