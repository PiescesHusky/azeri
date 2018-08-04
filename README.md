# azeri
## key observations
* azeri has no prefix (at least, none was found in the data)
* in train set: number of rules = 5171; number of minimal rules = 205; 69, 102, 124
## 2018.8.3
    @ yanzhe, add brief introduction here of the current method and performance. e.g. 5 lines per task.
## 2018.8.4 by yanzhe

Data:
Train_low: 100
Train_medium: 1000
Dev: 100
no lemma is shorter than 2 letters

Task 1: 
Given lemma and POS, produce inflected form. Required to use Levenshtein Distance to extract rules (not implemented)

Task 2: 
Given lemma and POS, produce inflected form. Use any feature we think best.
Implementation: 
1 Rule Extraction:
1) match the lemma and inflected form letter by letter from left to right, stop when mismatched letter is encountered. All letters starting from mismatched position will be used as rule. If no letter is left, use "0" (literal zero) in the rule. 
Examples of rules extracted:
take - took  PastTense  
(ake, ook) PastTense  Rule1
read - read Perfect
(0, 0) Perfect   Rule2
play - played PastTense
(0, ed) PastTense   Rule3

2) Extracted rules are stored in a defaultdictionary using POS(such as PP3P) as key. Since a POS might have multiple rules, such as Rule1 and Rule3 in the example above, a list is used as value for the key. Multiple rules for a POS are stored under the same POS key in the dictionary.

3) In addition to the rule extracted, 4 features are added to each rule.
a. last two letters of lemma
b. vowel in the last two letters. If both are vowels, the second last is chosen. (In fact, no word with consecutive vowels is found, so this setting is just a safeguard.) If no vowels is present in the last two letters, "no vowel" is used as value for this feature.
vowel_list = ['a', 'i', 'ı', 'ə', 'ö', 'ü', 'u', 'o', 'e']
c. If the vowel in the last two letters is a round vowel, defined as 
round_vowel = ['a', 'ı','u', 'o']
There are three possible labels for this feature: "round vowel", "no round vowel" and "no vowel".
d. The last letter of lemma. 

Examples of features:
training sample: 'mələk', 'mələyini', 'N;ACC;SG;PSS3S'
rule: ['k', 'yini', 'ək', 'ə', 'no round vowel','k']
        rule rule    a     b        c            d
Example of the dictionary:
defaultdict(list,
            {'N;ABL;PL': [['0', 'lərdən', 'üd', 'ü', 'no_round_vowel', 'd'],
              ['0', 'lardan', 'çu', 'u', 'round_vowel', 'u'],
              ['0', 'lərdən', 'ri', 'i', 'no_round_vowel', 'i'],
              ['0', 'lardan', 'ıq', 'ı', 'round_vowel', 'q'],
              ['0', 'lərdən', 'ək', 'ə', 'no_round_vowel', 'k'],
              ['0', 'lardan', 'an', 'a', 'round_vowel', 'n'],
              ['0', 'lardan', 'na', 'a', 'round_vowel', 'a'],
              ['0', 'lardan', 'uq', 'u', 'round_vowel', 'q'],
              ['0', 'lərdən', 'ər', 'ə', 'no_round_vowel', 'r'],
              ['0', 'lardan', 'ın', 'ı', 'round_vowel', 'n'],
              ['0', 'lərdən', 'ön', 'ö', 'no_round_vowel', 'n'],
              ['0', 'lərdən', 'iq', 'i', 'no_round_vowel', 'q'],
              ['0', 'lərdən', 'in', 'i', 'no_round_vowel', 'n'],
              ['0', 'lardan', 'od', 'o', 'round_vowel', 'd'],
              ['0', 'lərdən', 'ət', 'ə', 'no_round_vowel', 't'],
              ['0', 'lardan', 'ız', 'ı', 'round_vowel', 'z'],
              ['0', 'lardan', 'uz', 'u', 'round_vowel', 'z'],
              ['0', 'lərdən', 'nə', 'ə', 'no_round_vowel', 'ə'],
              ['0', 'lərdən', 'iz', 'i', 'no_round_vowel', 'z'],
              ['0', 'lardan', 'am', 'a', 'round_vowel', 'm'],
              ['0', 'lərdən', 'ül', 'ü', 'no_round_vowel', 'l'],
              ['0', 'lərdən', 'ən', 'ə', 'no_round_vowel', 'n'],
              ['0', 'lərdən', 'gə', 'ə', 'no_round_vowel', 'ə'],
              ['0', 'lərdən', 'əş', 'ə', 'no_round_vowel', 'ş'],
              ['0', 'lərdən', 'ic', 'i', 'no_round_vowel', 'c'],
              ['0', 'lərdən', 'çə', 'ə', 'no_round_vowel', 'ə'],
              ['0', 'lərdən', 'rə', 'ə', 'no_round_vowel', 'ə'],
              ['0', 'lərdən', 'və', 'ə', 'no_round_vowel', 'ə'],
              ['0', 'lərdən', 'iv', 'i', 'no_round_vowel', 'v'],
              ['0', 'lardan', 'az', 'a', 'round_vowel', 'z'],
              ['0', 'lardan', 'uş', 'u', 'round_vowel', 'ş'],
              ['0', 'lardan', 'ar', 'a', 'round_vowel', 'r'],
              ['0', 'lərdən', 'ki', 'i', 'no_round_vowel', 'i'],
              ['0', 'lərdən', 'pü', 'ü', 'no_round_vowel', 'ü'],
              ['0', 'lardan', 'aş', 'a', 'round_vowel', 'ş'],
              ['0', 'lardan', 'ka', 'a', 'round_vowel', 'a'],
              ['0', 'lardan', 'or', 'o', 'round_vowel', 'r'],
              ['0', 'lərdən', 'üş', 'ü', 'no_round_vowel', 'ş'],
              ['0', 'lardan', 'ab', 'a', 'round_vowel', 'b'],
              ['0', 'lardan', 'on', 'o', 'round_vowel', 'n'],
              ['0', 'lardan', 'aq', 'a', 'round_vowel', 'q'],
              ['0', 'lərdən', 'çi', 'i', 'no_round_vowel', 'i'],
              ['0', 'lardan', 'ış', 'ı', 'round_vowel', 'ş']],
...

2 Test
In test time, lemma and POS is provided.
Use POS to find corresponding rules in the dictionary, if only one rule is found, the rule is used directly to generate inflected form.
If more than one rule is found, use feature a first, if the last two letters match, use the rule. If no rule is matched with feature a, than feature b is used to search for a matched rule. Similarly, followed by feature c and d. If a feature is matched, following features are ignored.

The produced inflected form is then compared against true inflected form.
Current accuracy: 67%.

Task 3:
Given lemma and inflected form, produce POS.
1)feature extraction
Inflection rules are now used as the key for dictionary of rules.
take - took  PastTense  
(ake, ook) key
read - read Perfect
(0, 0) key
play - played PastTense
(0, ed) key
Value of the key is a list of rules, each of which consists of four features mentioned in Task 2 and POS.
defaultdict(list,
            {('0', '0'): [['da', 'a', 'round_vowel', 'a', 'N;NOM;SG'],
              ['ya', 'a', 'round_vowel', 'a', 'N;NOM;SG'],
              ['uq', 'u', 'round_vowel', 'q', 'N;NOM;SG'],
              ['ək', 'ə', 'no_round_vowel', 'k', 'N;NOM;SG'],
              ['aq', 'a', 'round_vowel', 'q', 'N;NOM;SG'],
              ['ük', 'ü', 'no_round_vowel', 'k', 'N;NOM;SG'],
              ['za', 'a', 'round_vowel', 'a', 'N;NOM;SG']],
             ('0', 'da'): [['aq', 'a', 'round_vowel', 'q', 'N;LOC;SG'],
              ['ka', 'a', 'round_vowel', 'a', 'N;LOC;SG']],
             ('0', 'də'): [['kü', 'ü', 'no_round_vowel', 'ü', 'N;LOC;SG']],
             ('0', 'dən'): [['ək', 'ə', 'no_round_vowel', 'k', 'N;ABL;SG'],
              ['şi', 'i', 'no_round_vowel', 'i', 'N;ABL;SG'],
              ['cə', 'ə', 'no_round_vowel', 'ə', 'N;ABL;SG'],
              ['üş', 'ü', 'no_round_vowel', 'ş', 'N;ABL;SG']],
             ('0', 'i'): [['əs', 'ə', 'no_round_vowel', 's', 'N;DEF;ACC;SG']],
             ('0','im'): [['ət', 'ə', 'no_round_vowel', 't', 'N;NOM;SG;PSS1S']],
             ('0', 'in'): [['əb','ə','no_round_vowel','b','N;NOM;SG;PSS2S'], 
             ['lb', 'no_vowel', 'no_vowel', 'b', 'N;NOM;SG;PSS2S']],
...

2) Test
During test time, inflection rule of test item is calculated and looked up in the dictionary of rules. If not key is matched, all the keys are looped and applied to the lemma, produced inflected form is compared with true inflected form, the first inflection with Levenshtein Distance = 1 is used. If no rule is found to give LV =1, the keys are compared again to find LV=2 . 
If a key is matched and more than one rules are present, features from a to d are used to decide which rule to use. If no feature is matched, the first rule is used by default.
Current accuracy: 56%.


