---
layout: post
title: "AI-Powered Java Code Review: My Google Gen AI Capstone"
date: 2024-04-12
tags: [Google, Kaggle, GenAI, Java, AI Code Review]
excerpt_separator: <!--more-->
---

## Exploring Generative AI: A Journey with Google

It began with a spark of curiosity and a steaming cup of coffee-the kind of morning where inspiration feels just within reach. As part of the 5-day **Generative AI Intensive Course** hosted by Google and Kaggle, I stepped into a creative challenge: to transform an idea into a working GenAI solution.

My inspiration? The often-overlooked, yet essential ritual of software development: **code reviews**. They're our safety nets, our teaching moments, our gatekeepers of quality. But let’s face it-manual code reviews can be time-consuming, inconsistent, and draining. What if we could change that?

Armed with Gemini, Google’s powerful large language model, I envisioned an assistant that never tires, never forgets the rules, and always offers constructive, structured feedback. What I built was more than a prototype-it became a glimpse into how generative AI could meaningfully support the engineering process.

<!--more-->

## Why Automate Code Reviews?

Imagine a development sprint in full swing. The backlog is full, deadlines are near, and pull requests are piling up. A senior developer opens one, scans through the code, and leaves a few rushed comments before jumping into their next meeting. It’s a familiar scene-and a risky one.

Code reviews are meant to:

- Catch bugs before they escalate
- Promote clean, maintainable code
- Share team knowledge and onboard new contributors

Yet in the hustle of modern development, reviews often become:

- Superficial
- Inconsistent
- A bottleneck for productivity

This is where generative AI enters the story-not as a replacement for humans, but as a dependable co-reviewer. One that offers:

- Immediate, consistent feedback
- Guidance aligned with best practices
- Objective insights for every developer, regardless of experience

## Prompting and Document Understanding in Action

At the core of this project are two techniques: **document understanding** and **few-shot prompting**.

### 🧠 Document Understanding

The tool reads actual `.java` files from a cloned GitHub repository:

```python
files = glob.glob(os.path.join(PROJECT_PATH, "**", "*.java"), recursive=True)
java_files = dict(zip(filenames, files))
```

These files are uploaded and passed directly into Gemini’s context:

```python
file = client.files.upload(file=filename)
contents = [prompt, file]
response = client.models.generate_content(model=MODEL, contents=contents)
```

This enables Gemini to interpret the file holistically-not just isolated code snippets.

### 🎯 Few-Shot Prompting for Review Summaries

Rather than relying on a single instruction, I use a structured, example-based approach to summarize code reviews:

```python
prompt = """
Summarize the following code review in 1-2 concise sentences.

Examples:
- Line 10: Unused variable; line 22: vague method name
-> Summary: Remove unused variable, clarify function name.

Code review: {code_review}
Summary:
"""
```

This helps control the output and ensures consistency-one of the key challenges in LLM workflows.

## A Glimpse at the Output

Here’s a snippet of what Gemini returns after analyzing a Java file:

> **Review:**  
>
> - Line 12: Method `processData()` is too generic.  
> - Line 18: Catch block should not use broad `Exception`.  
> - Line 33: Consider adding null checks for input parameter.  
>  
> **Summary:** Improve method naming, add better exception handling and null safety.  
> **Score:** 7/10  

The output is structured and actionable, and it can be stored or visualized in a DataFrame.

### 🧱 Using Gemini's Native JSON Output

To ensure that Gemini returns structured and machine-readable results, we make use of its native JSON generation mode by setting `response_mime_type = "application/json"`. This allows us to safely parse the output using `json.loads()` and ensures consistency across all reviews.

## Reflecting on the Review

To close the loop, the model even evaluates its own review using qualitative metrics:

> - Relevance: 10  
> - Correctness: 9  
> - Usefulness: 9  
> - Completeness: 8  
> - Consistency: 9  

This meta-evaluation brings an extra layer of feedback and trust in the process.

## Limitations & Future Possibilities

While the tool works well for small to medium-sized files, there are limitations:

- 📏 **Context length**: Larger files may be truncated or summarized too aggressively.
- 🔧 **Precision**: The LLM might overlook nuanced edge cases that experienced reviewers would catch.
- 🧰 **No IDE integration (yet)**: It’s still a notebook-based prototype.

But the future looks promising.

With Gemini’s evolving capabilities, it’s easy to envision:

- Real-time code review in the IDE
- Language-agnostic review pipelines
- Seamless integration into GitHub pull requests
- Team-tuned models that learn from project-specific standards

## Final Thoughts

This project showed me that GenAI isn’t just about text generation-it can be a powerful partner in software development.

With the right structure and intention, even tasks like code review can become smarter, faster, and more consistent.

Thanks for reading - and if you’re curious about the code, check out the [GitHub repo](https://github.com/MrBasM/Google-Capstone-Java-AI-Reviewer) or try it on [Kaggle - Google Capstone Java AI Reviewer](https://www.kaggle.com/code/mrbasm/google-capstone-java-ai-reviewer).

Let’s build what’s next - together.
