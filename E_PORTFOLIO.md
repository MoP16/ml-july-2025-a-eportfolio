# E-Portfolio: Evidence and Reflection on Intelligent Agents

**Name:** Baris Demirci

**Module:** Intelligent Agents

---

# Part 1: The Portfolio of Evidence

## 1. Introduction

This portfolio collates the outputs, code, designs, and analysis from the 12 units of the Intelligent Agents module. It provides the evidential foundation for the critical reflection in Part 2, demonstrating my progression from theoretical understanding to practical implementation.

---

## 2. Unit 1-4: Foundations of Intelligent Agents

### Analysis

My initial learning focused on the fundamental question: "What is an intelligent agent?" I learned to distinguish a simple program, which is a passive tool, from an autonomous agent, which is a system that perceives, reasons, and acts to achieve goals.

A key concept was the architectural debate between **Reactive** and **Deliberative** systems. Reactive architectures, like Brooks' subsumption model, argue for "intelligence without representation," where complex behavior emerges from simple, fast sensor-action rules. In contrast, deliberative architectures, like the **BDI model**, propose that agents need an internal model of the world Beliefs, Desires, and Intentions" to perform long-term, goal-directed planning.

I concluded that for a Digital Forensics Agent, which must perform a methodical, planned, and verifiable task, a purely reactive model would be insufficient. The BDI model was the clear choice, as it provides a logical structure for an agent that needs to maintain a goal and execute a multi-step plan.

### Evidence of Learning

* **First-Order Logic Exercise (Unit 2):**
    * **Natural Language:** "Every agent that has a goal and is autonomous can react to its environment."
    * **FOL Translation:** `∀x (Agent(x) ∧ HasGoal(x) ∧ Autonomous(x)) ⇒ Reactive(x)`

---

## 3. Unit 5-6: Agent Communication & Group Project Design

### Analysis & Evaluation of Group Project (Unit 6)

My group's Unit 6 report, "Building a Digital Forensics Agent," served as the essential blueprint for my individual practical implementation. The choice of the BDI model was a key strength, as it gave us a clear theoretical framework to map our agent's functions:
* **Beliefs:** The agent's knowledge of the file system.
* **Desires:** The high-level goal to "protect evidence".
* **Intentions:** The concrete plan to scan, hash, and archive.

The **UML Class Diagram** (see Figure 1 in Evidence) was invaluable. It forced us to define a modular, decoupled architecture. This design directly translated into my Python project, where each class (`FileScanner`, `Archiver`, etc.) had a single, clear responsibility. This modularity was the main reason I could code and debug each component one by one.

The **Sequence Diagram** (Figure 2) was equally critical, as it provided the step-by-step logic for the main coordinating agent. My final implementation follows this flow precisely: scan, loop, hash, archive, upload, and log.

A key strength of our design was the early justification for using **SQLite**. We argued it was "lightweight, portable, and accessible to smaller organisations", which aligned perfectly with our project goals and proved to be the right choice during implementation.

### Evidence of Individual Contribution to Group Work
![Screenshot of the UML design](evidence/11-UML%20Diagram.png)
![Screenshot of the Sequence Diagram](evidence/12-Sequence%20Diagram.png)

---

## 4. Unit 7-10: Modern AI Paradigms

### Analysis

This section of the module contrasted the classic, symbolic AI of our agent with modern statistical AI. In NLP (Units 7-8), I saw the shift from symbolic, pattern-matching methods (like Hearst's hyponyms) to statistical, distributed representations (like Mikolov's Word2Vec). This shift enabled computers to "learn" meaning from context rather than being explicitly taught rules.

The units on Deep Learning (Units 9-10) highlighted the power of adaptive algorithms. However, the discussion of AI incidents and ethics was most impactful. It framed the "black box" problem not as a technical inconvenience, but as a critical ethical and safety challenge, especially in high-stakes domains like forensics or medicine. This reinforced why our "glass box" agent "whose actions are fully auditable by design" is so appropriate for a legal context.

### Evidence of Learning

* **Constituency Parse Tree (Unit 8):**
    * **Sentence:** "The agent logs the hash."
    * **(S (NP (Det The) (N agent)) (VP (V logs) (NP (Det the) (N hash))))**

---

## 5. Unit 11-12: Practical Implementation of the Digital Forensics Agent

### Analysis

This section provides the evidence of my practical development of the 'Digital Forensics Agent'. The implementation was done in Python 3 using Google Colab, which aligned with our project goal of an accessible, low-infrastructure tool.

My code directly translates the group's UML design into four distinct classes: `FileScanner`, `Archiver`, `Logger`, and `Transmitter`. A main notebook (`main_forensics_notebook.ipynb`) acts as the coordinating `DigitalForensicsAgent`, initiating the workflow as defined in our Sequence Diagram.

The development process was iterative and involved significant debugging. The most challenging module was the `Transmitter`, which required implementing a robust error-handling and verification process to handle the file sync latency in Google Drive. This practical challenge reinforced the importance of building resilient systems that can cope with unpredictable environments.

### Evidence of Code (Snippets)

**`file_scanner.py`:**
```python
# Why glob with recursive=True? This is a highly efficient way to search
# through a directory and all its subdirectories for specific file types,
# which is essential for a thorough forensic scan.
search_pattern = os.path.join(self.scan_path, '**', file_filter)
self.found_files.extend(glob.glob(search_pattern, recursive=True))
```

**`archiver.py`:**
```python
# Why read in 4KB chunks? This method handles large files efficiently.
# It reads the file piece-by-piece instead of loading the entire
# file into memory, which would crash the agent on large evidence files.
for byte_block in iter(lambda: f.read(4096), b""):
    sha256_hash.update(byte_block)
```

**`logger.py`:**
```python
# Why use parameterized queries ('?')? This is a critical security
# practice to prevent SQL injection. It ensures that 'details' are
# treated as data, not as an executable command, protecting log integrity.
self.cursor.execute('''
    INSERT INTO audit_log (timestamp, action, status, details)
    VALUES (?, ?, ?, ?)
''', (timestamp, action, status, details))
```

**`transmitter.py`:**
```python
# Why check for directory? The agent must be robust. This check
# ensures the destination exists *before* the transfer is attempted,
# preventing a critical failure in the workflow.
try:
    os.makedirs(self.destination_url, exist_ok=True)
    print(f"Transmitter initialized. Destination verified: {self.destination_url}")
except OSError as e:
    print(f"Error creating destination directory: {e}")
```

### Evidence of Execution (Screenshots)

**Figure 1: Final Successful Run Output**
![Full successful run output in Colab](evidence/20-Final%20Output.png)

**Figure 2: Audit Trail (Log File Output)**
![Screenshot of the forensic_log.csv file](evidence/29-forensic%20log.png)

**Figure 3: Final Archive Transfer**
![Screenshot of the .zip file in the remote_server_destination folder](evidence/26-testin%20individual%20parts-1.png)

### Evidence of Testing (Screenshot)

**Figure 4: Successful Unit Test Result**
![Screenshot of the 'Ran 1 test... OK' output](evidence/28-unit%20test.png)

### README.md File

*(The full content of our README.md file)*

```markdown
# Digital Forensics Agent - README

## 1. Project Overview

This project is a practical implementation of a **Digital Forensics Agent**, an intelligent system designed to automate the collection, preservation, and transfer of digital evidence. The agent operates autonomously to scan directories, calculate cryptographic hashes to ensure integrity, create secure archives, and maintain a verifiable audit trail.

The system is built on the **Belief-Desire-Intention (BDI)** model of artificial intelligence, which structures the agent's behavior to be autonomous, reactive, and goal-directed.

## 2. Requirements

The agent is built in Python and relies on standard libraries. No special installation is required if run in Google Colab. The core libraries used are:
* `os`, `glob`, `shutil`
* `hashlib`
* `sqlite3`
* `datetime`, `time`
* `unittest`

## 3. How to Run the Agent in Google Colab

This project is designed to run directly in Google Colab using a shared folder link.

### Step 1: Open the Colab Notebook
Click the link below to open the main project notebook in Google Colab:
* https://github.com/MoP16/ml-july-2025-a-eportfolio/blob/main/main_forensics_notebook.ipynb

### Step 2: Access the Project Files
The Colab notebook needs access to the project's shared folder on Google Drive.
* https://drive.google.com/drive/folders/1rEhweZycaJNe163H7PnXPkeCFbKB_hBO?usp=sharing
* When prompted by the first code cell, click the URL to authorize Colab to access your Google Drive. Copy the authorization code and paste it into the input box.

### Step 3: Run the Main Workflow
Navigate to the final cell in the notebook, labeled "**FINAL DEFINITIVE TEST CELL**". This single cell contains the complete, end-to-end workflow.

* Click the "Play" button on this cell to execute the agent.

### Step 4: Verify the Output
After the script finishes, you can check the output folders within the shared Google Drive project directory:
* **`output/archives/`**: Will contain the timestamped `.zip` archive of the evidence.
* **`output/logs/`**: Will contain the `audit.db` database and a `forensic_log.csv` file with the complete audit trail.
* **`remote_server_destination/`**: Will contain a copy of the final `.zip` archive, simulating a successful secure transfer.

## 4. How to Run Unit Tests

To ensure the reliability of critical functions, unit tests have been provided.

1.  Open the Colab Notebook and run the setup cell to mount your drive.
2.  Find the cell containing the command `!python -m unittest discover tests`.
3.  Run this cell.
4.  The expected output is `Ran 1 test in ...s` followed by `OK`, which confirms that the hashing function is working correctly.
```

---

## 6. Conclusion

This portfolio of evidence demonstrates a comprehensive progression of learning. It spans from the foundational theories of what constitutes an intelligent agent, through the design of a multi-agent system, to the practical, tested, and documented implementation of a BDI-based agent. This journey has directly fulfilled the module's learning outcomes, providing me with the knowledge and skills to develop, deploy, and evaluate intelligent systems to solve real-world problems.

---

## 7. References (for Part 1)

* Brooks, R.A. (1991) 'Intelligence Without Representation', *Artificial Intelligence*, 47(1-3), pp. 139-159.
* Casey, E. (2011). *Digital Evidence and Computer Crime*. Academic Press.
* [Group D] (2025) 'Building a Digital Forensics Agent: Development Team Project Report'. *University of Essex, Intelligent Agents*.
* Hearst, M.A. (1992) 'Automatic acquisition of hyponyms from large text corpora', *COLING*, 2, pp. 539-545.
* Jennings, N.R. (2001). "An agent-based approach for building complex software systems." *Communications of the ACM*, 44(4), 35-41.
* Mikolov, T. et al. (2013) 'Distributed representations of words and phrases and their compositionality', *NIPS*, 1, pp. 3111-3119.
* Nasim, S.F., Ali, M.R. and Kulsoom, U. (2022) 'Artificial Intelligence Incidents & Ethics A Narrative Review', *IJTIM*, 2(2), pp. 52-64.
* Russell, S. and Norvig, P. (2016). *Artificial Intelligence: A Modern Approach (3rd ed.)*. Pearson.
* Wooldridge, M.J. (2009) *An Introduction to Multiagent Systems*. Chichester: John Wiley & Sons.

---
---

# Part 2: The Reflective Piece

## My Journey Through Intelligent Agents: A Reflective Analysis

### 1. Introduction

This reflection critically analyzes my learning journey throughout the Intelligent Agents module. It traces my development from understanding foundational AI theories to the practical implementation of a complex agent. Using the Rolfe et al. (2001) reflective model, I will analyze my experience within the group project and my individual development, focusing on the key challenges, the emotions they triggered, and the skills I have gained.

### 2. WHAT? (A description of the experience)

The module was a 12-week journey from core agent theory to modern AI applications. The central project was to design a 'Digital Forensics Agent' as a group in Unit 6, and then to individually build a working prototype in Python.

My group's design provided a comprehensive blueprint based on the BDI model. My individual task was to translate this blueprint into reality. This involved creating a modular system in Google Colab with four classes: a `FileScanner`, `Archiver`, `Logger`, and `Transmitter`.

A **critical incident** occurred during my individual development: the persistent failure of the `Transmitter` module. My code showed the transfer was failing, even though I could manually see the file in the destination. Emotionally, this was incredibly frustrating. I felt stuck and confused, as my code seemed logically correct, but the output repeatedly said "Failure: Transfer could not be verified." This debugging process took several hours, multiple attempts, and a complete re-evaluation of my assumptions.

### 3. SO WHAT? (An analysis of the experience)

**Analysis of Emotions:** The frustration I felt was a direct result of the mismatch between my mental model (how I thought the code *should* work) and the reality of the Colab/Drive environment. This frustration, however, was a necessary catalyst. It forced me to abandon simple "quick fixes" (like adding a `time.sleep(2)`) and adopt a more systematic, critical approach to debugging. It pushed me past "guessing" and into "investigating."

**Analysis of Learning:** This incident was where I learned the most.
* **SO WHAT?** My initial "fix" (the retry loop) was still based on an unproven assumption that it was a simple sync delay. It failed.
* **SO WHAT?** The *real* learning happened when I instrumented the code with detailed `[DEBUG]` logs. The output `[DEBUG] FATAL: Destination path is not a valid directory` was a revelation. The problem wasn't a *sync* issue; it was a *path* issue. I had been trying to solve the wrong problem.
* **SO WHAT?** This connects directly to my broader learning. The module wasn't just about AI theory; it was about the practical, real-world skill of **debugging complex systems**.
* **SO WHAT?** The question "Where is the AI?" was another key moment. I realized that my project's intelligence wasn't in a neural network, but in its **architecture**. By implementing the BDI model, I had created a system with **autonomy**, **reactivity**, and **pro-active, goal-directed behavior**. This directly addresses the module's core learning outcomes: I now have a deep understanding of what an "agent" truly is.

**Analysis of Collaboration:** Critically evaluating my group's Unit 6 work now, I see its immense value. Our detailed UML and Sequence diagrams were my lifeline. Without that clear, shared blueprint, my individual coding would have been directionless. My individual contribution to the group was designing the logging and archiving flow, and this prior thought process made the implementation of the `Archiver` and `Logger` classes the smoothest part of the build.

### 4. NOW WHAT? (The implications for future practice)

This project has equipped me with both theoretical knowledge and practical, real-world skills.
* **Skills Gained:** I have successfully developed, deployed, and evaluated an intelligent agent system (Learning Outcome 3). My Python skills, especially in modular design, exception handling, and testing, have improved significantly. Most importantly, I have a new, robust methodology for systematic debugging.
* **Changed Actions:** In my next project, I will not treat debugging as an afterthought. I will write unit tests *before* I write the full integration logic. I will also be much more skeptical of my own assumptions and use logging to get empirical evidence *before* attempting a fix.
* **Future Application:** I can directly apply this knowledge. The 'Digital Forensics Agent' is a real-world tool. Furthermore, the ethical lessons from Units 10 and 12 are crucial. "NOW WHAT?" means that as I continue to build "smarter" agents, I will be the one asking critical questions about bias, fairness, and accountability. This module has given me the skills not just to be a developer, but to be a responsible AI practitioner.

### 5. References (for Part 2)

* [Group D] (2025) 'Building a Digital Forensics Agent: Development Team Project Report'. *University of Essex, Intelligent Agents*.
* Nasim, S.F., Ali, M.R. and Kulsoom, U. (2022) 'Artificial Intelligence Incidents & Ethics A Narrative Review', *IJTIM*, 2(2), pp. 52-64.
* Rolfe, G., Freshwater, D. and Jasper, M. (2001). *Critical reflection in nursing and the helping professions: a user’s guide*. Basingstoke: Palgrave Macmillan.
* Wooldridge, M.J. (2009) *An Introduction to Multiagent Systems*. Chichester: John Wiley & Sons.
