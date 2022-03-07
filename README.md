# Finite-State Transducer for Mundurukú

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


