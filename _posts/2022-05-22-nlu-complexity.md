---
layout: distill
title: Transformers are Linear-Bounded Automata
description: Interesting implications of considering natural language from the perspective of computational complexity theory
date: 2022-05-22

authors:
  - name: Daniel C.H Tan
    url: "daniel-ch-tan.github.io"
    affiliations:
      name: UCL CS, A*STAR

bibliography: 2022-01-13-motor-skill-url.bib

# Optionally, you can add a table of contents to your post.
# NOTES:
#   - make sure that TOC names match the actual section names
#     for hyperlinks within the post to work correctly.
#   - we may want to automate TOC generation in the future using
#     jekyll-toc plugin (https://github.com/toshimaru/jekyll-toc).
toc:
  - name: Introduction
  - name: Transformers as LBAs
  - name: Natural Language as a Context-Free Grammar
    # if a section has subsections, you can add them as follows:
    # subsections:
    #   - name: Example Child Subsection 1
    #   - name: Example Child Subsection 2
  # - name: Citations
  # - name: Footnotes
  # - name: Code Blocks
  # - name: Layouts
  # - name: Other Typography?

# Below is an example of injecting additional post-specific styles.
# If you use this post as a template, delete this _styles block.
_styles: >
  .fake-img {
    background: #bbb;
    border: 1px solid rgba(0, 0, 0, 0.1);
    box-shadow: 0 0px 4px rgba(0, 0, 0, 0.1);
    margin-bottom: 12px;
  }
  .fake-img p {
    font-family: monospace;
    color: white;
    text-align: left;
    margin: 12px 0;
    text-align: center;
    font-size: 16px;
  }

---

## Introduction

Recently, I've been thinking about natural language understanding and its relationship to complexity theory.

First some definitions. A *language* is a set of strings over an alphabet. A *grammar* can be abstractly thought of as an algorithm that parses a string and *decides* whether it is in the language or not. A grammar always induces a corresponding language
 
The [Chomsky hierarchy](https://en.wikipedia.org/wiki/Chomsky_hierarchy) provides a classification of languages. The simplest are regular expressions and context-free grammars, which are induced by finite-state automata and pushdown automata respectively. 

I'm more interested in the two more complex ones, namely *context-sensitive grammers* and *recursively enumerable* languages. The latter is induced by a Turing Machine (TM) and the former by a Linear-Bounded Automaton (LBA). An LBA is simply a TM which is constrained to use memory linear in its input, as opposed to a TM which can use infinite memory. 

## Transformers as LBAs

I think this is interesting because transformers are LBAs. When processing a sequence, a transformer performs computations on a sequence of input token embeddings (linear in input size) and its own weights (constant in input size). Hence transformers are LBAs. 

Contrast this to RNNs, which have a *constant* memory size (the value of the hidden state) to process sequence inputs. 

## Natural Language as a Context-Free Grammar

Recall that CFGs are exactly the class of languages decided by LBAs, and transformers are LBAs. Therefore, the empirical observation that transformers do so well on natural language tasks may be an indication that natural language can be modelled very well as a CFG. 

Some work on understanding language as a CFG: [DELPH-IN](https://github.com/delph-in/docs/wiki/)

