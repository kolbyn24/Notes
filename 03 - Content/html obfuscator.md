---
creation date: July 19th 2024
last modified date: July 19th 2024
aliases: []
tags: #📕
---

Primary Categories: { Add link(s) [[]] back to related PRIMARY categories }
Secondary Categories:  { Add link(s) [[]] back to related SECONDARY categories }
Links: {Add link(s) [[]] to related terms}
Search Tag: #📕  

# [[html obfuscator]]  
___

## Description:  

This example is written in Python, and it uses the `BeautifulSoup` library to parse and manipulate HTML.

### HTML Obfuscator Script

First, make sure you have the required library installed (bash):
```
pip install beautifulsoup4

```
Then, you can use the following script (python):
```
from bs4 import BeautifulSoup
import random
import string
import sys

def obfuscate_html(html_content):
    soup = BeautifulSoup(html_content, 'html.parser')

    # Function to generate random string
    def random_string(length=10):
        letters = string.ascii_letters
        return ''.join(random.choice(letters) for i in range(length))

    # Obfuscate element IDs and classes
    for element in soup.find_all(True):
        if element.has_attr('id'):
            element['id'] = random_string()
        if element.has_attr('class'):
            element['class'] = [random_string() for _ in element['class']]

    # Add invisible HTML content
    invisible_div = soup.new_tag('div', style='display:none;')
    invisible_div.string = random_string(50)
    soup.body.append(invisible_div)

    # Add non-used JavaScript code
    script_tag = soup.new_tag('script')
    script_tag.string = f"""
    // Random unused JavaScript code
    function {random_string()}() {{
        var unusedVar = '{random_string(20)}';
        console.log(unusedVar);
    }}
    """
    soup.body.append(script_tag)

    return str(soup)

def main(input_file, output_file):
    # Read the input HTML file
    with open(input_file, 'r', encoding='utf-8') as file:
        html_content = file.read()

    # Obfuscate the HTML content
    obfuscated_html = obfuscate_html(html_content)

    # Write the obfuscated HTML to the output file
    with open(output_file, 'w', encoding='utf-8') as file:
        file.write(obfuscated_html)

if __name__ == "__main__":
    if len(sys.argv) != 3:
        print("Usage: python obfuscate_html.py <input_file> <output_file>")
        sys.exit(1)

    input_file = sys.argv[1]
    output_file = sys.argv[2]

    main(input_file, output_file)


```

### Usage

1. Save the script as `obfuscate_html.py`.
2. Run the script from the command line, providing the input and output file paths:
```
python obfuscate_html.py input.html output.html

```

___

## Resources:

| Hyperlink | Info |
| --------- | ---- |


Created Date: July 19th 2024 (08:57 am)  
Last Modified Date: <%+tp.file.last_modified_date("MMMM Do YYYY (hh:ss a)")%>
