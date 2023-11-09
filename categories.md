# Evaluation metrics for metadata extraction


Terminology:

- Dataset / annotation: manually prepared / verified metadata
- Result / result set: metadata extracted by program
- Noise: Unwanted / meaningless data


## ISBN

Type: Set of pairs (ID, tag), where ID is the ISBN identifier and tag is either print or digital.

Dashes are discarded from the ID.

Possibilities:

1. Identical non-empty set: correct result
2. Empty set in result and dataset: correct, but irrelevant / no data
3. Correct set of IDs, but with wrong tag
4. Overlap with missing data (subset)
5. Overlap with extraneous data (superset)
6. No overlap – empty result set
7. No overlap – extraneous data

Questions:

- How do we account for "partly correct" results?
- Which score for correct IDs but incorrect tag?


## ISSN

Identical to ISBN, except for dashes, since ISSNs have a stricter form.


## Title

Type: String.

Possibilities:

1. Equality
2. Different casing, valid (e. g. country names are uppercase)
3. Different casing, invalid (e. g. country names are lowercase)
4. Result is too long, includes some of the subtitle
5. Result is too long, includes some "noise"
6. Result is too short
7. Result is just "noise"


Questions/comments:

- For too long/too short situations, are we considering “substrings” (overlap is anywhere) or “startswith” (overlap is at the start)?
- What is the interplay of overlap and casing?
- Situation (4) (subtitle): requires having the subtitle available in the annotations
- Using editdistance should be thought through: ED("correct title", "The correct title") ==  ED("correct title", "coXrreXct tXitlXe"), but the first is better (intuitively...)


## Language

Type: string, 3-letters identifier ISO 639-2.

Questions:

- Do we just evaluate strict equality, or consider language proximity? (e.g. nob / nno is closer than eng / swe)


## Publication type

String, strict equality.


## Publication year

Type: int

Possibilities:

1. Equality
2. Missing from both annotation and result
3. Different values
4. Value in result but not in annotation ("hallucination")
5. Value in annotation but not in result ("miss")


## Authors

Type: Set of names, structured as first name / last name.

Possibilities:

1. Identical set
2. Empty set in both result and dataset
3. Overlap with missing names (subset)
4. Overlap with extra values (actual person names)
5. Overlap with extra values (noise / not actual names)
6. Some overlap, with both missing names and extra values
8. No overlap (empty result set)
9. No overlap (extraneous data)

Questions:

- How to detect automatically whether a name is a person's name or not? Is this even necessary for the evaluation?
- How to take persons' roles into account?
- How to score "correct name with incorrect structure" (first and last name inverted, missing first name...)


## Publisher

Type: String (+ authority file ID?)

Possibilities:
1. Equality
2. Correctly located, but writing form differs
3. Incorrectly located, but result is an organization named in the document
4. Noise

Questions:

- How to automatically evaluate 2 & 3?
- AuthId evaluation / linked entities?


## General comments

The fully correct or fully incorrect situations seem quite easy to pick up, and to score (1 / 0). The main questions are on the partially correct results.

I suggest that before trying to define some scoring formulas for these, we go through the lists of possible situations for each field to decide which of them are worth considering, and whether we need more/fewer "levels".
