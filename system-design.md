# Entity Resolution System Design Interview Question (30 minutes)

## Background
You are tasked with designing an entity resolution pipeline for a large-scale database containing millions of records. The system should identify and merge similar entities that represent the same real-world location or business but have slight variations in their representation. The system will consider address and business name data.

## Examples
Consider the following sets of entities that should be identified as the same:

Set 1 (Addresses):
- "2730eL PRESIDIO STREET CARSON CA90810 US"
- "2730EL PRESIDIO STREET CARSON CA 90810 US"
- "2730EL PRESIDIO STREET CARSON CA 90 810 US"
- "2730EL PRESIDIOSTREET CARSON CA90810 U S"

Set 2 (Business names and addresses):
- "Joe's Pizza, 123 Main St, New York, NY 10001"
- "Joes Pizza NYC, 123 Main Street, New York, New York 10001"
- "Joe's Famous Pizzeria, 123 Main St., NYC, NY 10001"

Set 3 (Businesses with multiple locations - only exact matches should be merged):
- "Starbucks Coffee, 200 Park Ave, New York, NY 10166"
- "Starbucks, 200 Park Ave NY "
- "Starbucks Coffee, 1000 S Pine Island Rd, Plantation, FL 33324" (should not be merged with the above)

## Requirements
Design a system that can:
1. Ingest and process large volumes of data (assume millions of records) including addresses and business names.
2. Identify and group similar entities using appropriate algorithms and techniques.
3. Merge or link identified entities while maintaining data integrity.
4. Ensure that businesses with multiple locations are only merged if they are exact matches.
5. Scale horizontally to handle increasing data volumes and processing requirements.
6. Provide a way to query the resolved entities efficiently.

## Expected Deliverables
1. High-level system architecture diagram
2. Description of key components and their interactions
3. Explanation of algorithms or techniques used for entity matching, including code snippets for core logic
4. Data model for storing raw and resolved entities
5. Approach for scaling the system to handle large data volumes
6. Discussion of potential challenges and how to address them

## Coding Expectation
Provide a code snippet (in a language of your choice) for a core algorithm in your entity matching process. This could be a function that:
- Calculates similarity between two strings (e.g., using Levenshtein distance or Jaccard similarity)
- Determines if two business entities should be merged based on name and address similarity
- Implements a blocking strategy to reduce the number of comparisons needed

## Questions to Consider
As you design your system, consider the following questions (and feel free to ask any of your own):
1. How would you handle different languages or character sets in business names and addresses?
2. What strategies would you employ to reduce false positives and false negatives in entity matching?
3. How would your system handle updates to existing entities or the addition of new data over time?
4. What approach would you take to optimize query performance on the resolved entities?
5. How would you ensure that businesses with multiple locations are only merged when they are exact matches?

You have 30 minutes to design this system. Be prepared to discuss your design choices, trade-offs, and any assumptions you've made. Don't hesitate to ask clarifying questions throughout the interview.

## Evaluation Criteria
- Scalability and performance considerations
- Choice and justification of algorithms and technologies
- Data modeling and storage decisions
- Handling of edge cases and data quality issues
- Overall system design and component interactions
- Quality and efficiency of the provided code snippet
- Ability to ask insightful questions and make reasonable assumptions
- Approach to handling businesses with multiple locations
