# Finite-State Transducer for Mundurukú

Bachelor Thesis of Evangelos Filippidis

International Studies Computational Linguistics

Eberhard Karls Universität Tübingen

## Installation

For testing and further implementation of the project, please install **Helsinki Finite-State Technology** (hfst) on your unix machine.
HFST includes the Xerox functions of the scripting language **xfst** that are used for implementing morphological transducers.

```
sudo apt-get update
sudo apt-get install hfst
```

## Load the project

Change your working directory to the download path of the *munduruku.fst* file.
After starting up hfst use *load stack* to import the project.
The file is the complete transducer including the morphological and phonological alternation rules.

```
hfst-xfst
hfst[0]:


hfst[0]: load stack munduruku.fst
hfst[1]: 
```

## Testing

### Functions

There are two functions to test the transducer.

Outputs the analysis form (upper-side language). Use the surface form of the word as input.

```
apply up your_word
```

Outputs the surface form (lower-side language). Use the analysis form of the word as input.

```
apply down moprhemes+your_word
```

### Testing the transducer

These are some tests that can be applied on the mundurukú.fst transducer.

```
apply up owebe
NUMBER=SING|PERSON=1+webe

apply up oaoka
NUMBER=SING|PERSON=1+Imperfective+(to_kill)

apply down NUMBER=SING|PERSON=1+Perfective+(to_kill)
oaokam

apply up ooroɡ̃
NUMBER=SING|PERSON=1+Perfective+(hunt)

apply up oxi
1SG+R1+mother

apply down 1SG+R1+arrow
odop

apply up tao
R2+leg
```


# Implementation

## LEXC Interface

Create a new file and add the ending *.lexc* to it.

This is the so-called lexicon that handles simple rules and stores our dictionary.


For the morphological analyzer, one has to declare how the analysis form should be represented. The linguist has the full controll of the morpheme representation since it is declared in the first line of the file. The *Multichar_Symbols* can be later used in the lexicons.
```
Multichar_Symbols 1SG+ 2SG+
```

First of all, lexicons are declared by using *LEXICON* and then the name of it.

The first lexicon is *Root*. Depending on the size of the transducer, words can be written inside the Root-lexicon. When working with multiple lexicons, it is require to declare them inside the Root-lexicon, otherwise they will not be recognized by the interface.
```
LEXICON Root
        Possessor_Index_Bound;
        Clitic;
```

A lexicon is build as follows:

```
LEXICON lex
form    continuation_class;
```

The form contains the upper- and lower-side language. For intance: *1SG+:o*
The continuation class has a special symbol *#* that indicates the end of the word. If we input a lexicon at this point, the interface will continue with that lexicon if the form was accepted.

```
LEXICON Possessor
1SG+ : o   Clitic;

LEXICON Clitic
webe     #;
```

## Alternation rules

The lexc interface is rather limited when it comes to morphological and phonological rules. Alternation rules can be built up on the intermediate output of the lexicon.

We are capable of declaring variables and use them later in the rules.
```
define vowel a|e|i|o|u;
```

We can have for instance a placeholder-symbol *M* from the intermediate form that can trigger a rule.
The rules name is isnM and it basically replaces (*->*) the capital *M* with a lower *m* if (*||*) it preceded by a vowel (*vowel _*).
```
define insM M −> m | | vowel_ ;
```

## Composition operator

The composition operator can combine lexicons and alternation rules into one large transducer.

First, the lexicon must be loaded into the interface
```
hfst[0]:
read lexc munduruku.lexc
hfst[1]:
```

Now we assign the lexicon to a new variable

```
hfst[1]:
define lexicon
hfst[0]:
```

The composition operator *.o.* combines the rules and the lexicon:
```
read regex lexicon .o. insM .o. insG;
```

## Save work

Is the transducer running properly and we want to save the current state of it, we can save it as follows:
```
save stack munduruku.fst
```
The binary file will be saved in the current working directory.
