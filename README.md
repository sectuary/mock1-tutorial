# 🔍 Mock Test 1 - Malware Analysis Tutorial

**A Beginner-Friendly Guide to Reverse Engineering with IDA Pro**

## 📖 About

This comprehensive tutorial teaches malware analysis for beginners preparing for exams like ST2617 or similar courses.

## 🆕 NEW: Exam Methodology Guide!

**Wondering "How do I know what to do and test?"**

Check out our new guides:
- 📘 **[METHODOLOGY.md](METHODOLOGY.md)** - Complete systematic approach
- ⚡ **[QUICK_START.md](QUICK_START.md)** - 5-step quick reference
- 🌐 **[exam-methodology.html](exam-methodology.html)** - Interactive HTML guide

## 🎯 What You Will Learn

- ✅ Analyze Windows PE executables using IDA Pro
- ✅ Understand assembly code and opcodes  
- ✅ Identify encryption algorithms (ADD cipher)
- ✅ Decrypt encrypted files
- ✅ **Know WHAT to look for and HOW to verify findings**
- ✅ Answer exam questions with citations

## 📚 Tutorial Files

| File | Description |
|------|-------------|
| **index.html** | Main tutorial (11 sections, 49KB) |
| **METHODOLOGY.md** | How to approach exam questions |
| **QUICK_START.md** | 5-step quick reference |
| **exam-methodology.html** | Interactive methodology guide |

## 🚀 Quick Start

**View the tutorial:** Open `index.html` in your browser

**Learn the method:** Read `METHODOLOGY.md` first!

**Decrypted Answer:** "THis is a mock test"

## 🎓 Key Concepts Taught

### Question Analysis Framework
1. Read question 3 times (type, keywords, evidence)
2. Static analysis BEFORE IDA (strings, xxd, stat)
3. IDA recon (Shift+F12, cross-references)
4. Collect evidence (opcodes, addresses)
5. Test & verify (calculations, sanity checks)

### Finding Answers
- **"How does it use..."** → String refs + cross-refs
- **"What criterion..."** → CMP + conditional jumps
- **"Explain how..."** → Algorithm analysis
- **"Decrypt this..."** → Algorithm + key + inverse

## 📝 Key Findings

- **Encryption:** ADD cipher `(input + key) mod 256`
- **Decryption:** SUB cipher `(encrypted - key) mod 256`  
- **Keys Used:** key0-key6 only (key7 NOT used!)
- **Selection:** Day of week (tm_wday at offset 0x18)
- **Settings.ini:** Friday = key5

## ⏰ Time Management

| Phase | Time | Action |
|-------|------|--------|
| Read All | 5 min | Skim questions |
| Static | 10 min | file, strings, xxd, stat |
| IDA Setup | 5 min | Load, analyze |
| Questions | 90 min | Systematic approach |
| Review | 10 min | Verify answers |

## 🎯 Perfect For

- ✅ Malware analysis students (ST2617, MRE courses)
- ✅ Reverse engineering beginners
- ✅ Exam preparation (2-hour mock test)
- ✅ Self-paced learning
- ✅ IDA Pro/IDA Free users

## 💡 Pro Tips

1. **Always check file dates** (reveals which key!)
2. **Verify opcodes** (01=ADD, 31=XOR)
3. **Test calculations** (encrypt answer back)
4. **Use static analysis first** (faster than IDA)
5. **Follow the methodology** (systematic approach)

## 📞 Live Tutorial

**GitHub Pages:** https://sectuary.github.io/mock1-tutorial/

**Repository:** https://github.com/sectuary/mock1-tutorial

---

**Good luck with your exam! 🍀**

*"Reverse engineering is a PROCESS, not magic!"*
