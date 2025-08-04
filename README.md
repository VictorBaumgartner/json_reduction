Gabon Data Processing Script
Overview
This Python script processes multiple JSON files (named gabon1.json to gabon40.json) containing data about various establishments in Gabon. It consolidates and cleans the data, removing duplicates, null values, and closed establishments, and outputs a single, well-structured JSON file (processed_gabon_data.json).
Functionality
The script performs the following tasks:

File Reading: Scans the current working directory for JSON files named gabonX.json (where X is a number from 1 to 40).
Data Consolidation: Merges data from multiple JSON files, handling both single-object and list-based JSON structures.
Duplicate Removal: Consolidates entries with the same Title (name) into a single entry, combining their opening hours and prioritizing non-empty, non-null values for other fields.
Data Cleaning: Removes null, empty, or "Horaires non disponibles" values, and skips entries marked as "Fermé" (closed).
Field Mapping: Processes specific fields to create a standardized output with the following attributes:
name: Establishment name (from Title).
image_url: Image URL (from Image).
category: Establishment category (from textsm).
city: City name (from textsm2).
phone_number: Phone number (from Field2_text or Field8_text, with "tel:" prefix removed).
email: Email address (from Text).
description: Longest description from Field11, Field3, or textsm3.
address: Address (from Field3).
ratings: Ratings (from Field14).
opening_hours: Consolidated opening hours from Field15, joined with semicolons.


Output: Saves the processed data to processed_gabon_data.json in the current working directory.

Requirements

Python 3.x
Standard libraries: os, json

Usage

Place the script (process_gabon_data.py) in the same directory as the input JSON files (gabon1.json to gabon40.json).
Run the script:python process_gabon_data.py


The script will:
Read all valid JSON files.
Process and consolidate the data.
Output the cleaned data to processed_gabon_data.json.


Check the console for any error messages related to file reading or JSON decoding.

Input JSON Structure
The input JSON files are expected to contain objects or lists of objects with fields like:

Title: Establishment name
Image: Image URL
textsm: Category
textsm2: City
px2: Status (e.g., "Fermé" for closed)
Field2_text, Field8_text: Phone numbers
Text: Email address
Field11, Field3, textsm3: Description candidates
Field3: Address
Field14: Ratings
Field15: Opening hours

Output JSON Structure
The output JSON file (processed_gabon_data.json) contains a list of objects with the following fields (where available):

name: Establishment name
image_url: Image URL
category: Category
city: City
phone_number: Phone number
email: Email address
description: Longest available description
address: Address
ratings: Ratings
opening_hours: Semicolon-separated opening hours

Notes

The script skips entries with missing titles or those marked as "Fermé".
Duplicate entries (based on Title) are merged, with opening hours combined and other fields populated with the first non-empty, non-null value.
The script handles encoding issues by using utf-8 for file operations.
Error handling is implemented for invalid JSON files and file access issues.

Example
Input (gabon1.json):
[
    {
        "Title": "Rhum'A Libreville",
        "Image": "https://example.com/image.jpg",
        "textsm": "Bar-Lounge",
        "px2": "Fermé",
        "textsm2": "Libreville",
        "Field2_text": "+24177666175",
        "Field15": "MARDI 16:00 - 22:00"
    },
    {
        "Title": "Rhum'A Libreville",
        "Image": "https://example.com/image.jpg",
        "textsm": "Bar-Lounge",
        "px2": "Fermé",
        "textsm2": "Libreville",
        "Field2_text": "+24177666175",
        "Field15": "MERCREDI 16:00 - 22:00"
    }
]

Output (processed_gabon_data.json):
[]

(Note: In this example, the output is empty because all entries are marked "Fermé".)
