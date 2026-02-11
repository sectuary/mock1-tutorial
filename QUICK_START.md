# 🚀 Quick Start Guide

## You Asked: "How do I know what to do and test?"

**Answer:** Use the **systematic methodology**! See `METHODOLOGY.md` for the complete guide.

## 5-Step Quick Method

### 1️⃣ **Read Question 3 Times**
- 1st: Identify type (How? What? Explain? Decrypt?)
- 2nd: Extract keywords (file names, technical terms)
- 3rd: Plan evidence needed (opcodes? calculations?)

### 2️⃣ **Static Analysis FIRST (Before IDA!)**
```bash
file binary                    # File type
strings binary | grep keyword  # Find keywords
xxd encrypted_file | head      # View hex
stat encrypted_file            # CHECK DATE!
```

### 3️⃣ **IDA Quick Recon**
- `Shift+F12` - Strings window
- Search for keywords from question
- `X` - Cross-references (where is it used?)

### 4️⃣ **Collect Evidence**
- Note opcode addresses
- Identify function calls  
- Trace data flow

### 5️⃣ **Test & Verify**
- Manual calculation
- Python script
- Does result make sense?

---

## Common Question Types

| Question Pattern | What to Look For | First Step |
|-----------------|------------------|------------|
| "How does it use..." | String refs, function calls | Shift+F12 → search term |
| "What criterion..." | CMP, jumps, offsets | Find decision logic |
| "Explain how..." | Algorithm, loop, math ops | Find function → analyze |
| "Decrypt this..." | Algorithm + key | Find algo → find key → inverse |

---

## Example: Mock Test 1

**Q:** "How does mock_1.exe use key files?"

**Steps:**
1. **Static:** `strings mock_1.exe | grep key` → See key0-key6 (NOT key7!)
2. **IDA:** Shift+F12 → "key0" → X → Jump to usage
3. **Find:** `call _stat` at 0x004014CB
4. **Test:** Count strings (7), check switch range (0-6)
5. **Answer:** Uses stat() to check sizes, selects via switch 0-6

---

## Verification Checklist ✅

Before submitting:
- [ ] Cited specific opcode addresses?
- [ ] Verified opcodes in IDA?
- [ ] Tested calculations manually?
- [ ] Result makes sense? (ASCII? Logical?)
- [ ] Answered everything asked?

---

## When Stuck

Ask yourself:
- "What function would do this?" (time? file I/O?)
- "What's the value range?" (0-6? 0-255?)
- "Does this result make sense?"
- "Can I verify by re-encrypting?"

---

**For full methodology:** See `METHODOLOGY.md`

**Main tutorial:** See `index.html`

**Good luck! 🍀**
