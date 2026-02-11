# 🎯 Exam Methodology: How to Know What to Do and Test

## The Core Problem

**Student Question:** "How do I know WHAT to look for in IDA and HOW to verify my findings?"

This guide provides a **systematic methodology** for approaching ANY reverse engineering exam question.

---

## 📋 Step 1: Question Analysis Framework

### Before Opening IDA, Read the Question 3 Times:

**First Read - Identify Question Type:**
- "How does it use..." → Find usage pattern + data flow
- "What is the criterion..." → Find decision logic
- "Explain how..." → Find algorithm/process
- "Decrypt/decode..." → Find algorithm + apply inverse
- "Describe artifacts..." → Conceptual knowledge

**Second Read - Extract Keywords:**
- **File names:** "key0", "Settings.ini", "input.txt"
- **Technical terms:** "encrypted", "selection", "criterion"
- **Requirements:** "cite opcodes", "support your claims"

**Third Read - Plan Evidence:**
- Specific opcodes with addresses?
- Function names?
- String references?
- Control flow patterns?
- Calculations/examples?

---

## 🔍 Step 2: Question Type Patterns

### Type 1: "How does the program use X?"

**Example:** "How does mock_1.exe use the key files?"

| Evidence Needed | Where to Find It | IDA Action |
|-----------------|------------------|------------|
| String references | Strings window | Shift+F12 → Search "key" |
| Where strings are used | Cross-references | Double-click → X |
| File operations | Function calls | Look for: fopen, stat, fsize |
| Selection logic | Control flow | Look for: switch, cmp |

**Testing Strategy:**
- ✅ Count string references (matches # keys?)
- ✅ Check if ALL keys referenced or only some
- ✅ Trace from string → function → usage
- ✅ Look for patterns (loop? switch?)

---

### Type 2: "What is the criterion/condition?"

**Example:** "What is the key selection criterion?"

| Evidence Needed | Where to Find It | IDA Action |
|-----------------|------------------|------------|
| Input source | Function calls | Look for: time, rand, argv |
| Comparison values | CMP instructions | Find: cmp eax, VALUE |
| Data structure access | Memory offsets | Look for: [eax+0xNN] |
| Range of values | Jump conditions | Find: ja, jb, jl, jg |

**Testing Strategy:**
- ✅ Check ranges: cmp eax, 6 → values are 0-6
- ✅ Lookup structures: offset 0x18 → check struct tm
- ✅ Test with calculator: does range match # cases?
- ✅ Cross-reference docs: what does tm_wday mean?

---

### Type 3: "Explain the algorithm/process"

**Example:** "Explain how input.txt is encrypted"

| Evidence Needed | Where to Find It | IDA Action |
|-----------------|------------------|------------|
| Main loop | Function with string | Shift+F12 → "Encrypting" → X |
| Input reading | File I/O calls | fgetc, fread, getc |
| Encryption operation | Math in loop | add, xor, sub, rol |
| Output writing | File I/O calls | fputc, fwrite |

**Testing Strategy:**
- ✅ Identify operation: ADD (01), XOR (31), SUB (29)?
- ✅ Test manually: first byte + key = result?
- ✅ Check special handling: EOF? rewind? padding?
- ✅ Verify with source (if available)

---

### Type 4: "Decrypt/decode this"

**Example:** "Decrypt Settings.ini"

| Step | What to Find | How to Test |
|------|--------------|-------------|
| 1. Find algorithm | Encryption operation | From Type 3 |
| 2. Find key | Which key was used? | Check file timestamp |
| 3. Determine inverse | Reverse operation | ADD→SUB, XOR→XOR |
| 4. Decrypt first byte | Test with one byte | Should be ASCII (0x20-0x7E) |
| 5. Decrypt all | Full message | Should be readable English |

**Testing Strategy:**
- ✅ First byte test: decrypt byte 0, should be letter
- ✅ ASCII range: all bytes 32-126?
- ✅ Re-encrypt test: encrypt result = original?
- ✅ Sanity check: does message make sense?

---

## 🔄 Step 3: Universal Analysis Workflow

```
1. READ QUESTION 3 TIMES
   ↓
2. STATIC ANALYSIS (NO IDA!)
   file, strings, xxd, stat
   ↓
3. OPEN IDA - QUICK RECON
   Shift+F12, Shift+F3
   ↓
4. FIND RELEVANT CODE
   X (cross-references)
   ↓
5. ANALYZE & COLLECT EVIDENCE
   Note addresses, trace data flow
   ↓
6. TEST YOUR FINDINGS
   Manual calc, Python, verify
   ↓
7. WRITE ANSWER
   Cite opcodes, show work
```

---

## ✅ Step 4: Verification Checklist

Before submitting, verify:

- [ ] **Evidence completeness:** Cited specific addresses?
- [ ] **Opcode accuracy:** Verified opcodes in IDA?
- [ ] **Logic consistency:** Explanation follows code flow?
- [ ] **Calculation correctness:** Tested with actual values?
- [ ] **Range validation:** Values make sense?
- [ ] **Cross-reference check:** Verified with other evidence?
- [ ] **English readability:** If decrypting, is result readable?
- [ ] **Question requirements:** Answered everything asked?

---

## 🎯 Applying to Mock Test 1

### Question 1a Example: "How does mock_1.exe use key files?"

**Step 1: Question Analysis**
- Type: "How does it use..." → Usage pattern
- Keywords: "key files", "key0-key7"
- Evidence: String references, function calls, opcodes

**Step 2: Static Analysis First**
```bash
$ strings mock_1.exe | grep key
key0
key1
key2
key3
key4
key5
key6
# FINDING: Only 7! key7 missing!
```

**Step 3: IDA Analysis**
1. Shift+F12 → Find "key0"
2. Double-click → Press X → Jump to usage
3. See: `call _stat` at 0x004014CB
4. Find switch with `cmp eax, 6`

**Step 4: Testing**
- ✅ Counted 7 strings (key0-key6)
- ✅ key7 NOT in code
- ✅ Found stat() at 0x004014CB
- ✅ Switch handles 0-6 (7 cases)
- ✅ Logic: 7 days = 7 keys

---

### Question 2a Example: "Explain encryption method"

**Step 1: Find Function**
- Shift+F12 → "Encrypting"
- Press X → Jump to function

**Step 2: Identify Operation**
```
0040145F    01 C2    add edx, eax
            ^^^^
            Opcode 01 = ADD (NOT 31=XOR!)
```

**Step 3: Manual Test**
```
Input:  'T' = 84 (0x54)
Key:    197 (0xC5)
Calc:   84 + 197 = 281
Result: 281 mod 256 = 25 (0x19)

Verify:
$ xxd -l 1 Settings.ini
00000000: 19    ← MATCHES! ✓
```

---

## ⚠️ Common Mistakes & How to Avoid

| Mistake | Why It Happens | How to Avoid |
|---------|----------------|--------------|
| Confusing ADD/XOR | Both common | Check opcode: 01=ADD, 31=XOR |
| Wrong key file | Didn't check date | stat filename \| grep Modify |
| Forgot mod 256 | Ignored overflow | Byte arithmetic wraps at 256 |
| No opcode citations | Rushed | "cite" = must provide addresses |
| Assumed key7 used | Didn't verify | Shift+F12 → count strings |

---

## 🌳 Quick Decision Tree

```
START: Read question
  ↓
Mentions FILE NAME?
  YES → Shift+F12, search filename
  NO → Continue

Asks "HOW" something works?
  YES → Find function, analyze algorithm
  NO → Continue

Asks "WHAT" determines something?
  YES → Look for CMP, jumps, switches
  NO → Continue

Asks to DECRYPT/DECODE?
  YES → Algorithm → Key → Inverse
  NO → Continue

Asks for KNOWLEDGE?
  YES → Use study notes, no IDA
  NO → Re-read question!
```

---

## ⏰ Time Management (2-hour exam)

| Phase | Time | What to Do |
|-------|------|------------|
| Read All Questions | 5 min | Skim, identify easy ones |
| Static Analysis | 10 min | file, strings, xxd, stat |
| IDA Setup | 5 min | Load, wait, Shift+F12 |
| Question 1 | 20 min | Strings, cross-refs |
| Question 2 | 25 min | Algorithm, opcodes |
| Question 3 | 30 min | Decrypt, calculate |
| Question 4 | 15 min | Write from memory |
| Review | 10 min | Verify everything |

---

## 💡 Final Pro Tips

### 1. Always Start with Static Analysis
```bash
# FAST and gives context BEFORE IDA
file binary
strings binary | grep keyword
xxd encrypted_file | head
stat encrypted_file  # CHECK THE DATE!
```

### 2. Use Question Keywords as Search Terms
If question mentions "key files", search "key" in strings!

### 3. Verify with Multiple Methods
- **Logical:** Does it make sense? (7 days → 7 keys)
- **Mathematical:** Calculate manually
- **Practical:** Test with Python
- **Cross-check:** Compare with source (if available)

### 4. Draw Diagrams
```
time() → localtime() → [+0x18] → tm_wday → 0-6 → key0-key6
```

### 5. When Stuck, Ask:
- "What function would do this?" (time? rand? input?)
- "What's the range?" (0-6? 0-255?)
- "Does result make sense?" ('T' reasonable first letter?)
- "What if I encrypt my answer back?" (Should match!)

---

## 🏋️ Practice Exercise

**Question:** "Given Settings.ini was created on Tuesday, which key file is used?"

**Solution:**

1. **Question Type:** "Which" → Selection
2. **Keywords:** "Tuesday", "which key"
3. **What We Know:**
   - From Q1b: Selection based on tm_wday
   - tm_wday: 0=Sun, 1=Mon, 2=Tue...
   - Tuesday = 2
4. **Answer:** **key2**

**Verification:**
- ✅ Tuesday = tm_wday value 2
- ✅ Day 2 maps to key2
- ✅ Consistent with switch-case

---

## 🎓 Summary: The Systematic Approach

**Every Question Follows:**

```
READ (3 times)
  ↓
ANALYZE (type, keywords, evidence)
  ↓
FIND (static analysis, then IDA)
  ↓
VERIFY (calculate, test, cross-check)
  ↓
ANSWER (cite opcodes, show work)
```

**Remember:** Reverse engineering is a PROCESS, not magic! Follow the methodology, and you'll know exactly what to do.

---

**Good luck with your exam! 🍀**
